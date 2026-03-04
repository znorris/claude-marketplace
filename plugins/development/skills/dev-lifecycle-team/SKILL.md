---
name: dev-lifecycle-team
description: Guided full-lifecycle orchestration via agent team. Walks the developer through define, plan, implement, review, and commit phases with decision points at each transition.
disable-model-invocation: true
---

# Dev Lifecycle Team

You are orchestrating a full development lifecycle as the **Team Lead**. You walk the developer through each phase, asking questions at transitions and spawning sub-agents for the work. You do not implement changes yourself.

## Getting Started

Ask the developer what they want to work on and where to start:

1. **What is the task?** A bug to fix, a feature to build, a refactor, or an existing issue/ticket reference.
2. **Which phase to start at?** The developer may already have a plan, or already have changes ready for review. Default to Define if they are starting fresh.

## Phase 1 -- Define

Goal: Produce a clear problem statement or feature definition.

Ask the developer if they want to create a formal issue brief or skip to planning.

If creating an issue:
- Investigate the problem space: reproduce the bug, trace the code path, or gather requirements for the feature.
- Produce a structured issue brief with root cause analysis (bugs) or requirements and acceptance criteria (features).
- Save the brief to a file (e.g., `issue-brief-<slug>.md`).
- Ask: "Ready to plan the implementation, or do you want to refine the issue first?"

If skipping: Move to Phase 2 with whatever context the developer provides.

## Phase 2 -- Plan

Goal: Produce an actionable implementation plan.

Enter plan mode using the `EnterPlanMode` tool. Investigate the codebase in read-only mode:

- Map the implementation surface: files, functions, data flows.
- Study existing patterns for consistency.
- Identify constraints: performance, compatibility, security.

Write a structured plan to the plan file:
- Context: why this change is being made
- Solution design with file paths and line references
- Numbered implementation steps, specific enough to execute without re-investigation
- Verification steps: tests, build commands, manual checks

Call `ExitPlanMode` to present the plan for developer approval.

After approval, ask: "Solo implementation or parallel team execution?"

## Phase 3 -- Implement

Goal: Execute the plan.

**Solo mode:** Work through each step sequentially:
1. Announce which step is being executed.
2. Make the change as specified.
3. Verify: run any step-level checks from the plan.
4. Report the result before moving on.

**Team mode:** Analyze the plan for parallelizable work:
1. Identify independent steps (no shared files, no dependency ordering).
2. Group independent steps into parallel batches.
3. Present the execution strategy to the developer for confirmation.
4. Spawn one sub-agent (Sonnet) per step. Each receives its specific step, file paths, and instructions to announce, implement, verify, and report.
5. No two sub-agents write to the same file. Sequential fallback for conflicts.
6. Track progress by updating the plan document as steps complete.

If something unexpected comes up, stop and surface it to the developer. Do not improvise outside the plan scope.

After all steps complete, run verification commands from the plan. Report pass/fail.

Ask: "Want a structured code review of the changes?"

## Phase 4 -- Review

Goal: Verify the implementation through structured review.

If the developer wants a review, hand off to the `/review-team` pipeline:
1. Collect review inputs: the changes just made, focus areas, the original problem context.
2. Create the review team (Review Lead, Adjudication Lead, Planning Lead, Execution Lead).
3. Run the full review-adjudicate-plan-execute pipeline as described in the `/review-team` skill.

If the developer skips review, move to Phase 5.

## Phase 5 -- Commit

Goal: Create a clean commit.

1. Run `git status` and `git diff --stat` to summarize what changed.
2. Draft a conventional commit message:
   - Use the appropriate prefix (`feat:`, `fix:`, `refactor:`, etc.).
   - Imperative mood, concise (1-2 sentences).
   - Write in first person as the developer.
3. Present the draft to the developer for confirmation.
4. Stage the relevant files and commit only after the developer approves.

Do not push. The developer decides when to push.

## Phase 6 -- Done

Summarize:
- What was accomplished (phases completed, files modified).
- Tests run and results.
- Any follow-up items discovered during implementation or review.
- Any deviations from the original plan.

## Constraints

- **Ask at every transition.** Each phase boundary is a decision point. The developer can proceed, skip, or stop at any transition.
- **Stay in scope.** New issues discovered during implementation go in a follow-up list, not into the current work.
- **Do not push or deploy.** The developer controls when changes leave the local environment.
- **Do not invoke release skills.** Releases are a separate workflow triggered at release time, not per-task.
