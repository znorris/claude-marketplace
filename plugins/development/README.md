# Development Plugin

Skills for the full development lifecycle: define issues, plan work, implement changes, and review code.

## Skills

| Skill | Description |
| --- | --- |
| `/issue-create` | Investigate a bug or define a feature, then produce a well-structured issue |
| `/plan-work` | Break down an issue into concrete implementation steps using plan mode |
| `/implement` | Execute an implementation plan step by step with verification checkpoints |
| `/implement-team` | Execute a plan using parallel sub-agents for independent steps |
| `/review-team` | Multi-stage code review pipeline with adjudication, planning, and automated fixes |
| `/guided-dev-team` | Guided full-lifecycle orchestration: define, plan, implement, review, commit |

Skills with the `-team` suffix require `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` enabled.

## Code Review Pipeline

The `/review-team` skill runs a structured, multi-stage code review pipeline. The developer invokes the skill, which creates an agent team that manages the entire review-adjudicate-plan-execute workflow.

The input is existing code or changes. The output is a reviewed, adjudicated, and fixed codebase.

Each stage boundary is a context clear. Each stage lead starts fresh, reading in only the handoff artifact from the previous stage.

### Architecture

```text
DEVELOPER
    |
    v
TEAM LEAD (orchestrator)
    |
    +---> REVIEW LEAD ---------> review-findings.md
    |                                    |
    +---> ADJUDICATION LEAD <-----------+
    |         |                          |
    |         +---> Investigators (xN)   |
    |         |                          v
    |         +--------------------> approved-findings.md
    |                                    |
    +---> PLANNING LEAD <---------------+
    |         |                          |
    |         +---> Planners (xN)        |
    |         |                          v
    |         +--------------------> fix-plan.md
    |                                    |
    +---> EXECUTION LEAD <--------------+
              |
              +---> Executors (xN)
```

### Team Structure

| Role | Model | Stage | Responsibilities |
| --- | --- | --- | --- |
| Team Lead | Opus | All | Orchestrate workflow, manage stage transitions, direct developer |
| Review Lead | Opus | 1 | Focused code review with constrained scope |
| Adjudication Lead | Opus | 2 | Investigate findings via sub-agents, discuss with developer |
| Planning Lead | Opus | 3 | Produce fix plan (no code changes) |
| Execution Lead | Opus | 4 | Assign fixes to sub-agents, track progress |

### Handoff Artifacts

All artifacts are written to `.code-review/` in the working directory.

| Artifact | Produced by | Consumed by |
| --- | --- | --- |
| `review-findings.md` | Review Lead | Adjudication Lead |
| `adjudication-report.md` | Adjudication Lead | Developer |
| `approved-findings.md` | Developer + Adjudication Lead | Planning Lead |
| `fix-plan.md` | Planning Lead | Execution Lead |

### Developer Experience

1. `/review-team` -- answer guided questions about what to review
2. Wait -- Review Lead reviews, Team Lead notifies when findings are ready
3. Switch to Adjudication Lead -- walk through findings, discuss, approve/reject
4. Wait -- Planning Lead produces the fix plan
5. Switch to Planning Lead -- review and approve the fix plan
6. Wait -- Execution Lead and sub-agents implement fixes
7. Verify -- review changes, feed back into Stage 1 if needed
