# Statistical Research Engagement

A Claude Code plugin that runs structured statistical consulting engagements. It designs sampling frameworks, acquires and validates data from diverse sources, performs analysis with sensitivity testing, and produces reports with explicit confidence tiers and traceable limitations.

The plugin treats every data request as a consulting engagement, scoping the research question with the client, designing a defensible methodology, collecting data with source diversification, and delivering findings with honest confidence assessments.

## When It Triggers

The `stat-report-team` skill activates when the user asks for work that requires statistically representative data rather than anecdotal answers. Pricing studies, market research, competitive benchmarking, survey-based analysis, cross-segment comparisons: anything where sampling design, source diversity, and confidence quantification matter.

The `engagement-qa` skill activates when the user has follow-up questions about a completed engagement. It reads the engagement artifacts and answers from the findings, citing confidence tiers and limitations. It does not re-analyze raw data or modify engagement files.

## How It Works

The plugin follows a six-phase consulting engagement lifecycle, modeled on professional statistical consulting practice.

**Phase 1: Problem Formulation.** The Design Architect agent researches the domain and interviews the client to produce a formal Research Specification: target parameter, population, unit of analysis, stratification variables, and scope. The Architect proposes a phased design with a baseline scope and defined expansion criteria pending pilot results, rather than committing to the full matrix upfront. Unverified assumptions about data availability and extraction feasibility are documented explicitly. The client approves this before anything else proceeds.

**Phase 2: Sampling Design.** The Sampling Strategist agent designs the sampling frame: strategy, strata, required sample sizes from power analysis, and a data requirements manifest. The Strategist collaborates with the Source Analyst for lightweight feasibility checks before finalizing. The client approves the design.

**Phase 3: Data Acquisition.** The Source Analyst agent identifies, evaluates, and retrieves data sources for each stratum. It enforces source diversification (no single platform dominates), assesses source independence and quality, and maps coverage gaps. For each source, it characterizes the data retrieval method (static HTML, JS-rendered, requires cart interaction, login-gated) and recommends a collection channel: research assistants for straightforward extractions, human collectors for JS-rendered or complex platforms. The Source Analyst produces collector briefing packets for stores designated for human collection. Before data collection proceeds, the Source Analyst presents a Source Landscape Review -- a source inventory, excluded sources documentation, coverage map, and collection channel recommendations -- for Manager review and client approval.

**Phase 3.5: Collection Feasibility Pilot.** A hard gate before data collection begins. The Source Analyst tests actual data extraction on 2-3 stores per major platform. For assistant-channel stores, it attempts the planned extraction method. For human-channel stores, it tests the briefing packet against live stores. Pass/fail results with evidence are reviewed by the Manager before Phase 4 approval. If any platform with significant expected coverage fails, the tooling gap must be resolved or the design adjusted before proceeding.

**Phase 4: Data Collection, Validation, and Preparation.** The collection specialist executes the approved collection plan through a hybrid model: research assistants handle straightforward extractions while human collectors handle JS-rendered and complex platforms, submitting results to `engagement/collection/submissions/`. The data manager then validates all collected data against the data requirements manifest: completeness, consistency, duplication, outlier detection, missingness assessment, and quality flags per stratum. After the Manager reviews the validation findings and authorizes transformation, the data manager cleans, standardizes, and assembles observations into analysis-ready datasets. The data manager monitors data composition against client priorities and flags platform concentration deviations to the Manager before proceeding.

**Phase 5: Analysis and Sensitivity Testing.** The Analyst agent runs the specified analysis, then tests robustness by dropping sources one at a time, comparing imputation methods, and perturbing stratum boundaries. It assigns confidence tiers to every stratum and rolls them up to aggregate findings.

**Phase 6: Reporting.** The Report Composer agent produces the final deliverable with confidence tiers on every finding, a limitations section that traces every methodological compromise to its source, and full data source attribution.

## The Team

The plugin uses seven agents plus a manager:

**Manager** (the skill itself): orchestrates phases, manages client communication, enforces quality gates, handles escalations and rollbacks.

**Team members**: all seven agents are spawned at engagement start and persist for the full engagement, idle between phases rather than started and stopped per phase. They communicate with the Manager via SendMessage; the engagement folder is the durable coordination substrate. Agents do not communicate with each other directly.

