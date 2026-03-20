# Source Inventory Template

The source analyst writes the source inventory to `engagement/sources/inventory.md`. This document records every source evaluated during the source landscape phase, whether included or excluded.

## Included Sources

For each source included in the active inventory:

```markdown
## Source [ID]: [Source Name]
- URL: [if applicable]
- Type: [primary/secondary/aggregator/government/etc.]
- Coverage: [which strata, estimated volume]
- Recency: [date of data / update frequency]
- Quality Ratings: Coverage [S/A/W] | Recency [S/A/W] | Accuracy [S/A/W] | Bias [S/A/W]
- Independence: [independent / shares upstream with Source X]
- Data retrieval method: [static HTML / JS-rendered / requires cart interaction / login-gated]
- Access method: [web scrape / API / download / manual]
- Notes: [any caveats, access issues, or special considerations]
```

## Excluded Sources

Sources evaluated but not included in the active inventory. Every exclusion must be documented.

```markdown
### Excluded Source [ID]: [Source Name]
- URL: [if applicable]
- Observed data format: [what the source actually publishes, per characterization claim]
- Evidence basis: [verified / inferred]
- Reason for exclusion: [why it was not included]
- Strata it could have served: [which strata in the sampling design]
- Partial relevance assessment: [could this source fill gaps for specific strata or tiers, even if not ideal? If yes, describe what it could contribute and under what caveats.]
```
