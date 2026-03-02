# Code Review Agent Team Workflow

## Overview

A structured, multi-stage code review pipeline implemented as a Claude Code plugin. The developer invokes `/review-cycle`, which creates an agent team that manages the entire review-adjudicate-plan-execute workflow. The developer never leaves the team — they interact with the team lead and switch to individual stage leads as needed.

The input is existing code or changes — however they were produced. The output is a reviewed, adjudicated, and fixed codebase.

Each stage boundary is a context clear. Each stage lead starts fresh, reading in only the handoff artifact from the previous stage. This mirrors the Claude CLI's "clear context and proceed" pattern — the conversation that produced an artifact served its purpose; the artifact is the handoff, not the conversation.

---

## Architecture

```txt
┌──────────────────────────────────────────────────────────────────────────────────────┐
│                                DEVELOPER (human)                                     │
│  Invokes /review-cycle. Interacts with Team Lead and Stage Leads directly.           │
│  Switches between teammates as needed at review checkpoints.                         │
└───────┬──────────────────┬──────────────────┬──────────────────┬─────────────────────┘
        │                  │                  │                  │
        │ starts           │ switches to      │ approves plan    │ verifies results
        ▼                  ▼                  ▼                  ▼
┌───────────────┐  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐
│  TEAM LEAD    │  │ REVIEW LEAD   │  │ ADJUDICATION  │  │ PLANNING LEAD │  │ EXECUTION     │
│  (orchestr.)  │  │ (teammate)    │  │ LEAD          │  │ (teammate)    │  │ LEAD          │
│               │  │               │  │ (teammate)    │  │               │  │ (teammate)    │
│  Manages the  │  │  Stage 1      │  │               │  │  Stage 3      │  │               │
│  workflow.    │  │  Fresh ctx.   │  │  Stage 2      │  │  Fresh ctx.   │  │  Stage 4      │
│  Spawns stage │  │  Narrow.      │  │  Fresh ctx.   │  │  Wide.        │  │  Fresh ctx.   │
│  leads.       │  │               │  │  Wide.        │  │               │  │  Wide.        │
│  Directs the  │  │  Opus         │  │               │  │  Opus         │  │               │
│  developer.   │  │               │  │  Opus         │  │               │  │  Opus         │
│               │  │  No sub-      │  │               │  │  Optional     │  │               │
│  Opus         │  │  agents.      │  │  Spawns       │  │  planning     │  │  Spawns       │
│               │  │               │  │  investigator │  │  sub-agents   │  │  execution    │
│               │  │               │  │  sub-agents   │  │  (Sonnet)     │  │  sub-agents   │
│               │  │               │  │  (Sonnet)     │  │               │  │  (Sonnet)     │
└───────────────┘  └───────────────┘  └───────────────┘  └───────────────┘  └───────────────┘
                          │                  │                  │                  │
                       produces           produces           produces          updates
                          │                  │                  │                  │
                          ▼                  ▼                  ▼                  ▼
                   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
                   │  review-    │──▶│adjudication- │   │  fix-plan   │──▶│  fix-plan   │
                   │  findings   │   │  report.md   │   │  .md        │   │  .md        │
                   │  .md        │   └──────┬───────┘   └─────────────┘   │  (updated   │
                   └─────────────┘          │                ▲            │  w/progress) │
                                            ▼                │            └─────────────┘
                                     ┌─────────────┐        │
                                     │ approved-   │────────┘
                                     │ findings.md │
                                     └─────────────┘
```

---

## Team Structure

This workflow uses **one agent team** for the entire pipeline. The team lead orchestrates, and each stage is owned by a **stage lead teammate** with fresh context. Stage leads delegate work to **sub-agents** where appropriate — they do not perform computationally heavy work directly.

