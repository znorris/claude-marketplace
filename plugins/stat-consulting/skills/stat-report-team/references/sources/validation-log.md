# Validation Log Template

The data manager writes the validation log to `engagement/data/validation_log.md`. This document records the results of validation checks for each dataset processed.

```markdown
# Validation Log

## Dataset: [Source ID / Submission ID]

### Completeness
- Records received: [N]
- Records passing all required fields: [N]
- Missing data rates: [per field]

### Consistency
- Range violations: [count, description]
- Coding errors corrected: [count, description]
- Unit inconsistencies resolved: [count, description]

### Duplication
- Exact duplicates removed: [count]
- Near-duplicates resolved: [count, resolution method]
- Cross-source duplicates identified: [count, resolution method]

### Outliers
- Error outliers corrected or removed: [count, with original values and corrections]
- Plausible outliers flagged (`_outlier_flag=true`): [count, by stratum]
  - [For each: stratum, observed value, distance from median in SD and IQR, plausibility note]
- Plausible outlier disposition is deferred to the statistical analyst

### Missingness
- Structural vs. incidental classification: [per field with >5% missingness]
- MCAR test result: [Little's test statistic and p-value if applicable, or "not applicable" with reason]
- MAR plausibility assessment: [covariates that predict missingness, if logistic regression was performed]
- Per-variable mechanism assessment:
  - [Field]: [MCAR/MAR/MNAR], [supporting rationale], [structural or incidental]
- Impact on analysis: [description]
```
