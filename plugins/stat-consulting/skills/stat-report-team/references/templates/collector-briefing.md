# Collector Briefing Packet Template

<!-- Source analyst: fill in all [placeholder] fields before sending to the human collector. -->
<!-- Remove HTML comments from the final document. -->

---

## Header

- **Engagement ID:** [engagement-id]
- **Cell / Stratum:** [cell-name-or-stratum-label]
- **Date:** [YYYY-MM-DD]
- **Priority:** [High / Normal / Low]

---

## Stores to Visit

<!-- List each store URL as a numbered item. Include any login notes if the store requires an account. -->

1. [store-url-1]
2. [store-url-2]
3. [store-url-3]

---

## Products to Find

<!-- Define each product category precisely. Ambiguous definitions cause collection errors. -->
<!-- Include photos or exact product names wherever possible. -->

### Category: [category-name]

**Definition:** [one-sentence definition of what qualifies]

**Positive examples (include these):**
- [product-name or description]
- [product-name or description]

**Negative examples (exclude these):**
- [product-name or description -- explain why it does not qualify]
- [product-name or description -- explain why it does not qualify]

<!-- Repeat the Category block for each additional product category. -->

---

## Data to Record Per Product

<!-- This table must match the variable schema exactly. Do not rename columns. -->

| Field | Description | Format / Units | Required |
|-------|-------------|----------------|----------|
| [variable-name] | [plain-language description] | [e.g., decimal, USD, text] | [Yes / No] |
| [variable-name] | [plain-language description] | [e.g., decimal, USD, text] | [Yes / No] |
| [variable-name] | [plain-language description] | [e.g., decimal, USD, text] | [Yes / No] |

<!-- Add rows as needed to cover all target variables from engagement/config.md. -->

---

## How to Format Results

Submit results as a CSV file or spreadsheet with the following exact column headers (order matters):

```
[column-1],[column-2],[column-3],[column-4]
```

<!-- Paste the exact header row above, matching the variable schema. -->

One row per product observation. Leave a cell blank if the value is genuinely unavailable (do not enter 0 or N/A for missing prices).

---

## Edge Cases and Troubleshooting

**Store is down or unavailable:**
Skip the store and note it in the comments column with the date and time attempted. Do not substitute a different store.

**Price is not listed:**
Leave the price field blank. Do not estimate or use a price from another store.

**Unsure whether a product qualifies:**
Record the product with a note in the comments column describing the uncertainty. Do not discard it. The analyst will adjudicate borderline cases.

**Login required:**
<!-- Source analyst: note here whether credentials are provided or the store should be skipped. -->
[Describe login instructions or state "skip if login is required."]

**Product match is ambiguous (size, variant, pack count):**
Record all variants you find and note the exact listing title in the comments column. Do not pick one arbitrarily.

---

## Return Instructions

- **Submit to:** [submission-url, email address, or shared folder path]
- **File naming convention:** `[engagement-id]_[cell-name]_[YYYY-MM-DD].[csv or xlsx]`
- **Expected turnaround:** [e.g., within 48 hours of receiving this briefing]
- **Questions:** Contact [analyst-name] at [contact-info]