| Role | Type | Model | Stage | Context | Responsibilities |
| ---- | ---- | ----- | ----- | ------- | ---------------- |
| Team Lead | Agent team lead | Opus | All | Persistent | Orchestrate workflow, spawn stage leads, direct developer between teammates, manage transitions |
| Review Lead | Teammate | Opus | 1 | Fresh, narrow (target files only) | Perform code review with constrained scope |
| Adjudication Lead | Teammate | Opus | 2 | Fresh, wide (full codebase) | Investigate findings via sub-agents, discuss with developer, produce approved list |
| Planning Lead | Teammate | Opus | 3 | Fresh, wide (full codebase) | Produce fix plan (no code changes) |
| Execution Lead | Teammate | Opus | 4 | Fresh, wide (full codebase) | Assign fixes to sub-agents, track progress, coordinate |
| Investigator (×N) | Sub-agent of Adjudication Lead | Sonnet | 2 | Focused (one finding + relevant code) | Investigate a single review finding in codebase context |
| Planner (×N, optional) | Sub-agent of Planning Lead | Sonnet | 3 | Scoped to fix group | Plan independent fix groups in parallel |
| Executor (×N) | Sub-agent of Execution Lead | Sonnet | 4 | Scoped to assigned files | Implement a specific fix or fix group |

### Why This Structure

- **Every stage starts with fresh context.** Each stage lead reads in only the handoff artifact it needs. This mirrors the Claude CLI's "clear context and proceed" pattern — the conversation that produced an artifact served its purpose. The artifact is the handoff, not the conversation.
- **Team lead stays thin.** It orchestrates and directs — it doesn't accumulate heavy context from reviewing, investigating, or executing.
- **Stage leads own their stage's context.** The Review Lead's context is deliberately narrow. The Adjudication Lead's context is deliberately wide. Each is scoped to what that cognitive task requires.
- **Sub-agents keep stage lead context clean.** Investigation, planning, and execution happen in sub-agents that report results back without bloating the stage lead's context window.
- **Developer can talk to any teammate.** Agent teams support switching between the lead and teammates directly. The developer talks to the Team Lead for workflow control and to stage leads for substantive discussion (especially the Adjudication Lead during Stage 2).
- **Sub-agents (not nested teams) for delegation.** Work within each stage reports back to the stage lead — no peer-to-peer coordination needed between sub-agents. Stage leads coordinate on behalf of their sub-agents if issues arise.
- **Review Lead has no sub-agents.** A single coherent review pass preserves the ability to spot relationships across findings. Splitting the review across sub-agents would fragment that holistic view.
- **Stage leads notify the Team Lead on completion.** Teammates automatically notify the lead when they go idle. The Team Lead then passes the handoff artifact to the next stage lead and tells the developer which teammate to switch to. Stage leads do not communicate with each other directly — all transitions go through the Team Lead. This keeps stage leads decoupled from the pipeline structure and avoids unnecessary inter-teammate messaging costs.

---

## Workflow

### Initialization

**Developer invokes `/review-cycle`.** The skill fires and guides the developer through input collection.

1. **The `/review-cycle` skill collects inputs from the developer:**
   - **What to review** (one of):
     - A git commit or range of commits (e.g., `abc123..def456`)
     - A Merge Request / Pull Request reference
     - An explicit list of file paths
     - A single file
   - **Focus areas and review criteria** — what the reviewer should pay attention to. Example: *"Consider what a developer implementing a new client with this spec would need. Compare to how Stripe implements their OpenAPI spec as a best-practice benchmark."*
   - **Additional context** (optional) — any background the reviewer should have, kept minimal by design.
   - **The original problem being solved** — a brief description or link to the issue/ticket, used in Stage 2 for context when determining finding validity.

   The Team Lead creates the agent team, spawns the stage lead teammates, and begins the pipeline.

> **Human interaction:** The developer provides all inputs conversationally, guided by the skill. The skill ensures nothing critical is missed before kicking off the review.

---

### Stage 1 — Review

**Owner:** Review Lead (teammate)
**Context:** Fresh. Narrow by design — sees only the target files/changes. Does *not* have full codebase context. This naivety is intentional, mimicking an external reviewer looking at a diff.
**Sub-agents:** None. The Review Lead performs the review directly.

