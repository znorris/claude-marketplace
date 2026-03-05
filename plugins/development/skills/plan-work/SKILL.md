---
name: plan-work
description: Break down an issue or task into concrete implementation steps with file locations, code changes, and acceptance criteria. Use when the user asks to "plan the implementation", "break down this task", "create an implementation plan", or needs a structured plan before coding.
---

Enter plan mode using the `EnterPlanMode` tool, then produce a detailed implementation plan for the given issue. Plan mode enforces read-only access during investigation and gives the developer a structured approval flow before any changes are made.

Prioritize long-term correctness, maintainability, and idiomatic solutions over quick fixes.

## Investigation Phase

Use Explore agents and read tools to understand the problem. You are read-only in plan mode — use this constraint to investigate thoroughly before committing to an approach.

- **Verify understanding**: Confirm the problem statement and root cause
- **Map the implementation surface**: Identify files, functions, and data flows involved
- **Study patterns**: How does the codebase handle similar cases? Find existing functions and utilities that can be reused.
- **Check runtime behavior**: Gather logs, queries, or API responses as needed
- **Note constraints**: Performance, compatibility, security requirements

Use `AskUserQuestion` to clarify requirements or choose between approaches during investigation.

## Plan Structure

Write the plan to the plan file provided by plan mode. Structure it as follows:

### Context

Why this change is being made: the problem or need, what prompted it, and the intended outcome.

### Root Cause Analysis

- What's happening and why (with evidence)
- Sequence of events or data flow leading to the issue
- Key code references: `[filename.ext:line](path#Lline)`

### Solution Design

Address the root cause directly — no workarounds:

- Follow **idiomatic patterns** for the language/framework
- Respect **separation of concerns** — logic belongs in the appropriate layer
- Consider **idempotency** and **graceful degradation** for distributed systems
- Flag **breaking changes** clearly for developer confirmation
- Reference existing functions and utilities found during investigation, with file paths

**Alternatives Considered**: Document rejected approaches and why.

### Files to Modify

List each file with:

- Path and line reference
- Brief description of the change (1-2 sentences)
- Concise code snippet only if essential

### Implementation Steps

Numbered, actionable steps. Keep code examples minimal — show the essential change, not surrounding context.

Each step should be specific enough to execute without re-investigation.

### Verification

**Automated Testing:**

- Specific test commands to run
- New test cases needed (describe intent, not full implementations)

**Manual/Staging Verification:**

- Steps to validate in a running environment
- Log queries or monitoring checks
- Expected behavior vs. previous behavior

**Post-Deployment:**

- Deployment sequence if order matters
- Rollback criteria and procedure
- Monitoring queries for regressions

## Quality Checklist

Before finalizing:

- [ ] Follows idiomatic patterns for the language/framework?
- [ ] Addresses root cause, not just symptoms?
- [ ] Handles concurrent/distributed edge cases?
- [ ] Handles error cases appropriately?
- [ ] Is this the simplest solution that solves the problem?
- [ ] Breaking changes called out for developer confirmation?
- [ ] No injection, auth bypass, or data exposure risks?

## Finalize

Once the plan is complete and the quality checklist passes, call `ExitPlanMode` to present the plan for developer approval. Do not ask about plan approval via text or `AskUserQuestion` — `ExitPlanMode` handles this.

## Principles

**Prioritize**: Long-term correctness > backward compatibility > implementation speed

**Avoid**:

- Leaking internal details across layer boundaries
- Over-engineering (extra config, abstractions, or features not requested)
- Ignoring existing codebase patterns
- Large code blocks — summarize or show only the essential diff
