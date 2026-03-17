# Coverage Map Template

The source analyst writes the coverage map to `engagement/sources/coverage_map.md`. This document maps the source portfolio against the full stratification matrix, showing where coverage exists and where gaps remain.

```markdown
# Coverage Map

## Stratification Matrix Coverage
| Stratum | Target N | Sources | Spec-Matching Yield | Partial/Component Yield | Status |
|---------|----------|---------|---------------------|-------------------------|--------|
| [...]   | [...]    | [IDs]   | [N]                 | [N]                     | Covered / Partial / Uncovered |

## Diversification Assessment
- Platform concentration: [max single-platform share per stratum]
- Channel distribution: [summary]
- Geographic distribution: [summary]

## Gaps Requiring Client Collection
| Gap | Stratum | Observations Needed | Collection Request ID |
|-----|---------|--------------------|-----------------------|
| [...] | [...]  | [N]               | [CR-001]              |

## Gaps Accepted as Unfillable
| Gap | Stratum | Impact on Analysis | Manager Decision |
|-----|---------|-------------------|------------------|
| [...] | [...]  | [description]     | [decision ref in decision_log.md] |
```
