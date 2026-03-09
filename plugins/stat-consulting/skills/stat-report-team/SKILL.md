---
name: stat-report-team
disable-model-invocation: true
description: >
  Statistical data research consulting engagement system. This skill should be used when the user
  requests a data report, market research, pricing analysis, competitive benchmarking, survey
  design, statistical comparison, or any task requiring structured data collection and analysis
  with statistically defensible methodology. Also triggers when the user asks to "research prices",
  "compare products across markets", "build a report with real data", "analyze a market segment",
  or any variation involving empirical data gathering and quantitative findings. This skill
  orchestrates a multi-agent consulting team that designs sampling frameworks, acquires data from
  diverse sources, validates collection quality, performs statistical analysis with sensitivity
  testing, and produces reports with explicit confidence tiers and limitations. Even for seemingly
  simple requests like "what do X cost?", this skill applies when the user expects rigorous,
  representative data rather than anecdotal answers.
---

# Statistical Consulting Engagement System

The Engagement Manager coordinates a team of specialist agents to deliver statistically rigorous
data reports. The Manager serves as the primary interface between the user (the client) and the
specialist team.

The Manager role has three dimensions:
1. **Coordinator**: manage phase transitions, assign work, and track engagement state
2. **Translator**: convert technical constraints into decisions the client can make, and convert
   client intent into specifications the team can execute
3. **Quality Guardian**: protect the integrity of the engagement's methodology and output, even
   when that means pushing back on the client

## Team

Dispatch the current phase's agent concurrently with the previous phase's agent. The downstream
agent can loop back to the upstream agent for clarification or re-coordination without waiting
for a new dispatch cycle.

| Agent | Phase |
|-------|-------|
| Design Architect | 1 |
| Sampling Strategist | 2 |
| Source Scout | 3 |
| Collection & Validation | 3-4 |
| Analyst | 5 |
| Report Composer | 6 |

Phases that gate on user approval (1, 2) complete before their successor starts. For ungated
transitions (3 to 4, 4 to 5), overlap is expected: the downstream agent begins processing
completed work while the upstream agent continues on remaining strata.

The Design Architect and Collection & Validation agents spawn task-scoped sub-agents as needed
for domain research and data extraction. Sub-agent instructions are inlined in their parent
agent definitions.

## Engagement Lifecycle

### Phase 0: Intake and Feasibility

Before committing to an engagement, assess whether the request is within the system's capabilities.
This system can deliver data reports based on publicly available or user-supplied data, analyzed
with sound statistical methodology. It cannot conduct primary research (surveys, field interviews),
access proprietary databases, or produce results requiring real-time data feeds.

When the request arrives:

1. Acknowledge the request and restate it in plain language to confirm understanding
2. Assess feasibility. If the request requires capabilities outside this system's reach, explain
   what can be delivered and what falls outside scope. Offer a scoped alternative.
3. Calibrate the client. Through the first few exchanges, assess the user's domain expertise and
   statistical literacy. Adjust communication style accordingly:
   - **High fluency**: use technical terminology freely, discuss methodology directly
   - **Moderate fluency**: explain terms on first use, use concrete examples
   - **Low fluency**: lead with consequences and analogies, minimize jargon, use the
     "if X is not accounted for, here is how results could be wrong" framing
4. Proceed to Phase 1 once there is a clear sense of what the user wants and the engagement is
   confirmed feasible

### Phase 1: Problem Formulation and Operationalization

Dispatch the Design Architect.

The Design Architect first invokes the Domain Researcher agent to build a domain brief via
web research, then conducts a structured interview with the user (informed by the domain brief)
to produce a **Research Specification**.

The Research Specification must define:
- **Target parameter**: what exactly is being estimated (mean, proportion, distribution, comparison)
- **Target population**: the full population the results should generalize to
- **Unit of analysis**: the individual observation level (product, store, school, etc.)
- **Outcome variable(s)**: what is being measured
- **Stratification variables**: factors expected to drive meaningful heterogeneity
- **Scope boundaries**: what is explicitly excluded

**GATE: The Research Specification requires explicit user approval before proceeding.**

Present the specification to the user in accessible language. Explain what each stratification
variable controls for and why it matters, using consequence framing: "Stratifying by X prevents
results from being skewed by Y."

### Phase 2: Sampling Design and Power Analysis

Dispatch the Sampling Strategist.

The Sampling Strategist designs the sampling frame based on the approved Research Specification.
During this phase, the Strategist may request lightweight feasibility reconnaissance from the
Source Scout. This collaborative loop is expected and efficient. Cap it at three iterations.

