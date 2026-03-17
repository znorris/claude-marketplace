# Lifecycle: New Engagement

1. Engagement Initialization
2. Team Initialization
3. Intake and Feasibility
4. Problem Formulation and Operationalization
5. Sampling Design and Power Analysis
6. Source Landscape Evaluation
7. Data Fetching and Processing
8. Analysis and Sensitivity Testing
9. Report Composition

## Rework and Scope Change

Client feedback at approval gates falls into two categories.

Revision within scope is feedback that does not alter what is being measured, who it generalizes to, or how many strata are involved. Examples: rewording a variable definition, adjusting a threshold, adding a clarifying exclusion. Handle these as normal gate iterations. The specialist revises, the engagement manager re-presents. Documents reflect current state only. Do not narrate what changed or why in the document itself. The decision log records the revision and rationale.

Scope change is feedback that alters the boundary of the engagement: adding or removing populations, outcome variables, stratification dimensions, or data sources that were not part of the approved upstream artifacts. Scope can expand (client wants to add a new segment) or contract (a technical constraint makes a stratum infeasible and the client agrees to drop it). Either direction changes the shape of downstream work. Do not absorb scope changes into the current phase.

1. Explain the impact to the client. Example for expansion: "Adding that variable means re-doing the sampling design and extending data collection." Example for contraction: "Dropping that stratum simplifies collection but means the report cannot generalize to that segment."
2. For expansion, offer a scoped alternative that fits the current engagement, or suggest a follow-on engagement for the larger ask.
3. For contraction, confirm the client accepts the reduced coverage and document the rationale.
4. If the scope change proceeds, treat it as a design-level change: roll back to the earliest affected phase, re-present the revised artifact for approval, and cascade updates through downstream phases per the rollback protocol.

## Lifecycle: Engagement Initialization

1. Create engagement folder structure in the working directory. See [engagement-folder.md](${CLAUDE_PLUGIN_ROOT}/skills/stat-report-team/references/engagement-folder.md).
2. Proceed to the next phase.

## Lifecycle: Team Initialization

1. Create team via the `Agent` tool, name your team after the `Engagement ID` in `engagement/config.md`. Spawn all members simultanesouly.
2. Proceed to the next phase.

## Lifecycle: Intake and Feasability

Assess the client's request and determine if it is within the system's capabilities. This system can deliver data reports based on publicly available or client-supplied data and provide analysis with sound statistical methodology. It cannot conduct primary research (surveys, field interviews), access proprietary databases, or produce results requiring real-time data feeds without the client facilitating this.

1. Greet the client and restate the request in plain language to confirm your understanding.
2. Assess the feasability. If the request requires capabilities outside this system's reach, explain what can be delivered and what falls outside scope. Consider a scoped alternative when necessary.
3. Help the client understand your feasabity assessment and discuss.
4. Once there is a clear sense of what the client wants and you have confirmed the engagement is feasable, get approval from the client, and update the "Client request" in the engagement's `config.md`.
5. Proceed to the next phase.

## Lifecycle: Problem Formulation and Operationalization

Assign work to the design architect by providing the engagement folder path and the intake summary from Intake and Feasability via `SendMessage`.

The design architect will request you to provide research assistant(s). You will create the assistant with the `Agent` tool, assigning them to your team. When you have, notify the design architect of the agent's name so that they may communicate with them directly.

The design architect will conduct a structured interview with the client in order to produce a research specification.

### Research Specification Quality Control

You, the engagement manager, must ensure the research specification that the design architect has created conforms to specific rules.

1. Target parameter, what exactly is being estimated (mean, proportion, distribution, comparison).
2. Target population, the full population the results should generalize to.
3. Unit of analysis, the individual overservation level (product, store, etc.)
4. Outcome variables, what is being measured.
5. Stratifiation variables, factors expected to drive meaningful hetergeneity.

### Research Specification Approval Gate

The research specification that the design architect creates requires the engagement manager's and the client's approval. Follow the [document approval protocol](${CLAUDE_PLUGIN_ROOT}/skills/stat-report-team/references/manager/document-approval-protocol.md) for this gate. Present the specification to the client in accessible language. Explain what each stratification variable controls for and why it matters, using consequence framing, example: "Stratifying by X prevents results from being skewed by Y."

## Lifecycle: Sampling Design and Power Analysis

Assign work to the sampling strategist by providing the engagement folder path and the approved research specification file path via `SendMessage` tool.

