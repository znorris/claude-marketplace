# Power Analysis

## Parameters

- Significance level: [alpha]
- Target power: [1 - beta]
- Expected achievable N per stratum: [from source analyst reconnaissance]

## Design Effect Assessment

- Data hierarchy: [describe nesting structure, e.g., "products within stores within school districts"]
- ICC source: [published estimate with citation / empirical estimate from pilot data / conservative default]
- ICC value: [value]
- Mean cluster size: [m]
- Computed DEFF: [1 + (m - 1) x ICC]
- DEFF inflation factor: [percentage applied for heterogeneous weights, e.g., 20%]
- Effective N per stratum: [raw N / adjusted DEFF]

### DEFF Sensitivity

| DEFF | Effective N | MDE at Target Power |
|------|-------------|---------------------|
| 1.0  | [value]     | [value]             |
| 1.5  | [value]     | [value]             |
| 2.0  | [value]     | [value]             |
| 3.0  | [value]     | [value]             |

## Per-Stratum Sensitivity Power Analysis

[For each stratum: expected achievable N from source reconnaissance, effective N after DEFF adjustment, MDE at target power, assessment of whether MDE is substantively meaningful for the client's decision-making needs, any adjustments made]

## Thresholds

[For each stratum: Target N (80% power for meaningful effect), Minimum Viable N (60% power), Non-Reportable threshold]

## Assumptions and Sensitivity

[What assumptions were made about variance, non-response, design effect?
How sensitive is the MDE to these assumptions?
What would change if the DEFF assumption is wrong?]

## Extraction Yield Degradation

Store-count targets assume full price extractability from accessible stores. If extraction
rates fall below 100%, analytical power degrades as follows:

| Extraction Yield | Effective N per Stratum | MDE at Target Power | Power at Original MDE |
|-----------------|------------------------|--------------------|-----------------------|
| 100%            | [value]                | [value]            | [value]               |
| 75%             | [value]                | [value]            | [value]               |
| 50%             | [value]                | [value]            | [value]               |
| 25%             | [value]                | [value]            | [value]               |
