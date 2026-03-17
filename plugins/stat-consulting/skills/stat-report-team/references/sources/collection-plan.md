# Collection Execution Plan Template

The source analyst writes the collection execution plan to `engagement/sources/collection_plan.md`. This plan documents the exact traversal strategy so the engagement manager and client can validate the approach before collection begins.

The plan must follow this numbered markdown list format:

```markdown
## Collection Execution Plan

1. Deduplicate school list: [N] unique schools (collapsed from [frame description])
2. For each school [[N] iterations]:
   1. Write task file for worker: search for school's online store URL
   2. Request worker from manager
   3. For each store found [1-3 per school]:
      1. Write task file for worker: fetch product listing page
      2. Request worker from manager
      3. Receive results, extract prices and categories per variables manifest
3. After every [N] schools: write batch file to `engagement/data/batches/`

**Deduplication**: [keying strategy, e.g., schools keyed by NCES ID; stores keyed by URL, same URL across schools recorded once and flagged]
**Expected yield**: [estimated observation range across estimated store count]
**Worker requests**: [estimated count]
```

## Requirements

- Every step numbered, with loops annotated `[N iterations]` or `[loop]`
- Worker requests called out inline
- Traversal unit (outer loop) stated explicitly
- Whether the same URL could be visited more than once made visible
- Deduplication rules and expected yield in a summary block
