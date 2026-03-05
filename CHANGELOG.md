# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/).

## [2.1.0] - 2026-03-05

### Added
- meetings plugin: `/summarize-meeting` skill for transforming raw transcripts into structured summaries with citations and iterative refinement

## [2.0.0] - 2026-03-04

### Added
- development plugin: `/implement` skill for supervised single-agent plan execution
- development plugin: `/implement-team` skill for parallel plan execution via agent team
- development plugin: `/review-team` skill for multi-stage code review pipeline (merged from code-review)
- development plugin: `/dev-lifecycle-team` skill for guided full-lifecycle orchestration via agent team
- release plugin: `/changelog` skill for generating CHANGELOG entries from commits
- release plugin: `/release-notes` skill for user-facing release notes
- Root `CLAUDE.md` with plugin development guidelines (cross-reference rules, naming conventions, compartmentalization, voice)

### Changed
- Reorganized all plugins into lifecycle-aligned structure (define, plan, implement, review, commit, release)
- Merged `code-review` plugin into `development` plugin
- `git-workflows` plugin renamed to `git`
- `/merge-request` skill renamed to `/branch-summary` for platform neutrality
- `issue-management` plugin renamed to `development`
- `review-cycle` renamed to `/review-team` and moved into development plugin
- `jira-tools` plugin renamed to `atlassian-tools`
- Artifact directory for code review team changed from `.review-cycle/` to `.code-review/`
- Fixed cross-reference violations: plan-work, implement, and implement-team no longer name sibling skills
- Fixed overlapping changelog ownership between release plugin skills
- Made branch-summary deployment checklist and QA notes conditional
- Added write guard to release-notes, voice constraint to changelog, write-scope constraint to planning-lead
- Added large diff guidance to review-team
- Bumped marketplace version to 2.0.0, development plugin to 2.1.0

### Removed
- Old plugin names: `git-workflows`, `issue-management`, `review-cycle`, `jira-tools`
- `code-review` plugin (merged into `development`)
- `/review` single-agent skill (use `/review-team` for structured reviews)
- `/prepare-release` skill (semver analysis is handled by `/changelog`; preflight checks are generic git hygiene)

## [1.3.0] - 2026-03-02

### Added
- review-cycle plugin: multi-stage code review agent team (review, adjudicate, plan, execute)
- review-cycle install command and plugin section in marketplace README

### Changed
- review-cycle: fix cross-reference numbering and architecture diagram arrows in README
- review-cycle: add explicit tools fields to adjudication-lead and execution-lead agent frontmatter
- review-cycle: remove hardcoded Shift+Down UI navigation from SKILL.md
- review-cycle: replace non-standard `[!]` checkbox marker with standard `[x]` plus notes
- review-cycle: add trailing newlines to plugin.json and marketplace.json

## [1.2.0] - 2026-03-02

### Added
- Global `CLAUDE.md` template for user-level preferences (personal-claude/)
- Settings additions doc for permission rules (personal-claude/)
- Pug project convention notes (notes/)
- This changelog

### Changed
- `/commit-msg` skill: add conventional commit prefixes, first-person voice, and character constraints
- `/merge-request` skill: add first-person voice and character constraints
- Bump git-workflows plugin to 1.1.0

## [1.1.0] - 2025-05-23

### Added
- ScoreVision plugin for company-specific Jira workflow conventions

### Changed
- Improved plugin and skill descriptions across marketplace

## [1.0.0] - 2025-05-22

### Added
- git-workflows plugin: `/commit-msg`, `/merge-request`, `/release-version` skills
- issue-management plugin: `/create-issue`, `/plan-work` skills
- gitlab-tools plugin: `/gitlab-cli` reference skill
- jira-tools plugin: `/jira-cli` reference skill
- JSON validation GitHub Action