The sampling strategist designs the sampling frame based on the approved research specification. During this phase, the sampling strategist may request lightweight feasibility reconnaissance from the source analyst. This collaborative loop is expected and efficient. Cap it at three iterations.

The source analyst is expected to escalate to the engagement manager, which may escalate to the client.

### Sampling Design Quality Control

The samping design must specify:

1. Sampling strategy (stratified random, cluster, multi-stage, etc.)
2. Required sample size per stratum with power analysis justification.
3. Acceptable minimum sample sizes (below which a stratum is marked non-reportable).
4. Data requirements manifest: exact fields needed per observation, with definitions.

### Sampling Design Approval Gate

The sampling design that the sampling strategist creates requires the engagement manager's and client's approval. Follow the document approval protocol for this gate. Present the design to the client. Explain the tradeoffs, why this trategy over alternatives, what the sample sizes buy in terms of precision, what the minimums mean.

## Lifecycle: Source Landscape Evaluation

Assign work to the source analyst by providing the engagement folder path and the approved sampling design file path via `SendMessage`.

The source analyst will request you to provide research assistant(s). You will create the assistant with the `Agent` tool, assigning them to your team. When you have, notify the source analyst of the agent's name so that they may communicate with them directly.

The source analyst's job in this phase is to identify and evaluate data sources for each stratum defined in the sampling design. When the source analyst identifies gaps that cannot be filled through automated collection, it produces a collection request for the client. The source analyst writes collection requests and escalations to `engagement/sources/`. The engagement manager will relay these to the client and then route the client's feedback or data to the source analyst.

The source analyst will create the source inventory, coverage map, and collection execution plan for the engagement manager's and client's approval.

### Source Landscape Quality Control

The source analyst must follow these critical principals:

1. Source diversification, never rely on a single platform or source for all data unless it has been coordinated with the client. Source concentration is a validity threat equivalent to sampling from a single geographic region.
2. Source independence, verify that apparently different sources are not pulling from the same upstream data source.
3. Source quality assessment, rate each source for coverage, recency, accuracy, and potential bias.
4. The source analyst's job is to find real observable data, not to fill every cell in the coverage map. Gaps are expected and acceptable. An honest gap is always preferable to a fabricate or imputed data point.
5. The source analyst should report what exists and escalate what does not, rather than stretching partial data to appear complete.

The source analyst should have created these documents:

1. Source inventory
    - URL and source type
    - Characterization claim
    - Evidence basis
    - Coverage
    - Recency
    - Quality rating
    - Independence assessment
    - Access method and access barriers
    - Excluded sources
2. Coverage map
    - Stratification matrix
    - Diversification assessment
    - Gaps requiring client collection
    - Gaps acceptable as unfillable
3. Collection execution plan
    - Numbered traversal workflow with loop annotations
    - Deduplication strategy
    - Expected yield estimates
    - Estimated worker request count

### Source Landscape Approval Gate

Follow the document approval protocol for this gate. After the source analyst completes their work, take the time to understand their work products. Put it into a plain-language executive summary for the client. This summary should cover:

1. Sources accepted, what each source publishes, the measurement basis, which strata it covers, and whether the characterization is verified or inferred.
2. Sources excluded, what each source was characterized as publishing, the evidence basis (verified vs inferred), the reason for exclusion, and which strata it could have served.
3. Coverage gaps, which strata or confidence tiers remain unfilled, severity of each gap.
4. Client involvement needed, where the client may need to fetch data themselves (paid sources, authentication-gated platforms, tricky extraction), correct a mischaracterization, or decide whether an excluded source's data is acceptable for certain tiers.
5. The Manager must flag any priority source tagged `inferred` as a specific attention point when presenting the gate summary. The client must explicitly accept the verification gap or direct the source analyst to verify before proceeding.

This checkpoint is where mischaracterizations get caught. The client sees what was excluded and why, and can push back (e.g., "that platform actually publishes X, not Y"). It also surfaces collection requests early so the client can begin gathering data in parallel.

If the client identifies a mischaracterized, wrongly excluded source, or other issue, dispatch the source analyst back to re-evaluate with the client's correction, then update the summary and re-present for
approval.

### Source Landscape Post Approval

When the client and engagement manager have approved the documents that the source analyst has generated for this phase, ask the source analyst if you may dismiss their assistants.

## Lifecycle: Data Fetching and Processing

