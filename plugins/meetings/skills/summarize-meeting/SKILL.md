---
name: summarize-meeting
description: Transform a raw meeting transcript into a structured summary with citations and iterative refinement. Use when the user asks to "summarize a meeting", "summarize this transcript", "write meeting notes", or has a meeting recording or transcript to process.
---

Transform a raw meeting transcript into a structured, citable summary document. Walk the developer through an iterative refinement loop to catch transcription errors and validate ambiguous content.

## Phase 1 -- Gather Input

1. Accept the transcript from the developer. It may be pasted directly, provided as a file path, or retrieved via a Slack MCP tool.
2. Ask where to write the output file. Default: `meeting-summary-YYYY-MM-DD.md` in the current working directory. Accept any path the developer provides.
3. Ask for any additional context: attendee names, project names, acronyms, or jargon that might help interpret the transcript accurately. If the developer has nothing to add, proceed.

## Phase 2 -- Structured Summary

Produce the first draft and write it to the output file. Use this structure:

```
# Meeting Summary -- [Date] -- [Topic]

## Attendees
- List of participants identified in the transcript (optional -- omit if not identifiable)

## Executive Summary
2-4 sentences capturing the most important outcomes and decisions.

## Learnings: How Things Are
What was discussed about how things work today. Include enough detail that someone unfamiliar could understand the starting point. Each point should have an inline citation.

## Discussion Topics

### [Category 1]
- Key point (Speaker, MM:SS)
- Decision or conclusion reached, called out explicitly (Speaker, MM:SS)

### [Category 2]
- ...

Group related discussion points into named categories. Call out decisions inline where they occur rather than in a separate section. Decisions should be clearly distinguishable from general discussion.

## Action Items

| Owner | Action |
|-------|--------|
| **Name** | Description of action item (Speaker, MM:SS) |

## Future State
Description of what the agreed-upon end state looks like, based on the decisions and action items above. This should read like a mini design document: concrete enough that someone could use it as a reference for implementation.
```

### Citation Rules

- Inline citations use the format `(Speaker, MM:SS)` referencing the transcript timestamp.
- Every factual claim, decision, and action item must have at least one citation.
- If the transcript lacks timestamps, use `(Speaker)` only.
- If a point spans a range, use `(Speaker, MM:SS--MM:SS)`.

### Writing Guidelines

- Avoid en dashes and em dashes.
- Be specific. Prefer concrete details over vague summaries.
- Preserve the original speakers' intent. Do not editorialize or reframe.

## Phase 3 -- Transcription Error Review

After writing the first draft, present a self-report to the developer. Do not wait to be asked.

1. List every word or phrase that appeared to be a transcription error and what it was corrected to.
2. List every name, acronym, or proper noun that might have been transcribed incorrectly.
3. For each correction, state the confidence level: certain, likely, or uncertain.

Format this as a numbered list in conversation, not in the file. Example:

```
1. "stem firm" -> "semver" (certain)
2. "Chrome" -> "Scrum" (likely)
3. "Frank said" -> "for instance" (certain)
4. "Maine" -> "main" (certain)
5. Speaker name "Cody" -- could not verify spelling (uncertain)
```

Ask the developer to confirm or correct each item. Apply confirmed corrections to the file.

## Phase 4 -- Developer Review

After transcription corrections are applied, ask the developer to review the summary for:

- Framing that does not match what was actually meant
- Missing context or nuance
- Misattributed statements
- Anything that feels off

Prompt: "I've applied the transcription corrections. Please review the summary and let me know if anything needs adjusting -- framing, missing context, misattributions, or anything that doesn't match what was discussed."

Apply any corrections the developer provides directly to the file.

## Phase 5 -- Uncertainty Surface

After the developer's corrections are applied, surface remaining uncertainties. Do not wait to be asked.

1. Names or product references that could not be verified
2. Statements where the transcript was too unclear to interpret confidently
3. Action items where the owner or scope is ambiguous
4. Anything omitted from the summary because the transcript was unintelligible

Format as a numbered list in conversation. Ask the developer to validate or clarify each item. Apply final corrections to the file.

If there are no remaining uncertainties, say so and move to Phase 6.

## Phase 6 -- Finalize

Confirm with the developer that the summary is complete. Report the final file path.

## Constraints

- Do not skip phases. Walk through each phase sequentially, waiting for developer input at each gate before proceeding.
- Do not fabricate content that is not supported by the transcript.
- Do not commit, push, or deploy the file.
- Do not modify any files other than the output summary.
- If the transcript is too short or too garbled to produce a useful summary, say so and ask for guidance rather than guessing.
