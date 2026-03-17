# Confidence Assessment Template

Write the confidence assessment to `engagement/analysis/confidence_assessment.md` with the following structure.

## Sections

### Per-Stratum Dimension Profile

Table with one row per stratum, showing the concern rating for each of the six assessment dimensions:

| Stratum | Overall Tier | Precision | Source Quality | Consistency | Coverage | Completeness | Robustness | Limiting Dimension(s)                    |
| ------- | ------------ | --------- | -------------- | ----------- | -------- | ------------ | ---------- | ---------------------------------------- |
| [name]  | [1-4]        | [concern] | [concern]      | [concern]   | [concern]| [concern]    | [concern]  | [dimensions at serious or worse]         |

Each cell contains the concern level (no concern, minor, serious, or very serious) with a brief supporting note. Example: "minor: single source but high quality" or "serious: 18% missingness, suspected MNAR."

The overall tier is derived by matching the dimension profile against the tier definitions in the confidence tier framework. Document the derivation for every non-Tier-1 stratum: which dimensions are at serious or very serious concern and how those map to the tier.

### Aggregate Finding Dimension Profile

Table with one row per aggregate finding, showing the aggregated concern level for each dimension:

| Finding | Overall Tier | Precision | Source Quality | Consistency | Coverage | Completeness | Robustness | Constraining Strata                      |
| ------- | ------------ | --------- | -------------- | ----------- | -------- | ------------ | ---------- | ---------------------------------------- |
| [name]  | [1-4]        | [concern] | [concern]      | [concern]   | [concern]| [concern]    | [concern]  | [strata that constrained each dimension] |

### Roll-Up Logic

For each aggregate finding, document how the aggregate dimension profile was derived from the per-stratum profiles:

- For each dimension: which strata contributed concern ratings, how the weighted aggregation was computed, and what the resulting aggregate concern level is.
- Which strata constrained the aggregate tier on which dimensions.
- Whether any Tier 4 strata contribute more than 10% of the weight (requires explicit disclosure per affected dimension) or more than 25% (caps aggregate at Tier 3).
- The overall aggregate tier derivation from the aggregated dimension profile.

### Tier Upgrade Paths (optional)

For non-Tier-1 findings, note what would be needed to reduce concern on each limiting dimension to achieve a higher tier. This informs the report's limitations section and any follow-on engagement scoping.
