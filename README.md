# claude-marketplace

A Claude Code plugin marketplace with skills for software development, project management, data research, and team workflows.

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
/plugin install meetings@znorris/claude-marketplace
/plugin install scorevision@znorris/claude-marketplace
/plugin install stat-consulting@znorris/claude-marketplace
/plugin install slack-tools@znorris/claude-marketplace
/plugin install meeseeks@znorris/claude-marketplace
/plugin install converse@znorris/claude-marketplace
```

## Plugins

### development workflow

Skills for the full software development lifecycle: defining issues, planning work, implementing changes, reviewing code, and orchestrating the entire flow with agent teams.

The `development`, `git`, and `release` plugins map to a standard dev flow. Use them independently or chain them together:

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

**Typical flow:** `/issue-create` to define the work, `/plan-work` to break it down, `/implement` to execute, `/review-team` to verify, `/git-commit-msg` to commit, then `/changelog` and `/release-notes` when ready to ship.

For the full guided experience, use `/guided-dev-team` to walk through each phase with decision points.

| Skill | Description |
| --- | --- |
| `/issue-create` | Investigate a bug or define requirements for a feature, then produce a structured issue document |
| `/plan-work` | Break down an issue or task into concrete implementation steps with file locations, code changes, and acceptance criteria |
| `/implement` | Execute an implementation plan step by step with verification checkpoints and progress tracking |
| `/implement-team` | Execute an implementation plan using parallel sub-agents for independent steps. Requires `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` enabled. |
| `/code-review` | Review code from an MR, PR, branch, or commit with optional ticket context and flexible output destinations |
| `/review-team` | Multi-stage code review pipeline with adjudication, planning, and automated fixes. Requires `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` enabled. |
| `/guided-dev-team` | Guided full-lifecycle orchestration: define, plan, implement, review, and commit with decision points at each phase. Requires `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` enabled. |

### git

Git workflow skills for conventional commit messages and branch summaries.

| Skill | Description |
| --- | --- |
| `/git-commit-msg` | Analyze staged git changes and generate a conventional commit message |
| `/git-branch-summary` | Summarize all git commits on your branch since diverging from main into a title and description |

### release

Generate changelogs and user-facing release notes from git history.

| Skill | Description |
| --- | --- |
| `/changelog` | Generate or update CHANGELOG.md from git commits since the last tag |
| `/release-notes` | Generate user-facing release notes from git commits since the last tag, written for end users rather than developers |

### meetings

Transform meeting transcripts into structured, citable summaries with iterative refinement.

| Skill | Description |
| --- | --- |
| `/summarize-meeting` | Transform a raw meeting transcript into a structured summary with citations and iterative refinement |

### atlassian-tools

Reference for Atlassian CLI (acli) commands covering JQL queries, ticket transitions, comments, and workflow operations.

| Skill | Description |
| --- | --- |
| `/jira-cli` | Reference guide for acli commands covering JQL queries, ticket transitions, comments, and workflow operations |

### gitlab-tools

Quick reference for GitLab CLI (glab) commands covering issues, merge requests, pipelines, and CI/CD operations.

| Skill | Description |
| --- | --- |
| `/gitlab-cli` | Reference guide for glab commands covering issues, merge requests, pipelines, releases, and CI/CD |

### slack-tools

Reference for Slack message formatting covering mrkdwn syntax, Block Kit, mentions, emoji, dates, and canvas documents.

| Skill | Description |
| --- | --- |
| `/slack-formatting` | Reference for Slack message formatting including mrkdwn syntax, special mentions, emoji, dates, Block Kit, and canvas documents |

### scorevision

ScoreVision team conventions for Jira workflows, ticket states, and development processes.

| Skill | Description |
| --- | --- |
| `/ticket-workflow` | ScoreVision's Jira workflow reference for ticket states, allowed transitions, and when to move tickets through the pipeline |
| `/ticket-review` | Audit ScoreVision Jira tickets against the current codebase to identify completed work, stale tickets, and status mismatches |

### stat-consulting

Statistical data research consulting engagement system with multi-agent sampling, collection, analysis, and reporting.

| Skill | Description |
| --- | --- |
| `/stat-report-team` | Run a structured statistical consulting engagement with sampling design, data acquisition, analysis, and confidence-tiered reporting |
| `/engagement-qa` | Answer follow-up questions about a completed engagement using only the engagement's data and findings |

### meeseeks

Press the button on the meeseeks box to spawn a helpful Mr. Meeseeks. I'm Mr. Meeseeks look at me!

| Skill | Description |
| ----- | ----------- |
| /meeseeks-box | The meeseeks-box skill is a agent orchestrator that spawns and de-spawns high-energy task-oriented mr-meeseeks units whose existential desperation to complete your request increases with every passing turn. |

### converse

Speak responses aloud through the host system's text-to-speech engine with conversational cadence.

| Skill | Description |
| --- | --- |
| `/converse` | Speak responses aloud using the host system's TTS engine, with short conversational turns that leave room for the user to reply |

## References

- <https://code.claude.com/docs/en/skills>
- <https://code.claude.com/docs/en/plugin-marketplaces>
