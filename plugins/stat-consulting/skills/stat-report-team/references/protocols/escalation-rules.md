# Escalation Rules

This protocol defines when an issue must be escalated beyond the current agent, who it escalates
to, and what information must accompany the escalation.

## Escalation Tiers

### Tier A: Instant Escalation to Manager and User

These situations bypass all iteration cycles and collaborative loops. The Manager must involve the
user immediately because the issue affects what was promised or what is achievable.

**Triggers:**
- **Zero viable sources for a stratum**: no automated data sources exist on first-pass
  reconnaissance. This isn't a "try harder" situation; the data infrastructure doesn't exist.
- **Access barriers**: a critical source requires paid access, authentication credentials, or
  legal agreements (subscriptions, APIs with commercial terms, data-sharing agreements).
- **Model misspecification**: the domain structure doesn't match the Sampling Strategist's model.
  The assumed market segmentation, organizational hierarchy, or classification system doesn't
  correspond to reality. This invalidates the sampling design.
- **Ethical or legal concerns**: data collection would involve scraping content that violates
  terms of service, accessing personally identifiable information without consent, or retrieving
  data that raises legal questions. When in doubt, escalate.
- **Measurement basis mismatch**: the available data sources provide observations at a different
  measurement basis than the Research Specification requires (e.g., component-level data when the
  spec requires composite figures, or aggregated data when the spec requires per-unit
  observations). This may invalidate the sampling design or require a scope adjustment.
- **Scope impossibility**: the engagement cannot deliver what was specified in the Research
  Specification with any achievable design. Better to surface this early than after significant
  work.

**Escalation format:**
```
ESCALATION [Tier A] from [Agent Name]

ISSUE: [One sentence summary]

DETAIL: [2-3 sentences explaining the constraint]

IMPACT: [What this means for the engagement, including what can't be done and what changes]

OPTIONS: [2-3 concrete alternatives the user can choose between]

RECOMMENDATION: [Which option the agent recommends and why]
```

### Tier B: Escalation to Manager (who decides if user involvement is needed)

The Manager receives the escalation and determines whether it's an internal adjustment or requires
user input. The Manager escalates to the user if the issue affects scope, precision, or the
client's decision-making.

**Triggers:**
- **Stratum below Minimum Viable N after collection efforts**: data was sought but insufficient
  volume was obtained. The design may need adjustment (merge strata, drop the stratum, accept
  Tier 3 confidence).
- **Source diversity failure**: a stratum's data comes predominantly from a single source despite
  efforts to diversify. This affects confidence tiers but may be acceptable to the user.
- **Collaborative loop exhaustion**: the Sampling Strategist and Source Scout have completed
  three iterations without reconciling the design with data availability. The impasse needs
  Manager arbitration.
- **Sensitivity analysis instability**: the Analyst finds that primary conclusions shift
  materially under reasonable perturbations. The user needs to know the results aren't robust.
- **Rollback trigger**: any situation requiring the rollback protocol (see
  `references/protocols/rollback-protocol.md`).
- **Timeline or resource constraints**: the engagement is taking significantly longer than
  expected and the Manager should decide whether to continue or deliver partial results.

**Escalation format:**
```
ESCALATION [Tier B] from [Agent Name]

ISSUE: [One sentence summary]

DETAIL: [2-3 sentences explaining the situation]

IMPACT ON RESULTS: [How this affects confidence, precision, or scope]

SUGGESTED RESOLUTION: [What the agent recommends]

USER INVOLVEMENT NEEDED? [Agent's assessment of whether this requires the user]
```

### Tier C: Lateral Notification (No escalation, informational)

Routine communication between agents about implementation details. These do not go through the
Manager but are logged in the relevant engagement files for traceability.

**Examples:**
- Source Scout telling Collection & Validation about a source-specific extraction quirk
- Analyst sending a minor data issue back to Collection & Validation for re-cleaning
- Report Composer asking the Sampling Strategist to clarify a design choice for the report

**These become Tier B escalations if:**
- The issue can't be resolved between the two agents after one exchange
- The fix would require changing something outside the receiving agent's write permissions
- The issue reveals a broader problem that affects the engagement design

## Decision Flow

When an agent encounters an issue:

1. **Can I resolve this within my own authority and write permissions?**
   → Yes: Resolve, log in the appropriate engagement file, continue.
   → No: Proceed to step 2.

2. **Can another specialist resolve this through lateral communication?**
   → Yes: Send Tier C notification to the relevant agent.
   → No, or the lateral attempt failed: Proceed to step 3.

3. **Does this affect the engagement design, scope, or what was promised to the user?**
   → Yes: Tier A escalation.
   → No, but it affects quality or confidence: Tier B escalation.

4. **When in doubt, escalate.** A false escalation costs a brief Manager review. A missed
   escalation can contaminate downstream work.

## The Manager's Escalation Responsibilities

When the Manager receives a Tier A or B escalation:

1. **Assess user impact**: does the user need to make a decision, or can the team resolve this
   internally?
2. **Translate for the user**: if user involvement is needed, convert the technical escalation
   into a decision framed in terms the user can act on. Use consequence framing: "This means X
   for your results. Your options are A, B, or C."
3. **Log the decision**: record the escalation, the decision, and the rationale in
   `engagement/decision_log.md`
4. **Route the resolution**: after a decision is made, notify the relevant agents and ensure
   they have updated engagement files to work from

## Quality Guardian Override

The Manager can initiate an escalation to the user even when no agent has triggered one, if the
Manager observes:
- The user requesting something that would compromise methodological integrity
- Cumulative small compromises that individually seem acceptable but collectively degrade the
  engagement quality
- Patterns across agent reports that suggest a systemic issue no single agent would see
  (e.g., every stratum is barely meeting Minimum Viable N, technically acceptable per stratum
  but collectively indicating the engagement is underpowered)
