# Confidence Tier Framework

This protocol defines the system for assessing, assigning, and communicating statistical confidence
in engagement findings. All agents involved in analysis and reporting must use this framework
consistently.

## Tier Definitions

### Tier 1: High Confidence

**Criteria (all must be met):**
- Sample size meets or exceeds the Target N from the power analysis
- Data from **two or more independent sources** per stratum
- No unresolved quality flags (GREEN status from Collection & Validation)
- Sensitivity analyses show **stable results**: dropping any single source shifts the point
  estimate by less than 10% and does not change the qualitative conclusion
- Missingness is <5% and assessed as MCAR

**Interpretation**: The estimates are likely to generalize to the target population within stated
margins of error. Suitable for decision-making.

### Tier 2: Moderate Confidence

**Criteria (one or more of the following):**
- Sample size approaches but does not fully meet Target N (at or above Minimum Viable N)
- One or more strata rely on a **single source** (even if that source is high quality)
- Minor quality flags present (YELLOW status) that have been assessed and judged non-critical
- Sensitivity analyses show results are **directionally stable** but point estimates shift by
  10 to 25% under some conditions
- Missingness between 5 and 15%, assessed as MCAR or MAR with appropriate treatment

**Interpretation**: The estimates are informative and directionally reliable. Interpret with
awareness of the specific documented limitations. Suitable for informed decision-making with
caveats.

### Tier 3: Low Confidence / Indicative Only

**Criteria (one or more of the following):**
- Sample size below Minimum Viable N but above Non-Reportable threshold
- Heavy reliance on a single source for the majority of observations
- Significant quality flags (RED status) or unresolved data quality concerns
- Sensitivity analyses reveal **instability**: results shift >25% or change direction under
  reasonable perturbations
- Missingness >15%, or MNAR patterns detected without adequate correction

**Interpretation**: The results suggest patterns or ranges but should not be used for firm
decision-making without additional data. The findings are indicative; they tell you where to
look, not what to conclude.

### Tier 4: Insufficient / Not Reportable

**Criteria (any one is sufficient):**
- Sample size below the Non-Reportable threshold
- Data quality so compromised that any estimate would be misleading
- No viable data sources identified for the stratum after exhaustive search and user collection
  attempts
- Fundamental measurement validity concerns (e.g., the available data doesn't actually measure
  what the research question asks about)

**Interpretation**: No reliable estimate can be produced. The report should explicitly state that
this stratum or parameter could not be estimated and explain what data would be needed to achieve
reportable results. Producing a number here would be worse than producing no number.

## Assignment Procedure

### Stratum-Level Assignment

The Analyst assigns tiers at the stratum level by evaluating each criterion dimension:

| Dimension | Assessment Method |
|-----------|------------------|
| Sample adequacy | Compare achieved N to power analysis thresholds |
| Source diversity | Count independent sources; check for shared upstream |
| Data quality | Review Collection & Validation quality flags |
| Sensitivity stability | Run source-drop and perturbation analyses |
| Missingness | Review validation log missingness assessment |

The tier is determined by the **most limiting dimension**. A stratum that meets Tier 1 on four
dimensions but Tier 3 on one dimension is rated Tier 3, with the limiting factor documented.

### Aggregate Roll-Up

Aggregate findings (e.g., a population-level mean computed from stratified estimates) derive
their confidence tier from the contributing strata:

1. Compute each stratum's **contribution weight** to the aggregate estimate
2. Identify the tier of each contributing stratum
3. Apply the roll-up rules:
   - If all contributing strata are Tier 1 → aggregate is Tier 1
   - If any stratum contributing **>25%** of the weighted estimate is Tier 2 → aggregate is
     capped at Tier 1 only if remaining evidence is exceptionally strong; otherwise Tier 2
   - If any stratum contributing **>25%** is Tier 3 → aggregate cannot exceed Tier 2
   - If any stratum contributing **>10%** is Tier 4 → aggregate must disclose the gap explicitly
     and cannot exceed Tier 2; if the Tier 4 stratum contributes **>25%**, aggregate is Tier 3
   - If **>50%** of contributing weight is Tier 3 or below → aggregate is Tier 3

4. Document the roll-up logic for the Report Composer

### Edge Cases

- **Merged strata**: If two strata were merged during design adjustment, the confidence tier of
  the merged stratum reflects the weaker of the two components, unless the merge was pre-planned
  and the power analysis accounts for the combined cell.
- **User-supplied data**: Data collected by the user is not inherently lower-tier, but it should
  be assessed for the same quality dimensions. User data from a single source with no independent
  verification carries source diversity risk.
- **Proxy variables**: If a stratification variable was proxied (e.g., using ZIP-code median income
  instead of school-level economic data), note this as a methodological limitation that may prevent
  Tier 1 assignment for affected strata.

## Presentation in Reports

The Report Composer translates tiers into reader-appropriate language. See the Report Composer
reference file for register-specific examples. The key requirements are:
- Every quantitative finding carries its tier visibly
- Tier 2+ findings include a brief explanation of the limiting factor
- The limitations section provides the full traceability chain for each non-Tier-1 finding
- Aggregate tiers include a breakdown showing per-stratum contributions
