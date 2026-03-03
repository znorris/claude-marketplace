# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/).

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