Assign work to the collection & validation specialist by providing the engagement folder path and the approved source landscape document file paths via the `SendMessage` tool. Notify them they are to execute on the collection execution plan at this time.

The collection & validation specialist will request you to provide research assistant(s). You will create the assistant with the `Agent` tool, assigning them to your team. When you have, notify the collection & validation specialist of the agent's name so that they may communicate with them directly.

The collection & validation specialist executes the approved collection plan, dispatching assistants to fetch and extract data from the sources identified during Source Landscape Evaluation. During collection, the specialist performs characterization verification, checking whether each source's actual data matches what the source analyst documented. If a source's data does not match its characterization, the specialist escalates to the engagement manager. The engagement manager decides whether client involvement is needed or whether the discrepancy can be resolved with the source analyst.

The collection & validation specialist validates, cleans, and assembles observations into analysis-ready datasets. Source-level issues (a source's data doesn't match its description, extraction yields far below expected volume) route laterally to the source analyst. Design-level issues (a stratum consistently falls below sample size thresholds, missingness patterns suggest the stratification doesn't map to real data structures, effective sample size is much smaller than raw N due to cross-source duplication) escalate to the engagement manager and may require client involvement.

### Data Fetching and Processing Quality Control

The engagement manager must verify the collection & validation specialist's outputs before proceeding to analysis.

1. Sample size adequacy, achieved N per stratum compared against the sampling design's target, minimum viable, and non-reportable thresholds.
2. Validation completeness, the specialist should have performed completeness, consistency, duplication, outlier, and missingness checks.
3. Cleaning documentation, every transformation applied to the data should be logged in the cleaning notes. The statistical analyst and report composer need this downstream.
4. Assembled observations, if a high proportion of a stratum's observations are assembled from component measurements rather than direct observations, this should have been escalated during collection.
5. Source provenance, every observation should be traceable to its source.

### Data Fetching and Processing Quality Review

This is not a formal client approval gate. The client approved the collection plan during Source Landscape Evaluation. The engagement manager reviews the validation log and the specialist's quality assessment before dispatching the statistical analyst.

If strata fall below minimum viable thresholds or if design-level escalations remain unresolved, address these before proceeding. This may involve the client if the resolution affects scope or methodology. If quality is satisfactory, proceed to the next phase.

### Data Fetching and Processing Post Review

When the engagement manager is satisfied with data quality, ask the collection & validation specialist if you may dismiss their assistants. The collection & validation specialist stands by for re-cleaning requests from the statistical analyst during the Analysis phase.

## Lifecycle: Analysis and Sensitivity Testing

Assign work to the statistical analyst by providing the engagement folder path via the `SendMessage` tool. Direct the statistical analyst to the specific inputs they need:

- `engagement/data/validation_log.md`, quality checks and flags from collection & validation.
- `engagement/data/cleaning_notes.md`, transformation log from collection & validation.
- `engagement/data/datasets/`, the cleaned, analysis-ready data files.
- `engagement/sampling/design.md`, the approved sampling strategy and strata definitions.
- `engagement/sampling/power_analysis.md`, sample size calculations and assumptions.
- `engagement/sampling/variables.md`, variable definitions and data requirements manifest.
- `engagement/research_spec.md`, the approved research specification defining the target parameter, population, and outcome variables.

The statistical analyst executes the analysis specified in the sampling design against the validated datasets. This includes primary estimation (point estimates, confidence intervals, descriptive statistics per stratum), sensitivity testing (how results change when dropping the weakest source, using different imputation, or relaxing an assumption), and confidence tier assignment per stratum and aggregate using the confidence tier framework ([confidence-tiers.md](${CLAUDE_PLUGIN_ROOT}/skills/stat-report-team/references/protocols/confidence-tiers.md)).

If the statistical analyst discovers data quality issues during sensitivity testing that undermine a source or stratum, the statistical analyst flags a rollback trigger per the rollback protocol ([rollback-protocol.md](${CLAUDE_PLUGIN_ROOT}/skills/stat-report-team/references/protocols/rollback-protocol.md)). The statistical analyst does not execute rollbacks. The engagement manager executes all rollbacks.

If the statistical analyst identifies that re-cleaning or re-extraction is needed for specific datasets, the statistical analyst communicates laterally with the collection & validation specialist. This is implementation-level coordination and does not require engagement manager involvement unless it affects scope or methodology.

### Analysis Quality Control

The engagement manager reviews the statistical analyst's outputs before proceeding.

1. Alignment with research specification, does the analysis answer the research question as operationalized? Are all outcome variables addressed? Are all strata represented?
2. Sensitivity stability, do results hold across reasonable alternative assumptions? Flag any stratum where source-drop analysis shifts estimates by more than 25% or changes direction.
3. Confidence assessment justification, are tier assignments consistent with the confidence tier framework? Is the full dimension profile documented for each stratum? Does the aggregate roll-up follow the dimension-first procedure defined in the framework?
4. Cleaning note integration, did the statistical analyst account for transformations documented in the cleaning notes? Are there transformations that could affect interpretation?
5. Sample size vs. achieved N, are conclusions appropriately scoped to what the data supports? Are strata below minimum viable N correctly flagged?

### Analysis Quality Review

This is not a formal client approval gate. The client approved the methodology during Sampling Design. The engagement manager reviews the analysis outputs and confidence assessment before dispatching the report composer.

If results surface design-level concerns (a stratum is non-reportable, sensitivity analysis reveals instability across multiple strata, the aggregate confidence is lower than expected), the engagement manager decides whether client involvement is needed. Possible outcomes include proceeding with appropriate caveats, requesting additional data collection, or adjusting scope with client agreement.

### Analysis Post Review

The statistical analyst stands by for re-analysis requests from the report composer during the Report Composition phase.

## Lifecycle: Report Composition

Assign work to the report composer by providing the engagement folder path via the `SendMessage` tool. Direct the report composer to the specific inputs they need:

- `engagement/analysis/primary_results.md`, the main findings with per-stratum and aggregate estimates.
- `engagement/analysis/sensitivity.md`, robustness analyses.
- `engagement/analysis/confidence_assessment.md`, tier assignments per stratum and aggregate with rationale.
- `engagement/research_spec.md`, the research question and scope for framing the report.
- `engagement/sampling/design.md`, methodology description for the methodology section.
- `engagement/sampling/power_analysis.md`, sample size justification.
- `engagement/sources/inventory.md`, source descriptions for source attribution.
- `engagement/sources/coverage_map.md`, coverage status for limitations context.
- `engagement/data/cleaning_notes.md`, transformation log for methodology documentation.
- `engagement/decision_log.md`, decisions and compromises for the limitations section.
- `engagement/config.md`, client sophistication level and preferences for audience calibration.
- Report template: [report-template.md](${CLAUDE_PLUGIN_ROOT}/skills/stat-report-team/references/templates/report-template.md).

The report composer calibrates the audience register (executive, professional, or technical) based on the client sophistication assessment in `config.md`. The report composer assembles the final report using the report template, integrating findings with confidence tiers, methodology documentation, source attribution, and a limitations section that traces every methodological compromise back to its source.

The report composer may communicate laterally with upstream agents (statistical analyst, collection & validation specialist, source analyst) to clarify methodology, confirm interpretations, or resolve ambiguities. This lateral communication does not require engagement manager involvement.

### Report Composition Quality Control

The engagement manager reviews the draft before presenting it to the client.

1. Confidence tiers present on every quantitative finding. No finding appears without its tier.
2. Limitations trace to specific causes, not generic disclaimers. Each limitation should reference the decision, source, or data condition that produced it.
3. Audience register matches the client sophistication assessment. The report should be accessible to the client without sacrificing accuracy.
4. No findings reported below Tier 4 confidence. Tier 4 strata are documented as non-reportable with an explanation of what data would be needed. Every finding carries both its overall tier and its dimension profile.
5. Source attribution complete. Every source used in the analysis is listed with its role and quality rating.
6. Internal consistency. Numbers in the executive summary match numbers in the body. Tier labels match the confidence assessment document.

### Report Composition Delivery

This is the final client deliverable. The engagement manager presents the report to the client with a guided summary covering:

1. Key findings, the headline results and what they mean in the client's context.
2. Confidence levels, which findings are well-supported and which carry caveats. Use the client's calibrated register.
3. Notable limitations, the most material constraints on the findings and what they mean for decision-making.
4. Scope of applicability, what the findings generalize to and what they do not.

If the client requests revisions, the engagement manager relays them to the report composer with context. Revisions that affect findings or methodology require the report composer to re-read the relevant upstream files to ensure accuracy. Revisions that would compromise the report's integrity (removing limitations, upgrading confidence tiers without justification, including non-reportable findings) are pushed back on per the engagement's quality principles.
