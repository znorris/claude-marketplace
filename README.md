# claude-marketplace

A Claude Code plugin marketplace with developer workflow skills.

## Installation

```bash
# Add the marketplace
/plugin marketplace add znorris/claude-marketplace

# Install plugins
/plugin install git-workflows@znorris/claude-marketplace
/plugin install issue-management@znorris/claude-marketplace
/plugin install gitlab-tools@znorris/claude-marketplace
/plugin install jira-tools@znorris/claude-marketplace
/plugin install scorevision@znorris/claude-marketplace
```

## Plugins

### git-workflows

Automate Git workflows: generate commit messages from diffs, create PR descriptions from branch history, and determine semantic versions for releases.

| Skill              | Description                                            |
| ------------------ | ------------------------------------------------------ |
| `/commit-msg`      | Analyze staged changes and generate a conventional commit message that accurately describes the what and why of your changes |
| `/merge-request`   | Analyze all commits on your branch since diverging from main, then generate a PR/MR title and description summarizing the complete changeset |
| `/prepare-release` | Examine commits since the last tag, categorize changes by type (breaking/feature/fix), and recommend the appropriate semantic version bump |

### issue-management

Turn bugs into actionable tickets and break down features into implementation plans—works with any issue tracker.

| Skill           | Description                                       |
| --------------- | ------------------------------------------------- |
| `/create-issue` | Investigate the root cause of a bug or define requirements for a feature, then produce a well-structured issue ready for your tracker |
| `/plan-work`    | Break down an issue or task into concrete implementation steps with file locations, code changes, and acceptance criteria |

### gitlab-tools

Quick reference for GitLab CLI (glab) commands—issues, merge requests, pipelines, and CI/CD operations.

| Skill         | Description                                            |
| ------------- | ------------------------------------------------------ |
| `/gitlab-cli` | Reference guide for glab commands covering issues, merge requests, pipelines, releases, and CI/CD—includes common workflows and syntax examples |

### jira-tools

Quick reference for Atlassian CLI (acli) commands—JQL queries, ticket transitions, comments, and workflow operations.

| Skill       | Description                                                 |
| ----------- | ----------------------------------------------------------- |
| `/jira-cli` | Reference guide for acli commands covering JQL queries, ticket transitions, comments, and workflow operations—includes common patterns and syntax |

### scorevision

ScoreVision team conventions for Jira workflows, ticket states, and development processes.

| Skill              | Description                                          |
| ------------------ | ---------------------------------------------------- |
| `/ticket-workflow` | ScoreVision's Jira workflow reference—ticket states, allowed transitions, and when to move tickets through the pipeline |
| `/review-tickets`  | Audit your ScoreVision Jira tickets against the current codebase—identify completed work, stale tickets, and status mismatches |

## References

- <https://code.claude.com/docs/en/skills>
- <https://code.claude.com/docs/en/plugin-marketplaces>
