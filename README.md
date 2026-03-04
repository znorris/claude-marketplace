# claude-marketplace

A Claude Code plugin marketplace with developer workflow skills organized by development lifecycle phase.

## Installation

```bash
# Add the marketplace
/plugin marketplace add znorris/claude-marketplace

# Install plugins (pick what you need)
/plugin install development@znorris/claude-marketplace
/plugin install git@znorris/claude-marketplace
/plugin install release@znorris/claude-marketplace
/plugin install atlassian-tools@znorris/claude-marketplace
/plugin install gitlab-tools@znorris/claude-marketplace
/plugin install scorevision@znorris/claude-marketplace
```

## Workflow

Plugins are organized by development lifecycle phase. Use them independently or chain them together:

```text
Define -> Plan -> Implement -> Review -> Commit -> Release
```

| Phase | Plugin | What it does |
| --- | --- | --- |
| Define | development | Create well-structured issues from bugs or feature requests |
| Plan | development | Break down issues into concrete implementation steps |
| Implement | development | Execute plans step by step, solo or with parallel sub-agents |
| Review | development | Review changes for correctness, security, and maintainability |
| Commit | git | Generate conventional commit messages and branch summaries |
| Release | release | Generate changelogs and user-facing release notes |

**Typical flow:** `/create-issue` to define the work, `/plan-work` to break it down, `/implement` to execute, `/review-team` to verify, `/commit-msg` to commit, then `/changelog` and `/release-notes` when ready to ship.

For the full guided experience, use `/dev-lifecycle-team` to walk through each phase with decision points.

Vendor-specific plugins (`atlassian-tools`, `gitlab-tools`) and team-specific plugins (`scorevision`) are opt-in and integrate with the core workflow where relevant.

## Plugins

### development

Full development lifecycle: define issues, plan work, implement changes, review code, and orchestrate the entire flow with agent teams.

| Skill | Description |
| --- | --- |
| `/create-issue` | Investigate the root cause of a bug or define requirements for a feature, then produce a well-structured issue ready for your tracker |
| `/plan-work` | Break down an issue or task into concrete implementation steps with file locations, code changes, and acceptance criteria |
| `/implement` | Execute an implementation plan step by step with verification checkpoints and progress tracking |
| `/implement-team` | Execute an implementation plan using parallel sub-agents for independent steps. Requires `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` enabled. |
| `/review-team` | Launch a multi-stage code review pipeline that reviews changes, adjudicates findings with you, plans fixes, and executes them via an agent team. Requires `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` enabled. |
| `/dev-lifecycle-team` | Guided full-lifecycle orchestration: define, plan, implement, review, and commit with decision points at each phase. Requires `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` enabled. |

### git

Generate conventional commit messages and branch summaries from git history.

| Skill | Description |
| --- | --- |
| `/commit-msg` | Analyze staged changes and generate a conventional commit message that accurately describes the what and why of your changes |
| `/branch-summary` | Analyze all commits on your branch since diverging from main, then generate a title and description summarizing the complete changeset |

### release

Generate changelogs and user-facing release notes from git history.

| Skill | Description |
| --- | --- |
| `/changelog` | Generate or update CHANGELOG.md with an entry for commits since the last tag or a specified range |
| `/release-notes` | Generate user-facing release notes from commits since the last tag, written for end users rather than developers |

### atlassian-tools

Quick reference for Atlassian CLI (acli) commands covering JQL queries, ticket transitions, comments, and workflow operations.

| Skill | Description |
| --- | --- |
| `/jira-cli` | Reference guide for acli commands covering JQL queries, ticket transitions, comments, and workflow operations |

### gitlab-tools

Quick reference for GitLab CLI (glab) commands covering issues, merge requests, pipelines, and CI/CD operations.

| Skill | Description |
| --- | --- |
| `/gitlab-cli` | Reference guide for glab commands covering issues, merge requests, pipelines, releases, and CI/CD |

### scorevision

ScoreVision team conventions for Jira workflows, ticket states, and development processes.

| Skill | Description |
| --- | --- |
| `/ticket-workflow` | ScoreVision's Jira workflow reference -- ticket states, allowed transitions, and when to move tickets through the pipeline |
| `/review-tickets` | Audit your ScoreVision Jira tickets against the current codebase -- identify completed work, stale tickets, and status mismatches |

## References

- <https://code.claude.com/docs/en/skills>
- <https://code.claude.com/docs/en/plugin-marketplaces>
