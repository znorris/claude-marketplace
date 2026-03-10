# Plugin Development Guidelines

## Plugin Structure

```text
plugins/<name>/
  .claude-plugin/plugin.json    # name, description, version
  skills/<skill>/
    SKILL.md                    # frontmatter: name, description (required)
    references/                 # supporting docs, loaded as needed
    assets/                     # output files (templates, images), not loaded
    scripts/                    # executable code, may run without loading
  agents/<agent>.md             # frontmatter: name, description, model, tools
  README.md                     # optional, for complex plugins
```

- Every plugin must have a `plugin.json` with `name`, `description`, `version`.
- Every skill must have a `SKILL.md` with frontmatter: `name`, `description`.
- Team/orchestrator skills add `disable-model-invocation: true` to frontmatter.

## Supporting Files

Skills can include files alongside SKILL.md. Reference them from SKILL.md so Claude knows they exist and when to load them. Keep SKILL.md under 500 lines; move detailed content to supporting files.

Convention from Anthropic's skill-development guide for organizing supporting files:

- `references/` - Documentation loaded into context as needed (schemas, API docs, domain knowledge). Information should live in either SKILL.md or references, not both. For large files (>10k words), include grep search patterns in SKILL.md.
- `assets/` - Files used in output, not loaded into context (templates, images, boilerplate). Claude copies or modifies these rather than reading them.
- `scripts/` - Executable code for deterministic/repetitive tasks. May be executed without loading into context.

Only create directories the skill actually needs.

### Referencing Supporting Files

SKILL.md must explicitly list supporting files so Claude knows they exist. Use relative paths from the skill directory:

```markdown
## Additional Resources
- **`references/patterns.md`** - Common patterns and examples
- **`assets/report-template.html`** - Report output template
```

### Path Variables

- `${CLAUDE_SKILL_DIR}` - Resolves to the directory containing the skill's SKILL.md. Use in bash injection commands to reference bundled scripts or files regardless of the current working directory.
- `${CLAUDE_PLUGIN_ROOT}` - Resolves to the plugin root directory. Use in hooks and MCP server configurations (hooks.json, .mcp.json), not in SKILL.md.

### Progressive Disclosure

Skills use a three-level loading system:

1. **Metadata** (name + description) - Always in context
2. **SKILL.md body** - Loaded when the skill triggers
3. **Supporting files** - Loaded as needed by Claude

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

Descriptions serve double duty: they are documentation AND a search surface for autocomplete. Skill names are the primary match; descriptions are secondary. Claude uses the description to decide when to load the skill automatically.

- Include concrete trigger phrases: `[Action statement]. Use when the user asks to "[phrase 1]", "[phrase 2]", or [scenario].`
- Anthropic's skill-development guide recommends third-person: `This skill should be used when the user asks to "..."`.
- Include natural-language trigger phrases that a developer would actually type or say.
- Include searchable keywords that relate to the skill's domain (e.g., "git" in commit/branch skill descriptions).
- Reference skills like `gitlab-cli` and `jira-cli` should say "Use when running [tool] commands" so Claude consults them proactively.
- Do not use em dashes or en dashes in descriptions.

## Compartmentalization

- Each skill should be self-contained and work independently.
- Skills must not overlap in ownership. Only one skill should own a given decision (e.g., version bumps belong to `prepare-release`, not `changelog`).
- Plugins group skills by lifecycle phase, not by implementation complexity.

## SKILL.md Writing Style

- Write the SKILL.md body in imperative/infinitive form (verb-first instructions), not second person.
- Use objective, instructional language: "To accomplish X, do Y" not "You should do X".
- Frontmatter descriptions use third person: "This skill should be used when..."

## Voice and Tone

- Write commit messages, PRs, and team-facing content in first person as the developer.
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

## Agent Tool

The `Agent` tool is available only to the main session (Manager). Team agents do not have it. When a team agent needs external work done (web research, document extraction, domain discovery), it writes a task file to the engagement folder and sends the Manager a short message requesting a worker spawn. The Manager spawns a general-purpose sub-agent (`subagent_type: general-purpose`, `model: claude-sonnet-4-6`) pointing at the task file path. The worker reads the task file, executes, writes results to the specified output path, sends a completion notice to the `reply_to` agent named in the task file, and shuts down.

**`subagent_type`** is a fixed enum of registered agent types. The available values are determined by marketplace registration -- they are not file paths. Example values: `general-purpose`, `stat-consulting:source-scout`, `stat-consulting:collection-validation`, `development:execution-lead`. Format for plugin-defined agents is `<plugin-name>:<agent-name>`.

**`team_name`** is what distinguishes a team agent from a sub-agent:

- **Team agent**: `Agent` tool with `team_name` set. The agent is persistent, interactive, surfaces permission prompts to the user, and can receive messages via SendMessage. Use the correct `subagent_type` for the specialist role (e.g., `stat-consulting:source-scout`), not `general-purpose`.
- **Sub-agent**: `Agent` tool without `team_name`. Disposable, fire-and-forget, returns a single result to the parent. Use for narrow tasks: web fetches, HTML parsing, PDF extraction, domain research.

Do not describe `subagent_type` as "pointing to a definition file" -- the registered type IS the definition. Do not instruct agents to "bypass the definition file" or describe it as a "behavioral contract" separate from the type registration.

## Release Process

- Always update CHANGELOG.md before committing changes.
- Bump the marketplace version and any edited plugin versions before committing.
- Renaming a skill is a breaking change (major version bump on both the marketplace and the affected plugin).

## Pre-Commit Checklist

Before committing changes to this repo, verify every item:

1. **SKILL.md frontmatter** -- `name` matches the directory name and `description` includes "Use when" trigger phrases.
2. **plugin.json** -- `version` is bumped for any plugin whose skills, agents, or config changed.
3. **marketplace.json** -- `version` bumped, and every plugin entry's `version` matches its `plugin.json`. New plugins are listed here.
4. **README.md** -- New or renamed skills appear in the correct plugin table. Install commands include any new plugins.
5. **Plugin README.md** -- If the plugin has one, skill names and descriptions are current.
6. **CHANGELOG.md** -- Entry added for the current version describing what changed.
7. **No em dashes or en dashes** -- Descriptions, skill bodies, and commit messages use only standard characters.
8. **Cross-references** -- Non-team skills do not name sibling skills. Team skills may reference others.
9. **Descriptions are searchable** -- Skill names prefix-group by noun for autocomplete. Descriptions contain keywords a developer would search for.
