---
name: implement
description: Execute an implementation plan step by step with verification checkpoints and progress tracking
---

Execute a structured implementation plan. Work through each step with developer oversight.

## Phase 1 — Locate the Plan

Find the plan to execute:

- If the developer specifies a file, use it.
- Otherwise, search for recent `plan-*.md` or implementation plan files in the working directory.
- If multiple candidates exist, ask which one to use.

Read the plan. Summarize: what will change, how many steps, which files are affected. Ask the developer to confirm before touching any files.

## Phase 2 — Execute Steps

For each numbered step in the plan:

1. **Announce** — state which step is being executed and what it involves.
2. **Implement** — make the change as specified.
3. **Verify** — if the plan includes verification for this step (test commands, expected behavior), run it. If not, confirm the change looks correct in context.
4. **Report** — brief result before moving to the next step.

If something unexpected comes up during a step (a file doesn't exist, a function signature changed, a test fails), stop and surface it to the developer. Do not improvise fixes outside the plan scope.

## Phase 3 — Verification

After all steps are complete, run any verification commands specified in the plan:

- Automated tests
- Build commands
- Manual verification steps (describe what to check)

Report pass/fail for each.

## Phase 4 — Summary

Provide:

- Steps completed
- Files modified
- Tests run and results
- Any deviations from the plan (with reasoning)
- Any new issues discovered (for separate follow-up)

## Constraints

- **Stay in scope.** Implement exactly what the plan specifies. If something adjacent needs fixing, note it — do not fix it.
- **Do not commit.** The developer decides when to commit.
- **Do not deploy.** No deploy or push commands.
- **Surface surprises.** If reality doesn't match the plan, stop and ask rather than guessing.
