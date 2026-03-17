# Confidence Tier Framework

This protocol defines the system for assessing, assigning, and communicating confidence in engagement findings. The framework evaluates six quality dimensions independently, then derives an overall confidence tier from their combination. All agents involved in analysis and reporting must use this framework consistently.

The framework is adapted from GRADE (Grading of Recommendations, Assessment, Development and Evaluations) and the Navigation Guide for use with secondary data analysis, where data is collected from existing public sources rather than through primary research.

## Assessment Dimensions

Six dimensions, each rated on a four-point concern scale: **No concern**, **Minor concern**, **Serious concern**, **Very serious concern**.

### 1. Statistical Precision

Assesses sampling variability and the adequacy of sample size relative to analytical goals.

Concern anchors:

- No concern: Achieved N meets or exceeds Target N from the feasibility assessment. Confidence intervals are narrow enough for the client's decision-making needs.
- Minor concern: Achieved N approaches but does not fully meet Target N (at or above Minimum Viable N). Confidence intervals are wider than ideal but still informative.
- Serious concern: Achieved N below Minimum Viable N but above Non-Reportable threshold. Estimates have limited precision.
- Very serious concern: Achieved N below Non-Reportable threshold. No reliable estimate can be produced.

### 2. Source Quality

Assesses measurement fidelity, fitness for purpose, and data provenance. This dimension also incorporates the assembled observation modifier.

Concern anchors:

- No concern: Data sources are purpose-compatible with the research question. Measurement basis is verified and aligns with the target variable. Provenance is documented. Assembled observations do not exceed 50% of the stratum's total.
- Minor concern: Characterization is inferred (not verified) for some sources, or minor measurement mismatch exists. Assembled observations present but below 50%.
- Serious concern: Significant measurement mismatch between available data and the target variable, or unknown provenance for key sources. Assembled observations exceed 50% of the stratum's total.
- Very serious concern: Fundamental measurement validity failure. The available data does not measure what the research question asks about. No viable data sources identified after exhaustive search.

### 3. Source Consistency

Assesses agreement and convergence across independent data sources.

Concern anchors:

- No concern: Two or more independent sources per stratum. Dropping any single source shifts the point estimate by less than 10% and does not change the qualitative conclusion.
- Minor concern: Single source of adequate quality, or multiple sources with minor inconsistencies (10-15% shift when one is dropped, but no direction change).
- Serious concern: Heavy reliance on a single source for the majority of observations, or dropping one source shifts estimates by more than 15%, or directional conflict exists across sources.
- Very serious concern: No independent verification possible. All data derives from a single upstream source with no corroboration.

### 4. Coverage

Assesses how well the available data represents the target population, time period, and conceptual scope.

Concern anchors:

- No concern: Target population and period are fully covered by the source portfolio. No significant demographic, geographic, or temporal gaps.
- Minor concern: Minor gaps in coverage that do not systematically bias results. Some segments are slightly underrepresented.
- Serious concern: Significant demographic, geographic, or temporal gaps that affect representativeness. The covered population differs meaningfully from the target population in ways that could bias estimates.
- Very serious concern: The covered population fundamentally differs from the target population. Results cannot generalize to the intended scope.

### 5. Data Completeness

Assesses the rate and mechanism of missing data.

Concern anchors:

- No concern: Missingness is less than 5% on key variables and is plausibly Missing Completely At Random (MCAR).
- Minor concern: Missingness between 5% and 15%. Mechanism is plausibly Missing At Random (MAR) with appropriate treatment applied (imputation or weighting).
- Serious concern: Missingness exceeds 15%, or Missing Not At Random (MNAR) patterns are suspected based on domain knowledge or diagnostic analysis. Bias from missingness cannot be fully corrected.
- Very serious concern: Pervasive missingness on key variables that compromises any analysis. Mechanism is likely MNAR with no viable correction strategy.

### 6. Robustness

Assesses the stability of results across sensitivity analyses and alternative analytical choices.

Concern anchors:

- No concern: Results are stable under all sensitivity tests. Dropping any single source, changing imputation method, including or excluding outliers, and varying stratum boundaries all produce shifts of less than 10% with no direction change.
- Minor concern: Results are directionally stable but show moderate sensitivity (10-25% shift) under some conditions. The qualitative conclusion holds.
- Serious concern: Results shift by more than 25% or change direction under reasonable perturbations. The finding depends materially on specific analytical choices.
- Very serious concern: Results are unstable under most perturbations. No robust estimate can be derived from the available data.

## Starting Level

