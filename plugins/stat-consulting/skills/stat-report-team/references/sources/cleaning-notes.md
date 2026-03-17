# Cleaning Notes Template

The collection & validation specialist writes the cleaning notes to `engagement/data/cleaning_notes.md`. This document logs every transformation applied to the data, stratum assignments, and known issues.

```markdown
# Cleaning Notes

## Transformation Log
| Step | Operation | Records Affected | Rationale |
|------|-----------|-----------------|-----------|
| 1    | [...]     | [N]             | [...]     |

## Stratum Assignment Summary
| Stratum | Observations Assigned | Quality Flag |
|---------|----------------------|--------------|
| [...]   | [N]                  | GREEN/YELLOW/RED |

## Outlier Flagging Summary

| Stratum | Flagged Count | Typical Distance | Plausibility Note |
|---------|---------------|-----------------|-------------------|
| [...]   | [N]           | [X SD / Y IQR]  | [brief reason values are plausible] |

Plausible outliers are flagged with `_outlier_flag=true` in the datasets and are not removed during cleaning. Disposition (retain, winsorize, or exclude) is handled by the statistical analyst during the analysis phase.

## Known Issues

[Any unresolved data quality issues, with assessment of impact]
```
