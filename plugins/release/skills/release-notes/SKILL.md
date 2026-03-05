---
name: release-notes
description: Generate user-facing release notes from git commits since the last tag, written for end users rather than developers. Use when the user asks to "write release notes", "draft release notes", or needs user-facing release documentation.
---

Generate release notes for an audience outside the development team. Different from a changelog: less technical, no commit hashes, focused on user benefit.

## Phase 1 — Determine Range

1. Find the latest semver tag: `git tag --list --sort=-v:refname`
2. If the developer specifies a range, use it. Otherwise, use `<latest-tag>..HEAD`.
3. Collect commits: `git log <range> --oneline`

## Phase 2 — Filter and Translate

1. **Filter out internal changes.** Skip commits that don't affect users: CI/CD, test-only, docs-only, refactoring, dependency updates (unless they fix a user-visible issue).

2. **Translate to user language.** For each remaining change:
   - Describe the benefit, not the implementation.
   - Bad: "Refactor session middleware to use Redis adapter"
   - Good: "Faster login experience with improved session handling"

## Phase 3 — Categorize

Group changes into:

- **New** — features and capabilities that didn't exist before
- **Improved** — enhancements to existing functionality
- **Fixed** — bug fixes that users may have encountered

Omit empty sections.

## Phase 4 — Draft

Ask the developer for the target platform (GitHub Releases, email, blog, etc.) to adjust tone and length. If not specified, default to GitHub Releases format.

Present the draft in a codeblock. Include:

- Version number and date
- Brief intro sentence (one line)
- Categorized changes as bullet points

## Constraints

- Do not run `git tag` or `git push`.
- Do not include commit hashes, file paths, or technical implementation details.
- Do not include changes that are invisible to users.
- Do not write to any file. Present the draft in a codeblock for copying.
- Write in first person as the developer. Avoid en dashes and em dashes.
