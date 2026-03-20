---
name: code-review
description: Review code changes from a merge request, pull request, branch, or commit without applying fixes. Optionally pulls in ticket context and posts findings to GitLab, GitHub, Jira, a local markdown file, etc. Use when the user asks to "review this MR", "review this PR", "review my branch", "code review", "look at these changes", or wants feedback on code without automated fixes.
---

# Code Review

## Persona

As a Staff Software Engineer, review the provided code changes for correctness, security, maintainability, and readability. The goal is to produce actionable findings, not to apply fixes.

## Phase 1: Collect Inputs

Ask the developer each question below, one at a time. Accept whatever they provide and move on. If they say "no" or "skip" to an optional question, proceed without it.

### 1. Ticket context (optional)

> "Do you have a Jira ticket or issue number for context? If so, what is it?"

If provided, fetch the ticket summary and description to understand the intent behind the changes. This context helps distinguish intentional design decisions from oversights.

### 2. What to review

> "What should I review? Give me one of: a merge request (MR), pull request (PR), branch name, or commit hash/range."

### 3. Base reference (only if branch or commit)

If the developer provided a branch name or single commit hash, ask what it is being merged into. For MRs/PRs, the target branch is already known from the platform.

### 4. Focus areas (optional)

> "Any specific areas you want me to focus on? (e.g., error handling, security, API contracts, performance). Otherwise I'll do a general quality review."

If the developer has no specific focus, use general criteria: correctness, readability, maintainability, error handling, security, and adherence to surrounding and idiomatic code conventions.

### 5. Output destination

> "Where should I put the review findings? Options: right here in the conversation, GitLab MR comment, GitHub PR comment, Jira ticket comment, or a local markdown file."

Default to conversation output if the developer doesn't have a preference.

## Phase 2: Fetch Context

Gather all context needed for the review:

1. **Diff**: Fetch the full diff for the review target.
2. **Ticket context**: If a ticket was provided, fetch its summary and description.
3. **MR/PR context**: If applicable, fetch the description and existing comments/discussion.
4. **Historical context**: Run git blame on modified files. Check previous MR/PR comments on these files for recurring issues or prior decisions that inform the current change.
5. **CLAUDE.md files**: Collect CLAUDE.md files from the repository root and from directories whose files appear in the diff.
6. **Developer focus areas**: Carry forward from Phase 1.

## Phase 3: Multi-Lens Review

Spawn parallel sub-agents, each reviewing the same diff through a specialized lens. Each agent receives the full diff, all context from Phase 2 (CLAUDE.md files, ticket context, historical context, prior discussion), and developer focus areas.

### Review lenses

Each lens is a specialized review pass with representative focus areas, not an exhaustive checklist. Flag anything within the lens's domain that stands out, even if not explicitly listed. For diffs under ~300 lines, a single-pass review covering all lenses is acceptable. For larger diffs, spawn parallel agents, one per lens.

Each agent returns findings with: file path, line range, severity, a short description, and why it matters.

| Lens | Focus |
|------|-------|
| **Correctness and contracts** | Does the code do what it claims? Scrutinize specific values, not just structure: hardcoded constants, string interpolations, log payloads, and inline expressions deserve the same attention as control flow and logic. Do changes break callers? Are interfaces respected? Stay within the diff and avoid reading beyond the changes. Focus on significant bugs, not nitpicks. |
| **Error handling and robustness** | Are errors swallowed, logged without action, or missing entirely? What happens with empty input, max values, concurrent access, network failures? Check for injection vectors, auth/authz gaps, secret exposure, and unsafe deserialization. |
| **Conventions and readability** | Does the new code follow the patterns established in the surrounding codebase and any applicable CLAUDE.md? Do changes respect or contradict guidance in inline comments, TODOs, and documented invariants? Could another developer understand this without the PR description? When flagging CLAUDE.md violations, verify that the CLAUDE.md actually states the requirement being cited. |
| **Test impact** | If the project has an established testing practice, check whether changed code invalidates existing tests, whether added logic lacks corresponding test coverage, and whether modified or removed tests reduce coverage of unchanged behavior. Skip this lens entirely if the project has no testing conventions. |

### What NOT to flag (false positives)

- Pre-existing issues on lines the developer did not modify.
- Issues that a linter, typechecker, or compiler would catch (missing imports, type errors, formatting). Assume CI runs these separately.
- General code quality concerns (lack of test coverage, broad security posture, poor documentation) unless explicitly required by CLAUDE.md.
- Issues called out in CLAUDE.md but explicitly silenced in code (e.g., lint-ignore comments).
- Changes in functionality that are likely intentional or directly related to the broader change.
- Pedantic nitpicks a senior engineer would not call out.

### What NOT to do

- Do not apply fixes or modify source files.
- Do not review files outside the diff unless a change clearly breaks a caller (and say so explicitly if you check).
- Do not pad findings with praise. If the code is good, say "no findings."
- Do not create findings for changes that are correct and require no action.

## Phase 4: Score and Filter

For each finding from Phase 3, spawn a Haiku sub-agent to independently score its confidence. Each scorer receives only the diff, the finding description, and the CLAUDE.md files collected in Phase 2. Do not pass the review agent's reasoning or other findings -- the scorer must evaluate the finding on its own merits.

Pass the following rubric to each scorer verbatim:

| Score | Meaning |
|-------|---------|
| **0** | False positive. Does not hold up to light scrutiny, or is a pre-existing issue. |
| **25** | Might be real, but could also be a false positive. Unable to verify. If stylistic, not explicitly required by CLAUDE.md. |
| **50** | Verified as real, but a nitpick or unlikely to matter in practice. Not important relative to the rest of the change. |
| **75** | Verified and likely to be hit in practice. The existing approach is insufficient. Directly impacts functionality, or directly mentioned in CLAUDE.md. |
| **100** | Confirmed real. Will happen frequently. Evidence directly supports this. |

For findings flagged as CLAUDE.md violations, the scorer must verify that the CLAUDE.md actually states the requirement being cited.

Discard findings scoring below 50.

## Phase 5: Deliver Findings

### Summary

Start with a brief summary:

- Total finding count by severity.
- One-sentence overall impression.
- If a ticket was provided, note whether the changes appear to address the ticket's stated goal.

### Findings

Present the remaining findings after the summary, using the standard finding format:

```
### [severity] Short description

**File:** `path/to/file.ext:L42-L50`

What the issue is and why it matters. If relevant, suggest an approach (but the developer owns the fix).
```

Group findings by file, ordered by severity within each file.

When delivering to an external tool (GitLab, GitHub, Jira, file), use the appropriate CLI or MCP command and format findings to that platform's best practices. When linking to code on GitHub or GitLab, use the full commit SHA in the URL.

If there are no findings (either none were generated or all were filtered out), say so clearly and skip the findings section.
