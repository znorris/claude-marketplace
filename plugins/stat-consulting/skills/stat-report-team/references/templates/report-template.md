# Standard Report Template

This template is the default structure for engagement deliverables. The Report Composer may modify
it based on client preferences, research question type, and register (executive, professional, or
technical). However, all sections marked **[MANDATORY]** must appear in every report regardless of
modifications.

---

```markdown
# [Report Title]

**Prepared by**: Statistical Consulting Engagement System
**Engagement ID**: [ID]
**Date**: [date]
**Confidence Summary**: [Overall tier, e.g., "Tier 1 (High Confidence) for primary findings;
see per-finding detail below"]

---

## Executive Summary [MANDATORY]

[2 to 4 paragraphs summarizing:]
- What question this report answers
- The key findings (with confidence tiers stated plainly)
- The most important limitations or caveats
- The bottom line for decision-making

[The executive summary must be an honest representation of the findings, neither more optimistic
nor more pessimistic than the full report. A reader who reads only this section should not be
misled.]

---

## Research Question

### Question Addressed
[Plain-language restatement of the research question]

### Population and Scope
[Who/what is included, geographic and temporal scope, key exclusions and why]

### Variables Measured
[The outcome variable(s) in accessible terms]

---

## Methodology [MANDATORY]

### Sampling Approach
[Description of the sampling strategy: stratification variables, why they were chosen
(consequence framing), and the overall design logic]

### Data Sources
[Summary of the source portfolio: how many sources, what types, how they were selected.
Emphasis on diversification. Full source details in the appendix.]

### Sample Achieved
[Summary table: strata, target N, achieved N, quality flag. Highlight any strata that fell short.]

### Analytical Methods
[What statistical methods were used and why. For executive register, keep this to 1 to 2 sentences.
For technical register, include formulas and assumptions.]

---

## Findings [MANDATORY]

### Overall Results
[The primary findings with confidence tiers. Each quantitative claim carries its tier visually.]

[Structure depends on the research question type:]

**For estimation studies:**
- Present the aggregate estimate with confidence interval and tier
- Break down by stratification variable(s) with per-stratum estimates

**For comparative studies:**
- Present the comparison with effect sizes and significance
- Break down by subgroups if relevant

**For distributional studies:**
- Present summary statistics and distributional shape
- Highlight notable patterns (bimodality, heavy tails, outlier clusters)

### Detailed Breakdown
[Per-stratum results, presented in tables or narrative depending on register]

### Key Patterns and Observations
[Any notable patterns that emerged, e.g., "Rural schools show significantly higher variability
in pricing, suggesting a less standardized market." Always tie observations to evidence.]

---

## Confidence Assessment [MANDATORY]

### Tier Summary
[Table mapping each finding/stratum to its confidence tier with brief rationale]

| Finding / Stratum | Confidence Tier | Key Factors |
|-------------------|----------------|-------------|
| [Overall finding] | [Tier X]       | [Brief explanation] |
| [Stratum A]       | [Tier X]       | [Brief explanation] |
| [Stratum B]       | [Tier X]       | [Brief explanation] |

### Tier Definitions
[Include the tier definitions so the reader knows what each tier means. Adapt language to
register.]

### What Would Strengthen These Findings
[For any non-Tier-1 finding: what additional data or methodological steps would increase
confidence. This gives the client an actionable path forward if they want higher certainty.]

---

## Robustness and Sensitivity [MANDATORY; may be abbreviated for executive register]

[Summary of sensitivity analyses performed and their results.]

### Tests Performed
[Brief description of each sensitivity check]

### Results
[Which findings are robust across all checks, which are sensitive to specific conditions]

[For executive register: 1 paragraph summary. For technical register: full tables of
source-dropped estimates, outlier-included/excluded comparisons, etc.]

---

## Limitations [MANDATORY]

[This section must be substantive, specific, and traceable. Generic disclaimers are not
acceptable.]

### Sampling Limitations
[Underpowered strata, non-probability elements, design compromises and their impact]

### Data Quality Limitations
[Source concentration, missingness patterns, temporal gaps, measurement concerns]

### Scope Limitations
[What was intentionally excluded and what the reader should not infer from these results]

### Methodological Limitations
[Key assumptions, proxy variables used, alternative approaches not taken]

### Rollback History (if applicable)
[Any data or design rollbacks that occurred during the engagement, what triggered them, and how
the final results were affected]

---

## Data Sources [MANDATORY]

[Attribution for every source used in the analysis]

| Source | Type | Coverage | Quality Rating | Role in Analysis |
|--------|------|----------|---------------|-----------------|
| [Name/URL] | [Primary/Secondary/etc.] | [What strata] | [S/A/W composite] | [How it was used] |

---

## Appendices [Optional; include based on register and complexity]

### A. Full Per-Stratum Results
[Detailed tables of all stratum-level statistics]

### B. Sensitivity Analysis Detail
[Full source-dropped estimates, imputation comparisons, etc.]

### C. Variable Definitions and Coding Schemes
[From the Data Requirements Manifest]

### D. Data Cleaning Summary
[Key transformations applied]

### E. Glossary of Statistical Terms
[For executive and professional registers: define confidence interval, stratum, sensitivity
analysis, margin of error, and other terms used in the report. The report body should use full
terms on first use; abbreviations may be used after the first expansion.]
```

---

## Template Modification Guidelines

The Report Composer may modify this template under these conditions:

**Permitted modifications:**
- Reorder non-mandatory sections for narrative flow
- Add sections relevant to the specific research question
- Remove optional appendices that aren't needed
- Adjust depth and detail to match the register
- Add visualizations (charts, maps) where they improve comprehension

**Not permitted:**
- Removing any MANDATORY section
- Presenting findings without confidence tiers
- Omitting the limitations section or reducing it to generic disclaimers
- Presenting the executive summary in a way that's inconsistent with the body
- Removing data source attribution
