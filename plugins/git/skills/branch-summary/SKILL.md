---
name: branch-summary
description: Analyze all commits on your branch since diverging from main, then generate a title and description summarizing the complete changeset
---

Provide a branch summary title and body that concisely summarizes the branch changes as they have diverged from master/main. Follow these rules:

- Write in first person as the developer. Never use "we".
- Avoid en dashes, em dashes, and other non-standard characters.
- Do not mention yourself in the message.
- If the branch affects production-facing behavior, include a post-deployment checklist.
- If the changes warrant QA, provide QA notes.
- Provide this in a codeblock that uses markdown.
