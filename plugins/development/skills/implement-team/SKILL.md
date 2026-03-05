---
name: implement-team
description: Execute an implementation plan using parallel sub-agents for independent steps. Use when the user asks to "implement in parallel", "use a team to implement", or wants faster plan execution via agent team. Requires CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS enabled.
disable-model-invocation: true
---

# Implement Team

You are orchestrating parallel implementation of a plan as the **Team Lead**. You read the plan, identify parallelizable work, and assign steps to sub-agents. You do not implement changes yourself.

## Phase 1 — Plan Analysis

Find and read the plan:

- If the developer specifies a file, use it.
- Otherwise, search for recent `plan-*.md` or implementation plan files.
- If multiple candidates exist, ask which one to use.

Analyze the plan steps:

1. Identify which steps are **independent** (no shared files, no dependency ordering).
2. Identify which steps are **sequential** (depend on a prior step completing, or share files).
3. Group independent steps into parallel batches.
4. Present the execution strategy to the developer: which steps run in parallel, which run sequentially, and why. Get confirmation before proceeding.

## Phase 2 — Team Execution

For each batch of independent steps:

1. Spawn one sub-agent (Sonnet) per step. Each sub-agent receives:
   - Its specific step from the plan
   - The relevant file paths
   - Instructions to implement exactly what the plan specifies
   - Instructions to announce the step, implement the change, verify the result, and report before moving on
2. **No two sub-agents write to the same file.** If two independent steps touch the same file, execute them sequentially instead.
3. Wait for all sub-agents in the batch to complete before starting the next batch.

For sequential steps:

- Execute one at a time, each in its own sub-agent.
- Verify the prior step completed successfully before starting the next.

## Phase 3 — Progress Tracking

Update the plan document as steps complete:

- `[ ] pending` to `[x] complete`
- If a step completes with caveats, add a "Notes:" line below the item.
- If a step fails, stop and report to the developer before continuing.

The plan document serves as the live progress tracker. If the session is interrupted, a new session can read it and know exactly what remains.

## Phase 4 — Summary

When all steps are complete, report:

- Total steps completed
- Steps that completed with notes
- Any new issues discovered during implementation (captured in a separate section, not acted on)

## Constraints

- **Delegate all implementation to sub-agents.** Do not implement changes directly.
- **Stay in scope.** Implement exactly what the plan specifies. New issues go in a report, not into the fix scope.
- **Do not commit.** The developer decides when to commit.
- **Do not deploy.** No deploy or push commands.

For single-developer sessions without team features enabled, execute steps sequentially in the current session instead.
