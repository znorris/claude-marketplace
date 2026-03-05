# Plugin Development Guidelines

## Plugin Structure

```text
plugins/<name>/
  .claude-plugin/plugin.json    # name, description, version
  skills/<skill>/SKILL.md       # frontmatter: name, description
  agents/<agent>.md             # frontmatter: name, description, model, tools
  README.md                     # optional, for complex plugins
```

- Every plugin must have a `plugin.json` with `name`, `description`, `version`.
- Every skill must have a `SKILL.md` with frontmatter: `name`, `description`.
- Team/orchestrator skills add `disable-model-invocation: true` to frontmatter.

## Cross-Reference Rules

- Individual skills must NOT reference sibling skills by name in their body instructions.
- Express intent without naming specific skills. Write "steps should be specific enough to execute without re-investigation" not "specific enough for `/implement`".
- Team/workflow skills (with `-team` suffix) MAY reference other skills because orchestration is their purpose.
- Any skill may reference universal tools (git commands, shell commands, etc.).

## Naming Conventions

- Skills that spawn agent teams use the `-team` suffix (e.g., `/implement-team`, `/review-team`).
- Plugin names: lowercase, hyphenated.
- Skill names: lowercase, hyphenated.
- Prefix-group related skills by noun so they cluster in autocomplete (e.g., `git-commit-msg` and `git-branch-summary` both appear when typing `/git`).
- Name by what the developer would search for, not internal terminology. Prefer `issue-create` over `create-issue` when `/issue` is a more likely search than `/create`.
- Keep names short but specific enough to distinguish from similar skills.

## Skill Descriptions

Descriptions serve double duty: they are documentation AND a search surface for autocomplete. Skill names are the primary match; descriptions are secondary.

- Follow the pattern: `[Action statement]. Use when the user asks to "[phrase 1]", "[phrase 2]", or [scenario].`
- Include natural-language trigger phrases that a developer would actually type or say.
- Include searchable keywords that relate to the skill's domain (e.g., "git" in commit/branch skill descriptions).
- Reference skills like `gitlab-cli` and `jira-cli` should say "Use when running [tool] commands" so Claude consults them proactively.
- Do not use em dashes or en dashes in descriptions.

## Compartmentalization

- Each skill should be self-contained and work independently.
- Skills must not overlap in ownership. Only one skill should own a given decision (e.g., version bumps belong to `prepare-release`, not `changelog`).
- Plugins group skills by lifecycle phase, not by implementation complexity.

## Voice and Tone

- Write in first person as the developer.
- Never use "we".
- Avoid en dashes, em dashes, and other non-standard characters.
- Use personas only where the role meaningfully shapes output quality (e.g., investigation, planning, review skills). Do not add personas to simple action skills like commit message generators or reference guides.

## Constraints Pattern

- Every skill that produces output must specify whether it writes to files or presents in a codeblock.
- Skills that write artifacts must specify the target directory or file.
- Skills should guard against scope creep with explicit "do not" constraints (e.g., "Do not commit", "Do not deploy").

## Agent Definitions

- Frontmatter fields: `name`, `description`, `model`, `tools`.
- Tools list should be minimal. Only grant what the agent needs.
- If an agent should not modify source files, state it explicitly even if the tools list suggests it (e.g., a planning agent with `Write` access should be told "Write only to artifact files").

## Release Process

- Always update CHANGELOG.md before committing changes.
- Bump the marketplace version and any edited plugin versions before committing.
