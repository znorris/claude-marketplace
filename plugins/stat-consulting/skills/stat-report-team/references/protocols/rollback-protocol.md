# Rollback Protocol

This protocol governs how the engagement team handles situations where previously accepted data
or design decisions are found to be problematic and must be reversed. The goal is to remove
contaminated artifacts cleanly, preserve audit trails, and re-invoke downstream agents against
a corrected state, without polluting any agent's context with invalidated information.

## Rollback Severity Levels

### Level 1: Data-Level Rollback

**Trigger**: A specific data source or dataset is found to be unreliable, biased, or erroneous
after it has already been incorporated into the working data.

**Examples**:
- The Analyst discovers during sensitivity analysis that a single source is driving the primary
  finding (outsized influence suggesting measurement artifact or systematic bias)
- The data manager identifies post-hoc that a source rated as independent actually shares
  an upstream database with another source already in the dataset
- A data extraction error is discovered that systematically affected values from one source

**Scope of impact**: Source Analyst (needs to find replacement), collection specialist (re-fetch if replacement source found), data manager (re-validate and re-process), Analyst (needs to re-run). Sampling Strategist and Design Architect are unaffected.

### Level 2: Design-Level Rollback

**Trigger**: A constraint or discovery forces a change to the sampling design, stratification
scheme, or research specification scope.

**Examples**:
- A stratum is confirmed as permanently unfillable, requiring the design to be restructured
- The Analyst discovers that the stratification variable doesn't map to real-world data structures
  (e.g., the assumed market segmentation doesn't exist in practice)
- Cross-source duplication is so pervasive that the effective sample size is far below the
  designed N, requiring a fundamental reassessment

**Scope of impact**: Cascades upward to the Sampling Strategist and potentially the Design
Architect. All downstream agents must re-engage after the design is revised.

## Rollback Execution Sequence

The **Manager** executes all rollbacks. No individual agent initiates a rollback unilaterally;
agents flag the trigger, and the Manager coordinates the response.

### Step 1: Document the Trigger

The Manager creates a timestamped entry in `engagement/decision_log.md`:

```markdown
## ROLLBACK [RB-NNN], [Date/Time]

### Trigger
- **Flagged by**: [Agent name]
- **Severity**: Level [1/2]
- **Description**: [What was discovered]
- **Evidence**: [How it was discovered, e.g., sensitivity analysis result, validation finding]

### Affected Artifacts
- [List of specific files that contain contaminated data or invalidated decisions]

### Decision
- [What action is being taken and why]
- [If Level 2: summary of the design change required]
```

### Step 2: Archive Contaminated Artifacts

Move all affected files from their active locations to `engagement/archive/`:

- Use naming convention: `RB-NNN_[original_filename]`
- Preserve the original file completely (do not modify archived copies)
- Create a `RB-NNN_manifest.md` in the archive listing all moved files and their original paths

Example:
```
engagement/archive/
├── RB-001_manifest.md
├── RB-001_coastal_pricing.csv
├── RB-001_primary_results.md
└── RB-001_sensitivity.md
```

**CRITICAL**: After archiving, the active engagement folder must not contain any reference to the
contaminated data. The coverage map, validation log, and any analysis files that incorporated the
bad data must be updated or regenerated.

### Step 3: Update Active Engagement Files

**For Level 1 (Data-Level):**
- `engagement/sources/coverage_map.md`: mark the affected stratum as having a re-opened gap
- `engagement/sources/inventory.md`: update the affected source entry with the quality issue
- `engagement/data/validation_log.md`: add a rejection entry for the contaminated data
- `engagement/data/datasets/`: remove the contaminated data files (archived in Step 2)

**For Level 2 (Design-Level):**
All Level 1 updates, plus:
- `engagement/sampling/design.md`: the Sampling Strategist produces a revised version; the old
  version is archived
- `engagement/sampling/power_analysis.md`: updated if sample size requirements change
- `engagement/sampling/variables.md`: updated if stratification variables change
- `engagement/research_spec.md`: updated only if the research question scope changes (requires
  client approval)

### Step 4: Notify Affected Agents

Each agent that needs to re-engage receives a **rollback brief**, a concise notification
specifying:

1. What changed and why (one paragraph)
2. Which engagement folder files have been updated (so the agent knows what to re-read)
3. What the agent needs to do in response

**Level 1 notification targets:**
- Source Analyst: "A data source has been rolled back. The coverage map shows a re-opened gap in
  [stratum]. Please identify a replacement source or confirm the gap is unfillable."
- Collection Specialist: "If a replacement source is found, re-fetch the data per the revised collection plan."
- Data Manager: "Re-validate and re-process affected datasets once replacement data is available.
  The contaminated data has been archived."
- Analyst: "Previous analysis results have been archived. Once replacement data is available (or
  the gap is confirmed), re-run the analysis by reading the current state of the data/ folder."

**Level 2 notification targets:**
All Level 1 targets, plus:
- Sampling Strategist: "The sampling design requires revision due to [reason]. Please update
  design.md, power_analysis.md, and variables.md as appropriate."
- Manager to Client: "A constraint has been encountered that requires adjusting the approach. Here
  are the options..." (Manager handles the client communication)

### Step 5: Reset Downstream Agents

**This is the critical context hygiene step.**

Downstream agents whose prior work was invalidated must get a clean context. The Manager kills the affected team agent(s) and spawns fresh replacements on the same team. The new agent reads the current state of the engagement folder, which has been updated in Steps 2 and 3 to reflect the corrected state. It does not inherit the prior agent's conversation history.

For Level 2 rollbacks, always reset affected agents. For Level 1 rollbacks, use judgment: if the rollback invalidates a small, isolated piece of the agent's work, a SendMessage with corrected instructions may suffice. If in doubt, reset.

The decision log preserves the full history (including the rollback) for the Manager and the final report's limitations section. Working agents see only current truth.

### Step 6: Record Resolution

Once the rollback is fully resolved (replacement data found, analysis re-run, or gap accepted),
the Manager adds a resolution entry to the decision log:

```markdown
### Resolution [RB-NNN], [Date/Time]
- **Outcome**: [Replacement source found / Gap accepted / Design revised]
- **Impact on final report**: [How this rollback affects the confidence tiers or limitations]
- **Downstream agents re-engaged**: [List]
```

## Iteration Limits

- **Level 1 rollbacks**: Up to 3 per stratum before the Manager escalates to the client with a
  recommendation to either accept the gap or adjust scope.
- **Level 2 rollbacks**: Each one requires client involvement. More than 2 Level 2 rollbacks in a
  single engagement should trigger a feasibility re-assessment; the engagement may be attempting
  something that isn't achievable with available resources.

## Rollback and the Final Report

The Report Composer must reference rollbacks in the limitations section when they materially
affect the findings. The reader should know:
- That a data source or design element was revised during the engagement
- Why the revision was necessary
- How the final results differ from what would have been produced without the rollback
- Whether the replacement data or revised design is as strong as the original plan

This transparency is part of the engagement's integrity guarantee.
