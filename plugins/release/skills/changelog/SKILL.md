---
name: changelog
description: Generate or update CHANGELOG.md from git commits since the last tag. Use when the user asks to "update the changelog", "write a changelog entry", "document changes", or needs to record changes for a release.
---

Generate a changelog entry from git history and update CHANGELOG.md.

## Phase 1 — Determine Range

1. Find the latest semver tag: `git tag --list --sort=-v:refname`
2. If the developer specifies a range, use it. Otherwise, use `<latest-tag>..HEAD`.
3. If no tags exist, use all commits.
4. Collect commits: `git log <range> --oneline`

## Phase 2 — Read Existing Format

Read `CHANGELOG.md` if it exists. Match the project's existing format.

If no changelog exists, use [Keep a Changelog](https://keepachangelog.com/) format with these sections:

- **Added** — new features
- **Changed** — changes to existing functionality
- **Deprecated** — features that will be removed
- **Removed** — features that were removed
- **Fixed** — bug fixes
- **Security** — vulnerability fixes

## Phase 3 — Categorize Commits

Map conventional commit prefixes to changelog sections:

- `feat:` to Added
- `fix:` to Fixed
- `refactor:`, `perf:` to Changed
- `docs:`, `test:`, `ci:`, `chore:`, `style:` to omitted (unless they have user-facing impact)
- `BREAKING CHANGE` or `!` marker to a prominent note at the top of the entry

For non-conventional commits, categorize by the change description.

## Phase 4 — Draft Entry

Draft the entry with:

- Version number (derive from the latest tag and commit types, confirm with the developer)
- Today's date
- Categorized changes

Present the draft in a codeblock for review. Do not write to the file until the developer confirms.

## Phase 5 — Write

Prepend the confirmed entry to CHANGELOG.md, below the header and above the previous version entry.

## Constraints

- Do not run `git tag` or `git push`.
- Do not bump version files. This skill only updates the changelog.
- Match the project's existing format and conventions.
- Omit commits that are purely internal (CI, test-only, docs-only) unless the developer asks to include them.
- Write in first person as the developer. Avoid en dashes and em dashes.
