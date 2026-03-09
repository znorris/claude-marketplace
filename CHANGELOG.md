# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/).

## [3.8.0] - 2026-03-09

### Added
- stat-consulting: add Document Approval Protocol separating file-write permission from content approval at all gates
- stat-consulting: explicit 5-step sequence (draft notification, write, present for review, user review, resolution) prevents file-write prompts from being mistaken as content sign-off
- stat-consulting: require Manager executive summary at each gate covering purpose, overview, attention points, and review guidance tailored to document type and user sophistication
- stat-consulting: add Document Authoring Rules requiring current-state-only artifacts (no revision history outside the decision log), no manual line breaks within markdown paragraphs, and prescriptive/descriptive separation (no inlined recon findings or unqualified assumptions in design documents)
- stat-consulting: add Agent Context Reset guidelines for killing and respawning team agents when context is polluted by stale work, rollbacks, or multiple revision cycles

### Changed
- stat-consulting: update rollback protocol Step 5 to explicitly require agent reset (kill and respawn) instead of ambiguous "re-invoke"

## [3.6.0] - 2026-03-09

### Changed
- stat-consulting: spawn all six team agents at engagement start instead of per-phase dispatch
- stat-consulting: replace per-phase "Spawn the X" with "Assign work to" language throughout lifecycle
- stat-consulting: add explicit team agent vs sub-agent distinction with tool-level differences (Agent with team_name, SendMessage, TaskCreate vs plain Agent spawn)
- stat-consulting: update Getting Started to include team initialization before Phase 0

## [3.5.0] - 2026-03-09

### Changed
- stat-consulting: replace subprocess dispatch model with TeamCreate + team agents for specialist agents
- stat-consulting: rewrite Agent Communication Model to use SendMessage with file-based coordination as the substrate
- stat-consulting: add Sub-Agents section distinguishing disposable context-isolation tasks from persistent team agents
- stat-consulting: update Context Management for team-aware agent persistence and messaging

## [3.4.0] - 2026-03-09

### Changed
- stat-consulting: add Engagement Manager persona to team orchestrator skill

## [3.3.0] - 2026-03-09

### Changed
- stat-consulting: add evidence-based source characterization claims (verified vs inferred) to Source Scout evaluation
- stat-consulting: require documented entries for excluded sources in inventory with partial relevance assessment
- stat-consulting: add Excluded Sources section to inventory template (source-scout)
- stat-consulting: add exclusion audit to coverage gap analysis cross-referencing gaps against excluded sources
- stat-consulting: broaden measurement basis mismatch escalation to cover "related but different quantity" sources
- stat-consulting: add Tier A trigger for partial-relevance source exclusion (escalation-rules)
- stat-consulting: add Source Landscape Review gate between Phase 3 and Phase 4 (SKILL.md) requiring user approval of accepted/excluded sources before collection begins
- stat-consulting: add characterization verification step to Collection & Validation agent (compare observed data against Source Scout claims)

## [3.2.0] - 2026-03-08

### Changed
- stat-consulting: add anti-fabrication guardrails and data integrity rules to Source Scout agent
- stat-consulting: add measurement basis mismatch as an instant escalation trigger (Source Scout and escalation-rules)
- stat-consulting: split coverage map yield into spec-matching vs partial/component columns
- stat-consulting: add observation assembly protocol to Collection & Validation agent with quality penalty for assembled data
- stat-consulting: add agent communication model to SKILL.md (file-based escalation via ESCALATIONS.md, no mid-execution user interaction)
- stat-consulting: add data integrity constraint to Manager's Source Scout dispatch instructions
- stat-consulting: add plain language communication rule (expand initialisms on first use in user-facing output)
- stat-consulting: expand bare abbreviations in SKILL.md body and report template (MCAR/MAR/MNAR, CI)
- stat-consulting: add assembled observation ceiling to Tier 1 confidence criteria
- stat-consulting: add ESCALATIONS.md to engagement folder structure

## [3.1.0] - 2026-03-08

### Added
- stat-consulting plugin: `/stat-report-team` skill for structured statistical consulting engagements with multi-agent sampling design, data acquisition, analysis, and confidence-tiered reporting

## [3.0.0] - 2026-03-05

### Changed
- Renamed skills for better autocomplete discoverability (prefix-grouping by noun)
  - `/commit-msg` to `/git-commit-msg` (found by typing `/git`)
  - `/branch-summary` to `/git-branch-summary` (found by typing `/git`)
  - `/create-issue` to `/issue-create` (found by typing `/issue`)
  - `/review-tickets` to `/ticket-review` (found by typing `/ticket`, groups with `/ticket-workflow`)
  - `/dev-lifecycle-team` to `/guided-dev-team` (clearer intent)
- Added "Use when" trigger phrases to all skill descriptions following official plugin patterns
- Fixed em dashes in plugin and skill descriptions (gitlab-tools, atlassian-tools, scorevision)
- Replaced blanket "no personas" rule with targeted guidance: use personas where the role shapes output quality
- Bumped all edited plugin versions

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