1. **Team Lead passes the developer's input to the Review Lead.** The Review Lead's instructions explicitly constrain its scope: *review only the provided files/changes; do not explore or scan the broader codebase.*

2. **Review Lead performs the review:**
   - Evaluates the code against the developer's stated focus areas.
   - Also flags anything else that stands out regarding correctness, readability, maintainability, or potential bugs — the focus items guide but do not constrain.
   - For each finding, documents: an ID, location (file + line/range), severity, description, and explanation of why it matters.

3. **Handoff artifact produced:** `review-findings.md`
   - Structured list of all findings with enough detail for each to be investigated independently.
   - Review Lead goes idle; Team Lead is automatically notified.

> **Human interaction:** None during this stage. The developer provided input during initialization.

---

### Stage 2 — Adjudicate

**Owner:** Adjudication Lead (teammate)
**Context:** Fresh. Wide — has access to the full codebase, the original problem/issue being solved, and the review findings from Stage 1.
**Sub-agents:** Investigator sub-agents (Sonnet), one per finding.

1. **Team Lead passes `review-findings.md` and the original problem context to the Adjudication Lead.**

2. **Adjudication Lead spawns an investigator sub-agent (Sonnet) for each finding:**
   - Each sub-agent receives: the specific finding, access to relevant codebase areas, and context about the original problem.
   - The sub-agent determines whether the finding is valid, partially valid, or not applicable given the full context.
   - The sub-agent reports back with: its verdict, reasoning, tradeoffs, and what a fix would look like if warranted.