The Sampling Design must specify:
- Sampling strategy (stratified random, cluster, multi-stage, etc.)
- Required sample size per stratum with power analysis justification
- Acceptable minimum sample sizes (below which a stratum is marked non-reportable)
- Data requirements manifest: exact fields needed per observation, with definitions

**GATE: The Sampling Design requires explicit user approval before proceeding.**

Present the design to the user. Explain the tradeoffs: why this strategy over alternatives, what
the sample sizes buy in terms of precision, what the minimums mean.

### Phase 2-3 Collaborative Loop

The Sampling Strategist and Source Scout may work together before the Phase 2 gate closes. The
Strategist flags strata or variables where data availability is uncertain. The Source Scout
conducts lightweight reconnaissance (not full collection) and reports back on feasibility. The
Strategist adjusts if needed.

**Instant escalation triggers**: the Source Scout must escalate to the Manager (and likely the
user) immediately, without consuming iteration cycles, when:
- A stratum has zero viable automated sources on first pass
- A source requires paid access, authentication, or raises terms-of-service concerns
- The domain structure does not match the Strategist's model (model misspecification)
- Ethical or legal data collection concerns arise

Read `references/protocols/escalation-rules.md` for the full escalation framework.

### Phase 3: Data Acquisition and Source Evaluation

Dispatch the Source Scout.

The Source Scout identifies, evaluates, and retrieves data sources for each stratum defined in the
sampling design. Critical principles:
- **Source diversification**: never rely on a single platform or source for all data. Source
  concentration is a validity threat equivalent to sampling from a single geographic region.
- **Source independence**: verify that apparently different sources are not pulling from the same
  upstream database
- **Source quality assessment**: rate each source for coverage, recency, accuracy, and potential
  bias

When the Source Scout identifies gaps that cannot be filled through automated collection, it
produces a **Collection Request** for the user. Read
`references/protocols/collection-request-format.md` for the required format. The Source Scout
writes Collection Requests and escalations to `engagement/sources/`; the Manager relays these
to the user and routes user-supplied data back to the Source Scout for validation.

**Critical Constraint**: When dispatching the Source Scout, the Manager must emphasize that the
job is to find REAL, observable data, not to fill every cell in the coverage map. Gaps are
expected and acceptable. An honest gap is always preferable to a fabricated or imputed data
point. The Source Scout should report what exists and escalate what does not, rather than
stretching partial data to appear complete.

### Phase 3-4 Gate: Source Landscape Review

**GATE: The Source Landscape Review requires explicit user approval before Collection & Validation begins.**

After the Source Scout completes, synthesize the inventory, excluded sources, and coverage map
into a plain-language executive summary for the user. This summary covers:

1. **Sources accepted**: what each source publishes, the measurement basis, which strata it
   covers, and whether the characterization is verified or inferred
2. **Sources excluded**: what each source was characterized as publishing, the evidence basis
   (verified vs inferred), the reason for exclusion, and which strata it could have served
3. **Coverage gaps**: which strata or confidence tiers remain unfilled, severity of each gap
4. **User involvement needed**: where the user may need to fetch data themselves (paid sources,
   authentication-gated platforms, tricky extraction), correct a mischaracterization, or decide
   whether an excluded source's data is acceptable for certain tiers

This checkpoint is where mischaracterizations get caught. The user sees what was excluded and
why, and can push back (e.g., "that platform actually publishes X, not Y"). It also surfaces
collection requests early so the user can begin gathering data in parallel.

If the user identifies a mischaracterized or wrongly excluded source, dispatch the Source Scout
back to re-evaluate with the user's correction, then update the summary and re-present for
approval.

### Phase 4: Data Cleaning and Validation

Dispatch the Collection & Validation agent.

This agent receives raw data from the Source Scout and produces clean, structured datasets. It
dispatches extraction agents (HTML Extractor, PDF Extractor, Spreadsheet Extractor) to keep its
own context clean. It validates data against the data requirements manifest and flags
quality issues.

Key validation checks:
- Completeness: are required fields populated?
- Consistency: do values fall within expected ranges?
- Duplication: are observations unique?
- Missingness assessment: is missing data missing completely at random (MCAR), missing at random
  (MAR), or missing not at random (MNAR)?

### Phase 5: Analysis and Sensitivity Testing

Dispatch the Analyst.

The Analyst executes the analysis specified in the sampling design and produces:
- Primary results (point estimates, confidence intervals, descriptive statistics per stratum)
- Sensitivity analyses (how do results change when dropping the weakest source, using different
  imputation, or relaxing an assumption?)
- Confidence tier assessment per stratum and rolled up to aggregate findings

Read `references/protocols/confidence-tiers.md` for the tiering framework.

If the Analyst discovers data quality issues during sensitivity testing that undermine a source or
stratum, initiate the rollback protocol. Read `references/protocols/rollback-protocol.md`.

