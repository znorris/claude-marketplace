# Sensitivity Analysis Template

Write the sensitivity analysis to `engagement/analysis/sensitivity.md` with the following structure.

## Sections

### Source Sensitivity

Per-stratum table:

| Source Dropped | Revised Estimate | Change from Primary | Direction Change? |
|----------------|------------------|---------------------|-------------------|
| [source name]  | [value (CI)]     | [+/-X%]             | [Yes/No]          |

Flag any source whose removal shifts the estimate by more than 10% or changes the qualitative conclusion.

### Outlier Sensitivity

Results with and without flagged outliers. Report both estimates and the magnitude of difference. Note whether outlier treatment was documented in the cleaning notes or performed during analysis.

### Imputation Sensitivity (if applicable)

Compare results under different missing data treatments:

- Complete-case analysis.
- Mean/median imputation.
- Model-based imputation (e.g., multiple imputation).

Report whether results diverge and what that implies about the missingness mechanism.

### Assembled Observation Sensitivity (if applicable)

For strata where observations were assembled from component data rather than directly observed at the measurement basis:

- Results using all observations (assembled + direct).
- Results using only directly observed observations.
- Magnitude of difference and implications for interpretation.

### Stratum Boundary Sensitivity (if applicable)

Results under alternative stratification cutpoints where boundaries involved judgment calls. Report whether findings are sensitive to reasonable alternative definitions.

### Weighting Sensitivity

Weighted vs. unweighted estimates. Large divergence indicates the sample composition does not match the target population well.

### Summary of Sensitivity Findings

For each primary finding, state:

- Whether the finding is robust or sensitive.
- What it is sensitive to (which test revealed instability).
- The magnitude of sensitivity (minor shifts vs. qualitative change).