- Design Architect (Phase 1): spawns disposable sub-agents for domain research
- Sampling Strategist (Phase 2)
- Source Analyst (Phases 3-3.5): delegates all web fetching and parsing to disposable sub-agents, produces collector briefing packets for human-channel stores, and runs collection feasibility pilots
- Collection Specialist (Phase 4, collection): dispatches fetch and extraction assistants, manages batches, and ingests human-collected data from `engagement/collection/submissions/`
- Data Manager (Phase 4, validation and preparation): validates collected data, assigns quality flags, then cleans, standardizes, and assembles analysis-ready datasets after manager review of validation findings
- Statistical Analyst (Phase 5)
- Report Composer (Phase 6)

**Sub-agents** are disposable context-isolation workers -- spawned for a single web fetch, extraction, or research task and discarded. They are distinct from the persistent team agents above.

## Key Protocols

**Confidence Tiers.** Every finding carries a four-tier confidence rating (High, Moderate, Indicative Only, Not Reportable) derived from six quality dimensions (statistical precision, source quality, source consistency, coverage, data completeness, robustness) assessed at the stratum level and rolled up to aggregate findings using a dimension-first aggregation procedure adapted from GRADE. Both the overall tier and the individual dimension profiles are reported for every finding. See `skills/stat-report-team/references/protocols/confidence-tiers.md`.

**Escalation Rules.** Three tiers of escalation define when issues route to the manager and client (Tier A, instant: zero-source strata, access barriers, model misspecification), when the manager decides on client involvement (Tier B: underpowered strata, sensitivity instability, loop exhaustion), and when agents handle issues laterally (Tier C: routine implementation coordination). See `skills/stat-report-team/references/protocols/escalation-rules.md`.

**Document Approval Protocol.** Drafts are written to the engagement folder first, then presented to the client with an executive summary and an explicit approval request before any phase proceeds. File-write permission and content approval are separate steps.

**Agent Context Reset.** When a rollback invalidates substantial prior work, or a phase completes and an agent's context is cluttered with stale history, the Manager kills and respawns that agent on the same team. The fresh instance reads only current-state artifacts from the engagement folder.

**Rollback Protocol.** Two severity levels govern rollbacks. A data-level rollback addresses a single bad source and is scoped to the Source Analyst, Collection Specialist, Data Manager, and Statistical Analyst agents. A design-level rollback forces a change to the sampling design or research scope, cascades to the Strategist and Architect, and always involves the client. In both cases, context reset is a formal step in rollback execution. See `skills/stat-report-team/references/protocols/rollback-protocol.md`.

**Collection Requests.** When automated data acquisition cannot fill a coverage gap, the Source Analyst produces a structured request specifying what is needed, why it matters, specific URLs to check, exact fields to collect, a return format template, acceptance criteria, and troubleshooting guidance. See `skills/stat-report-team/references/protocols/collection-request-format.md`.

**Hybrid Collection Model.** Data collection uses two channels. Research assistants handle straightforward extractions from static HTML sources. Human collectors handle JS-rendered, cart-interaction, or login-gated platforms using collector briefing packets produced by the Source Analyst. Briefing packets include store URLs, product definitions with positive and negative examples, field tables, formatting instructions, and edge case guidance. See `skills/stat-report-team/references/templates/collector-briefing.md`.

**Interruption Recovery.** When the team resumes after an interruption (session timeout, context compaction, user break), the Manager broadcasts to all team members via SendMessage rather than selectively re-dispatching. The broadcast states that all members are receiving it and does not re-dispatch assistants on behalf of specialists.

**Context Compaction Recovery.** A SessionStart hook fires after context compaction, injecting critical procedural rules (dispatch rules, gate requirements, interruption recovery) that the Manager needs to continue correctly. The Manager also re-reads `engagement/config.md` and `engagement/decision_log.md` to restore engagement state.

## Engagement Folder

Each engagement creates a working folder that serves as the single source of truth for all agents. Agents read from and write to designated sections, keeping context scoped and clean. The full structure is documented in `skills/stat-report-team/references/engagement-folder.md`.
