# Statistical Research Engagement

A Claude Code plugin that runs structured statistical consulting engagements. It designs sampling frameworks, acquires and validates data from diverse sources, performs analysis with sensitivity testing, and produces reports with explicit confidence tiers and traceable limitations.

The plugin treats every data request as a consulting engagement, scoping the research question with the user, designing a defensible methodology, collecting data with source diversification, and delivering findings with honest confidence assessments.

## When It Triggers

The `stat-report-team` skill activates when the user asks for work that requires statistically representative data rather than anecdotal answers. Pricing studies, market research, competitive benchmarking, survey-based analysis, cross-segment comparisons: anything where sampling design, source diversity, and confidence quantification matter.

## How It Works

The plugin follows a six-phase consulting engagement lifecycle, modeled on professional statistical consulting practice.

**Phase 1: Problem Formulation.** The Design Architect agent researches the domain and interviews the user to produce a formal Research Specification: target parameter, population, unit of analysis, stratification variables, and scope. The user approves this before anything else proceeds.

**Phase 2: Sampling Design.** The Sampling Strategist agent designs the sampling frame: strategy, strata, required sample sizes from power analysis, and a data requirements manifest. The Strategist collaborates with the Source Scout for lightweight feasibility checks before finalizing. The user approves the design.

**Phase 3: Data Acquisition.** The Source Scout agent identifies, evaluates, and retrieves data sources for each stratum. It enforces source diversification (no single platform dominates), assesses source independence and quality, and maps coverage gaps. Before data collection proceeds, the Source Scout presents a Source Landscape Review -- a source inventory, excluded sources documentation, and a coverage map -- for Manager review and user approval. When automated collection cannot fill a gap, it produces structured collection requests with specific URLs, field definitions, return formats, and acceptance criteria for the user to fulfill.

**Phase 4: Data Cleaning and Validation.** The Collection & Validation agent processes raw data through format-specific extractor agents (HTML, PDF, spreadsheet), validates against the data requirements manifest, checks for duplication and missingness, and assigns quality flags per stratum.

**Phase 5: Analysis and Sensitivity Testing.** The Analyst agent runs the specified analysis, then tests robustness by dropping sources one at a time, comparing imputation methods, and perturbing stratum boundaries. It assigns confidence tiers to every stratum and rolls them up to aggregate findings.

**Phase 6: Reporting.** The Report Composer agent produces the final deliverable with confidence tiers on every finding, a limitations section that traces every methodological compromise to its source, and full data source attribution.

## The Team

The plugin uses six agents plus a manager:

**Manager** (the skill itself): orchestrates phases, manages user communication, enforces quality gates, handles escalations and rollbacks.

**Team members**: all six agents are spawned at engagement start and persist for the full engagement, idle between phases rather than started and stopped per phase. They communicate with the Manager via SendMessage; the engagement folder is the durable coordination substrate. Agents do not communicate with each other directly.

- Design Architect (Phase 1): spawns disposable sub-agents for domain research
- Sampling Strategist (Phase 2)
- Source Scout (Phase 3): delegates all web fetching and parsing to disposable sub-agents to keep its own context clean for coordination and quality assessment
- Collection & Validation (Phases 3-4): spawns disposable sub-agents for HTML, PDF, and spreadsheet extraction
- Analyst (Phase 5)
- Report Composer (Phase 6)

**Sub-agents** are disposable context-isolation workers -- spawned for a single web fetch, extraction, or research task and discarded. They are distinct from the persistent team agents above.

## Key Protocols

**Confidence Tiers.** Every finding carries a four-tier confidence rating (High, Moderate, Indicative Only, Not Reportable) assessed at the stratum level and rolled up to aggregate findings. Tier assignment is based on sample adequacy, source diversity, data quality flags, and sensitivity analysis stability. See `skills/stat-report-team/references/protocols/confidence-tiers.md`.

**Escalation Rules.** Three tiers of escalation define when issues route to the manager and user (Tier A, instant: zero-source strata, access barriers, model misspecification), when the manager decides on user involvement (Tier B: underpowered strata, sensitivity instability, loop exhaustion), and when agents handle issues laterally (Tier C: routine implementation coordination). See `skills/stat-report-team/references/protocols/escalation-rules.md`.

**Document Approval Protocol.** Drafts are written to the engagement folder first, then presented to the user with an executive summary and an explicit approval request before any phase proceeds. File-write permission and content approval are separate steps.

**Agent Context Reset.** When a rollback invalidates substantial prior work, or a phase completes and an agent's context is cluttered with stale history, the Manager kills and respawns that agent on the same team. The fresh instance reads only current-state artifacts from the engagement folder.

**Rollback Protocol.** Two severity levels govern rollbacks. A data-level rollback addresses a single bad source and is scoped to the Scout, Validation, and Analyst agents. A design-level rollback forces a change to the sampling design or research scope, cascades to the Strategist and Architect, and always involves the user. In both cases, context reset is a formal step in rollback execution. See `skills/stat-report-team/references/protocols/rollback-protocol.md`.

**Collection Requests.** When automated data acquisition cannot fill a coverage gap, the Source Scout produces a structured request specifying what is needed, why it matters, specific URLs to check, exact fields to collect, a return format template, acceptance criteria, and troubleshooting guidance. See `skills/stat-report-team/references/protocols/collection-request-format.md`.

## Engagement Folder

Each engagement creates a working folder that serves as the single source of truth for all agents. Agents read from and write to designated sections, keeping context scoped and clean. The full structure is documented in `skills/stat-report-team/references/templates/engagement-folder.md`.
