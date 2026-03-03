# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/).

## [1.3.0] - 2026-03-02

### Added
- review-cycle plugin: multi-stage code review agent team (review, adjudicate, plan, execute)

### Fixed
- Register review-cycle plugin in marketplace.json so the plugin system discovers it

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
