# Collection Request Format

This protocol defines the standard format for data collection requests issued to the user when
automated data acquisition cannot fill a coverage gap. These requests must be specific, actionable,
and self-contained: the user should be able to fulfill the request without additional guidance.

## Request File Naming

Save each request as `engagement/sources/collection_requests/CR-NNN.md` where NNN is a
sequential number.

## Required Sections

### Header

```markdown
# Collection Request CR-[NNN]

**Date issued**: [date]
**Issued by**: Source Scout
**Stratum**: [which stratum this fills]
**Priority**: [High / Medium / Low, based on impact on overall engagement quality]
**Status**: [Open / In Progress / Fulfilled / Partially Fulfilled / Unfillable]
```

### Data Needed

A plain-language description of the data gap, written so a non-technical user understands what
they're looking for.

> The engagement needs retail pricing data for branded athletic apparel sold through school-operated
> fan stores at rural high schools (NCES locale codes 31, 32, 33, 41, 42, 43). Automated sources
> cover urban and suburban schools well but have almost no representation of rural school stores.

### Why It Matters

A brief explanation of the analytical consequence: what this data enables and what happens without
it. Frame in terms of the user's objectives.

> With this data, the analysis can report confident pricing estimates for rural schools and include
> them in the national average with full weight. Without it, the rural stratum will carry a "Low
> Confidence / Indicative Only" rating, and the national average will carry a caveat that rural
> schools are underrepresented.

### Specific Sources to Check

A numbered, prioritized list of specific places to look. Include URLs where possible. Explain what
to look for at each source.

> **Check these sources in order:**
>
> 1. **[School Name] Athletics Store**, [URL]
>    Look for: apparel section, note the listed retail price for each item. Focus on t-shirts,
>    hoodies, and caps.
>
> 2. **[School Name] Booster Club Shop**, [URL]
>    Look for: team merchandise tab. Some items may be listed with fundraiser markup. Note
>    if the listing indicates "fundraiser pricing."
>
> 3. **[Platform Name]**, [URL]
>    Search for: "[School Name] [Sport] apparel"
>    Note: This platform sometimes shows wholesale pricing. Only retail (consumer-facing)
>    prices are needed.

Provide at least 3 sources per request when possible, ranked by expected usefulness.

### Fields to Collect

A precise list of data points to record for each item found. Include definitions for any field
that could be ambiguous.

> For each item, record:
>
> | Field | Description | Example |
> |-------|-------------|---------|
> | school_name | Full name of the school | "Lincoln Rural High School" |
> | school_state | Two-letter state code | "MT" |
> | product_name | Item name as listed | "Varsity Football Hoodie" |
> | product_category | One of: T-shirt, Hoodie, Cap, Jersey, Other | "Hoodie" |
> | sport | The sport the item is branded for | "Football" |
> | retail_price | Listed price in USD (before tax, before shipping) | 42.99 |
> | source_url | The page URL where you found this item | "https://..." |
> | date_collected | The date you recorded this price | "2025-03-15" |
> | notes | Anything unusual (sale price, bundle deal, out-of-stock, etc.) | "Marked 20% off" |

### Return Format

A template the user fills in. Prefer simple table or CSV format.

> Please return the data in one of these formats:
>
> **Option A: Copy this table and fill it in.**
>
> | school_name | school_state | product_name | product_category | sport | retail_price | source_url | date_collected | notes |
> |-------------|-------------|--------------|-----------------|-------|-------------|-----------|---------------|-------|
> | | | | | | | | | |
>
> **Option B: A CSV file with these exact column headers.**
> `school_name,school_state,product_name,product_category,sport,retail_price,source_url,date_collected,notes`

### Acceptance Criteria

The minimum submission that makes this data usable. Be explicit about thresholds.

> **Minimum viable submission:**
> - At least **15 product observations** from at least **3 different schools**
> - Schools must be in **at least 2 different states**
> - At least **2 product categories** represented
>
> **Ideal submission:**
> - 30+ observations from 6+ schools across 3+ states
>
> Fewer observations are still useful; even 5 items help characterize the range. But below 15,
> it is not possible to compute meaningful confidence intervals for this stratum.

### Troubleshooting

Anticipate common problems and provide solutions.

> **If a school store website is down or unavailable:**
> Skip it and try the next source on the list. Note which sites were unavailable so that
> alternative approaches can be attempted.
>
> **If prices aren't listed (e.g., "contact for pricing"):**
> Record the item with retail_price left blank and note "price not listed" in the notes field.
> These are still useful for understanding the product assortment.
>
> **If you're unsure whether an item qualifies:**
> Include it and describe your uncertainty in the notes field. It is better to have an extra
> observation that can be evaluated than to miss a valid one.
>
> **If a source requires login or payment:**
> Do not create accounts or pay for access. Skip the source and note the barrier. An
> alternative will be found.
>
> **If the data looks very different from what's described here:**
> Report what you are seeing. The search strategy may need adjustment.

## Source Scout's Obligations After Issuing a Request

- Monitor for user submissions in `engagement/sources/user_submissions/`
- Validate submissions against the acceptance criteria promptly
- If a submission has issues, communicate specifically what needs correction
- If the user reports difficulty, provide alternative sources or simplified instructions
- Update the request status field as it progresses
- When a request is fulfilled or confirmed unfillable, update `engagement/sources/coverage_map.md`
  accordingly

## Linking to the Engagement

Each collection request must be referenced in the coverage map so the full team can see which
gaps have pending user requests and which are resolved.