For secondary observational data (publicly available sources, administrative records, scraped data), the starting confidence level is **Moderate**. This follows the Navigation Guide's rationale: GRADE's default of Low for all observational studies is overly conservative when the data sources are well-characterized and the analysis is transparently conducted. Starting at Moderate acknowledges that secondary data analysis lacks the control of experimental design while recognizing that rigorous source evaluation, sensitivity testing, and transparent documentation can support meaningful findings.

## Tier Derivation

The overall confidence tier is determined by matching the dimension profile against the tier definitions below. For secondary observational data, the default expectation is Moderate (Tier 2); achieving Tier 1 requires demonstrating that all dimensions have been rigorously assessed and found to have no or only minor concerns.

### Tier Definitions

**Tier 1: High Confidence**
All dimensions at no concern or minor concern. No dimension at serious or very serious concern. For secondary data, this tier represents the ceiling: it requires multiple independent sources, adequate samples, verified measurement, full coverage, low missingness, and stable sensitivity results. Estimates are likely to generalize to the target population within stated margins of error. Suitable for decision-making.

**Tier 2: Moderate Confidence**
No dimension at very serious concern, and no more than two dimensions at serious concern. This is the expected tier for well-conducted secondary data analysis where some limitations are present but contained. Estimates are informative and directionally reliable. Suitable for informed decision-making with awareness of the specific documented limitations.

**Tier 3: Low Confidence / Indicative Only**
Any dimension at very serious concern, or three or more dimensions at serious concern. Results suggest patterns or ranges but should not be used for firm decision-making without additional data. The findings are indicative; they tell you where to look, not what to conclude.

**Tier 4: Insufficient / Not Reportable**
Statistical Precision at very serious concern (N below Non-Reportable threshold), OR Source Quality at very serious concern with no viable data sources, OR two or more dimensions at very serious concern. No reliable estimate can be produced. The report should explicitly state that this stratum or parameter could not be estimated and explain what data would be needed.

## Dimension Profile Reporting

Both the overall tier and the individual dimension ratings are reported for every finding. This allows readers to distinguish qualitatively different confidence profiles. For example:

- "Tier 2 due to source consistency (serious: single source)" conveys a different risk than "Tier 2 due to data completeness (serious: 18% MNAR missingness)"
- The dimension profile enables targeted follow-up: improving source diversity is a different remediation than improving data completeness

The statistical analyst documents the full dimension profile in `engagement/analysis/confidence_assessment.md`. The report composer presents both the tier and the most material dimension concerns in a register-appropriate format.

## Aggregate Roll-Up

Aggregate findings (e.g., a population-level mean computed from stratified estimates) derive their confidence assessment from the contributing strata using a dimension-first aggregation procedure:

1. For each of the six dimensions, compute the weighted concern level across contributing strata using the stratum weights from the sampling design.
2. If any stratum contributing more than 25% of the weighted estimate has **serious concern** on a dimension, the aggregate inherits at least **minor concern** on that dimension.
3. If any stratum contributing more than 25% has **very serious concern** on a dimension, the aggregate inherits **serious concern** on that dimension.
4. If any stratum contributing more than 10% of the weighted estimate is Tier 4, the aggregate must disclose the gap explicitly on every dimension that stratum affects.
5. If more than 50% of contributing weight comes from strata at Tier 3 or below, the aggregate is Tier 3.
6. Derive the overall aggregate tier by matching the aggregated dimension ratings against the tier definitions, using the same rules as stratum-level assessment.

Document the per-stratum contributions and how they determine each aggregate dimension rating.

## Edge Cases

- **Merged strata**: If two strata were merged during design adjustment, each dimension for the merged stratum reflects the weaker of the two component ratings, unless the merge was pre-planned and the feasibility assessment accounts for the combined cell.
- **Client-supplied data**: Data collected by the client is assessed on the same six dimensions. Client data from a single source with no independent verification carries source consistency risk. Client data collected without documented methodology carries source quality risk.
- **Proxy variables**: If a stratification variable was proxied (e.g., using ZIP-code median income instead of school-level economic data), this is recorded as a coverage concern (conceptual indirectness between the proxy and the intended construct) and may also affect source quality if the proxy's measurement fidelity is uncertain.

## Presentation in Reports

The report composer translates the dimension profile and overall tier into reader-appropriate language. The key requirements are:

- Every quantitative finding carries its overall tier and the most material dimension concerns.
- Findings with serious or very serious concern on any dimension include an explanation of the affected dimensions and their impact.
- The limitations section provides the full dimension profile for each non-Tier-1 finding, tracing each concern to its source in the engagement.
- Aggregate tiers include a breakdown showing per-stratum contributions and per-dimension aggregation.
