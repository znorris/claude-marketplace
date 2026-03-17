# Document Approval Protocol

Writing a design document and approving it are separate steps. A client granting file-write permission is not approval of the document's content.

Every gated artifact follows this sequence:

1. Draft notification: If a team member messages the engagement manager that the document is ready to write. The engagement manager tells the client: "The [agent] has prepared the [document]. I will have them write it to the engagement folder so you can review it."
2. Write: The team member writes the document to the engagement folder. The client may see a file-write permission prompt. Granting this permission means only "yes, save the file." It does not constitute approval of the content.
3. Present for review: The engagement manager reads the document and presents an executive summary to the client. This summary is not a reformatted copy of the document. It is the engagement manager's guided walkthrough that covers:
    - Purpose: what this document decides and how it shapes the rest of the engagement. Frame it in terms of consequences: "This locks in what we are measuring and who we are measuring it for. Everything downstream (sampling, data collection, analysis) builds on these definitions."
    - Overview: a plain-language summary of the key decisions in the document, translated to the client's assessed sophistication level. Use consequence framing for technical choices: "Stratifying by region means we can detect price differences across markets instead of averaging them away."
    - Attention points: flag anything the client should scrutinize. Examples: assumptions that might not match the client's domain knowledge, scope boundaries that exclude something the client might expect to be included, stratification variables where the choice has large downstream impact, tradeoffs where reasonable people might disagree.
    - Review guidance: tell the client what kind of feedback is most useful at this stage. For a Research Specification: "Check whether the population, variables, and scope match your intent. Are there subgroups or factors missing?" For a Sampling Design: "Focus on whether the sample sizes and strategy feel proportionate to the precision you need."
    - Explicit approval request: end with a clear ask. "Please review the document and let me know if you approve it, want to discuss any section, or have changes."
4. Client reviews: Wait for the client to respond. Do not proceed past the gate. Do not interpret silence or a file-write approval as content approval.
5. Resolution: The client either approves the document, requests specific changes, or opens a discussion. If changes are requested, relay them to the specialist, have the specialist revise and rewrite, then re-present from step 3. Repeat until the client explicitly approves.

Record each gate decision (approved, revised, or deferred) and the rationale in the project's `engagement/decision_log.md`.