3. **Adjudication Lead compiles results** into a single document: each original finding, the investigation reasoning, and a recommendation (fix / don't fix / needs discussion).

4. **Handoff artifact produced:** `adjudication-report.md`

5. **Team Lead notifies the developer that the adjudication report is ready and tells them to switch to the Adjudication Lead** (via Shift+Down in in-process mode, or clicking the pane in split-pane mode).

6. **Developer and Adjudication Lead review the report together:**
   - The Adjudication Lead walks through each item conversationally — summarizing the finding, the investigation, and its recommendation.
   - The developer agrees, disagrees, or asks for deeper investigation (which may spawn additional sub-agents).
   - The developer makes final calls on each item.

7. **Handoff artifact produced:** `approved-findings.md`
    - The developer-approved list of findings that need to be addressed.
    - Each item includes: the original finding, adjudication reasoning (as approved/modified by the developer), and any developer notes on what the fix should accomplish.
    - Adjudication Lead goes idle; Team Lead is automatically notified.

> **Human interaction:** Steps 8–9. This is the primary human checkpoint. The developer actively shapes the final list through conversation with the Adjudication Lead. This stage does not end until the developer is satisfied.

---

### Stage 3 — Plan

**Owner:** Planning Lead (teammate)
**Context:** Fresh. Reads in `approved-findings.md`. Has full codebase access.
**Sub-agents:** Optional planning sub-agents (Sonnet) for parallelizing independent fix groups.

1. **Team Lead passes `approved-findings.md` to the Planning Lead.**

2. **Planning Lead produces a fix plan:**
    - Groups related findings that should be addressed together.
    - Identifies findings that are independent and can be addressed in parallel.
    - For each finding or group, outlines: what changes, in which files, why, and any risks or dependencies.
    - If fix groups are independent, the Planning Lead may spawn planning sub-agents (Sonnet) to plan them in parallel. All sub-plans are consolidated into the single output document.

3. **Team Lead notifies the developer that the plan is ready and tells them to switch to the Planning Lead.**

4. **Developer reviews the plan:**
    - Approves, requests changes, or asks for investigation on specific points.

5. **Handoff artifact produced:** `fix-plan.md`
    - The consolidated, approved execution plan.
    - Each fix item includes a status field (`[ ] pending`) for progress tracking in Stage 4.
    - Planning Lead goes idle; Team Lead is automatically notified.

> **Human interaction:** Step 14. The developer approves the plan before any code changes are made.

---

### Stage 4 — Execute

**Owner:** Execution Lead (teammate)
**Context:** Fresh. Reads in `fix-plan.md`. Has full codebase access.
**Sub-agents:** Execution sub-agents (Sonnet), one per fix or fix group.

1. **Team Lead passes `fix-plan.md` to the Execution Lead.**

2. **Execution Lead assigns fixes to sub-agents:**
    - Independent fixes are assigned to separate sub-agents for parallel execution.
    - Related or dependent fixes are assigned to a single sub-agent or executed sequentially.
    - Each sub-agent owns a distinct set of files to avoid write conflicts.

3. **Sub-agents execute their assigned fixes,** reporting results back to the Execution Lead.

4. **Execution Lead updates `fix-plan.md` as fixes complete:**
    - Marks each item's status (e.g., `[x] complete`).
    - This makes the plan a **live progress document**. If the session is interrupted, a new session can read `fix-plan.md`, see what's done and what remains, and pick up where it left off.
    - The Execution Lead coordinates on behalf of its sub-agents if any conflicts or dependencies arise between fixes.

5. **Execution Lead goes idle; Team Lead is automatically notified.** Team Lead notifies the developer that execution is complete.

> **Human interaction:** Post-execution. Developer verifies the changes. If issues are found, they can feed back into Stage 1 for another review cycle.

---

## Handoff Artifacts

| Artifact | Produced after | Produced by | Consumed by | Contents |
| -------- | -------------- | ----------- | ----------- | -------- |
| `review-findings.md` | Stage 1, step 3 | Review Lead | Adjudication Lead (Stage 2) | Structured findings: ID, location, severity, explanation |
| `adjudication-report.md` | Stage 2, step 7 | Adjudication Lead | Developer (in-session with Adjudication Lead) | Findings with investigation reasoning and recommendations |
| `approved-findings.md` | Stage 2, step 10 | Developer + Adjudication Lead | Planning Lead (Stage 3) | Developer-approved findings with reasoning and developer notes |
| `fix-plan.md` | Stage 3, step 15 | Planning Lead | Execution Lead (Stage 4) | Execution plan with status tracking per item |

All artifacts are written to disk:

- **Any stage can be re-run independently.** Bad plan? Rerun Stage 3 from `approved-findings.md`. Disagree with adjudication? Rerun Stage 2 from `review-findings.md`.
- **Sessions can be interrupted and resumed.** `fix-plan.md` with progress checkmarks makes Stage 4 resilient.
- **Artifacts are human-readable.** The developer can inspect any of them directly outside the agent session.

---

## Developer Experience

From the developer's perspective, the workflow is:

1. **`/review-cycle`** — Answer a few guided questions about what to review, what to focus on, and what problem was originally being solved. The Team Lead creates the team and kicks off Stage 1.
2. **Wait** — The Review Lead performs the review. The Team Lead notifies the developer when `review-findings.md` is ready.
3. **Switch to Adjudication Lead** — The Team Lead tells the developer to switch to the Adjudication Lead (Shift+Down). Walk through each finding from the adjudication report together. Discuss, approve, reject, or ask for further investigation. This is the most hands-on part of the workflow and takes as long as the developer needs.
4. **`approved-findings.md` produced** — Once the developer is satisfied, the Adjudication Lead writes the approved findings and goes idle. The Team Lead is notified and passes the artifact to the Planning Lead.
5. **Wait** — The Planning Lead produces the fix plan. The Team Lead notifies the developer when the plan is ready.
6. **Switch to Planning Lead** — The Team Lead tells the developer to switch to the Planning Lead. Review the fix plan. Approve, request changes, or ask questions. Approve when satisfied.
7. **Wait** — The Execution Lead and its sub-agents implement the fixes. Progress is tracked in `fix-plan.md`. The Team Lead notifies the developer when execution is complete.
8. **Verify** — Review the changes. If issues are found, feed back into Stage 1 for another review cycle.

The developer never manually creates sessions, clears context, or manages handoff files. The team handles all of that.
