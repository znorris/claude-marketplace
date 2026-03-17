# Primary Results Template

Write the primary results to `engagement/analysis/primary_results.md` with the following structure.

## Sections

### Data Audit Summary

- Achieved N per stratum vs. target N, minimum viable N, and non-reportable threshold from the power analysis.
- Quality flag distribution (GREEN, YELLOW, RED counts across strata).
- Distributional notes (normality, skewness, multimodality, heavy tails).

### Per-Stratum Results

One subsection per stratum. Each includes:

- **N**: achieved sample size.
- **Point estimate**: value with confidence interval (CI lower, CI upper).
- **Standard error**: value.
- **Sources contributing**: list with observation counts per source.
- **Distribution**: summary (normal, skewed, multimodal, etc.).
- **Design adjustments**: any weights, design effects, or corrections applied.

### Aggregate Results

One subsection per aggregate finding. Each includes:

- **Weighted estimate**: value with confidence interval.
- **Weighting method**: description of how stratum weights were derived.
- **Contributing strata**: list with each stratum's weight and contribution.

### Comparison Results (if applicable)

For group comparison analyses:

- Test used, test statistic, p-value.
- Effect size with interpretation.
- Multiple comparison correction applied (if any).
