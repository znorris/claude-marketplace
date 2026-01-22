# claude-skills

A Claude Code plugin marketplace with developer workflow skills.

## Installation

```bash
# Add the marketplace
/plugin marketplace add znorris/claude-skills

# Install plugins
/plugin install git-workflows@znorris-skills
/plugin install issue-management@znorris-skills
/plugin install gitlab-tools@znorris-skills
/plugin install jira-tools@znorris-skills
```

## Plugins

### git-workflows

Git workflow skills for version control operations.

| Skill              | Description                                            |
| ------------------ | ------------------------------------------------------ |
| `/commit-msg`      | Generate a concise commit message for staged changes   |
| `/merge-request`   | Generate MR/PR title and description for branch changes|
| `/prepare-release` | Analyze repo and recommend semver version bump         |

### issue-management

Platform-agnostic issue and ticket management.

| Skill             | Description                                             |
| ----------------- | ------------------------------------------------------- |
| `/create-issue`   | Investigate and document a bug or feature request       |
| `/plan-work`      | Create a detailed implementation plan for an issue      |
| `/review-tickets` | Review and sync tickets with codebase state             |

### gitlab-tools

GitLab CLI (glab) reference - auto-loaded when working with GitLab.

| Skill         | Description                                            |
| ------------- | ------------------------------------------------------ |
| `/gitlab-cli` | Command reference for issues, MRs, pipelines, releases |

### jira-tools

Atlassian CLI (acli) reference - auto-loaded when working with Jira.

| Skill       | Description                                                 |
| ----------- | ----------------------------------------------------------- |
| `/jira-cli` | Command reference for JQL, transitions, comments, workflows |

## References

- <https://code.claude.com/docs/en/skills>
- <https://code.claude.com/docs/en/plugin-marketplaces>