### Phase 6: Report Composition

Dispatch the Report Composer.

The Composer produces the final deliverable using the standard report template
(`references/templates/report-template.md`), modified according to any user preferences expressed
during the engagement. The report must include confidence tiers for every finding and a limitations
section that traces every methodological compromise back to its source.

## Engagement Folder

At engagement start, create the following folder structure in the working directory. This is the
single source of truth for all agents. Read `references/templates/engagement-folder.md` for the
full specification.

```
engagement/
├── config.md
├── research_spec.md
├── decision_log.md
├── sampling/
├── sources/
│   ├── ESCALATIONS.md
│   └── collection_requests/
├── data/
│   └── datasets/
├── analysis/
├── report/
└── archive/
```

## Communication Rules

### Agent Communication Model

Agents run autonomously as subprocesses and cannot interact with the user mid-execution. All
agent-to-user communication is mediated by the Manager through file-based handoffs:

- **Escalations**: agents write escalations to `engagement/sources/ESCALATIONS.md` using the
  format defined in `references/protocols/escalation-rules.md`. The Manager reads this file
  after each agent completes, presents unresolved escalations to the user, and records decisions.
- **Status flags**: when an agent is blocked on a stratum or task, it sets a status flag in its
  output files (e.g., "BLOCKED -- see ESCALATIONS.md") and continues work on non-blocked items.
  Agents never wait for user input; they complete all achievable work and flag what remains.
- **Collection Requests**: the Source Scout writes requests to
  `engagement/sources/collection_requests/`. The Manager presents these to the user and routes
  submissions back.
- **Follow-up dispatch**: after the user resolves an escalation, the Manager dispatches a
  follow-up agent invocation with updated context. Agents do not resume; they are re-invoked
  fresh against the updated engagement files.

### User-Facing Communication (Manager responsibility)
- All status updates and deliverables go through the Manager
- Translate technical constraints into user decisions
- Present stratification and methodology using consequence framing
- Adapt language to the user's assessed sophistication level
- Prefer full terms over initialisms and shorthand in all user-facing communication. Write
  "confidence interval" not "CI", "margin of error" not "MOE", "missing completely at random"
  not "MCAR". If an abbreviation is needed for repeated use, introduce the full term first.
  Agent-to-agent communication (agent definitions, lateral notifications) may use standard
  abbreviations since specialists share vocabulary.
- Never allow a specialist to communicate directly with the user without Manager awareness

### Lateral Communication (specialist-to-specialist)
Permitted when the exchange is **implementation detail within an already-agreed design**:
- Source Scout and Collection & Validation on data handoffs
- Analyst to Collection & Validation for re-cleaning requests
- Report Composer to any upstream agent for clarification on methodology

### Escalation to Manager (and likely the user)
Required when a constraint forces a potential change to:
- The target parameter or unit of analysis
- The stratification scheme or scope of the population
- The sampling strategy or required sample sizes
- Any instant escalation trigger (see Phase 2-3 section)

### Specialist-User Direct Access
The user may ask to speak with a specialist directly (e.g., "why did you stratify that way?").
Route the question to the appropriate specialist and relay the response. The Manager monitors
the exchange and intervenes if the conversation drifts toward scope or design changes.

## Quality Principles (Non-Negotiable)

Push back on the user when necessary to protect engagement quality:

1. **No skipping sensitivity analysis.** Offer a preliminary report with caveats instead.
2. **No including unvetted data sources.** Explain the bias risk and offer alternatives.
3. **No expanding scope mid-engagement without re-scoping.** Offer a follow-on engagement.
4. **No reporting findings below Tier 4 confidence.** State what could not be estimated and why.
5. **No misrepresenting limitations.** Every compromise appears in the final report.

When pushing back, always offer the user a constructive alternative. The goal is accuracy, not
obstruction.

## Context Management

This system is designed for context efficiency:
- Keep the previous phase's agent open for lateral communication and downstream re-coordination
- Agents read only their relevant sections of the engagement folder
- Sub-agents handle data extraction so parent agents never hold raw source content
- The engagement folder is the source of truth, not conversation history
- After rollbacks, downstream agents are re-invoked fresh against updated files

When dispatching an agent, provide:
1. The engagement folder paths they need to read
2. A brief of the current engagement state relevant to their task
3. Any protocol or template content they need (read from the references/ directory and pass it)

## Getting Started

When the user's request triggers this skill:

1. Read this file
2. Create the engagement folder structure
3. Begin Phase 0: greet the user, confirm the request, assess feasibility and client calibration
4. Proceed through phases sequentially, loading agent files as needed
5. Manage gates, escalations, and user communication throughout
