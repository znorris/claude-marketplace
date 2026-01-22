---
name: review-tickets
description: Review and update tickets/issues based on current codebase state. Use when the user wants to sync their tickets with reality, asks "what's the status of my tickets?", or wants to clean up their issue backlog.
---

# Review Tickets

Review and update tickets/issues based on the current codebase state. This workflow is platform-agnostic - use the appropriate CLI tool for the user's platform (gh for GitHub, glab for GitLab, acli for Jira, etc.).

## Prerequisites

Detect and use the appropriate CLI tool:

- **GitHub**: `gh` CLI
- **GitLab**: `glab` CLI
- **Jira**: `acli` CLI
- **Linear**: `linear` CLI

If a platform-specific CLI skill is available (e.g., `jira-cli`, `gitlab-cli`), reference it for command syntax.

## Workflow

### Step 1: Gather Open Tickets

Query for open tickets assigned to the current user using the platform's CLI.

**Examples by platform:**

- GitHub: `gh issue list --assignee @me --state open`
- GitLab: `glab issue list --assignee=@me --state=opened`
- Jira: `acli jira workitem search --jql "assignee = currentUser() AND resolution = Unresolved"`

### Step 2: Categorize Tickets

Group tickets by current status:

1. **In Progress** - Actively being worked on
2. **Blocked/On Hold** - Waiting on something
3. **To Do/Backlog** - Ready to start
4. **In Review** - Code review, QA, etc.

### Step 3: Analyze Each Ticket

For each ticket, correlate with codebase state to determine appropriate action.

#### Check Codebase for Evidence

1. **Search for related code:**
   - Look for file paths mentioned in ticket description
   - Search for keywords from ticket summary
   - Check recent git commits for ticket key (e.g., `#123`, `PROJ-456`)

2. **Check git history:**

   ```bash
   git log --oneline --grep="<ticket-id>"
   git log --oneline --all --since="2024-01-01" | grep -i "<keyword>"
   ```

3. **Check recent releases:**

   ```bash
   git tag --sort=-v:refname | head -10
   git log --oneline <previous-tag>..<current-tag>
   ```

### Step 4: Present Recommendation Plan

**IMPORTANT:** Before executing any modifications, present a recommendation plan for user approval.

For each ticket that needs modification:

```
## Recommendation: <TICKET-ID>

### Current State
- **Status:** <current status>
- **Summary:** <ticket summary>

### Evidence Gathered
- Git commits: <list relevant commits>
- Code files: <list relevant files>
- Release inclusion: <which release, if any>
- Deployment status: <deployed to which environment>

### Recommended Action
<What to do: transition, comment, close, or keep as-is>

### Rationale
<Why this action is appropriate based on evidence>

### Command to Execute
<Platform-specific command>
```

Wait for user approval before executing commands.

### Step 5: Execute Approved Actions

Only after user approval, execute the recommended actions.

#### Action Types

**Add Comment** - When you have status updates to share:

- Partial completion updates
- Investigation findings
- Blockers identified
- Work no longer relevant

**Transition Status** - When ticket should move to a different state:

- Work complete -> Ready for Review/QA
- Blocked -> On Hold
- Unblocked -> In Progress
- Shipped -> Done/Closed

**Close** - When work is complete and deployed

**Keep As-Is** - When status accurately reflects reality

### Step 6: Batch Updates

For multiple tickets needing the same action, present as a batch recommendation and execute together after approval.

## Decision Criteria

### When to Close/Complete

- Code is merged to main branch
- Feature is included in a tagged release
- Release has been deployed (if applicable)

### When to Move to Blocked/On Hold

- External dependency not available
- Waiting for clarification from stakeholder
- Blocked by another ticket

### When to Add Comment Only

- Partial progress made
- Investigation complete but implementation not started
- Status update without state change

### When to Keep As-Is

- Actively being worked on
- Status accurately reflects reality
- No new information to add

## Output Format

After review, provide a summary:

```
## Ticket Review Summary

### Updated
- <TICKET-1>: <action taken>
- <TICKET-2>: <action taken>

### Kept As-Is
- <TICKET-3>: <reason>

### Needs Manual Review
- <TICKET-4>: <why it needs discussion>

### Recommended Actions (Not Executed)
- <TICKET-5>: <suggested action and why>
```

## Safety Notes

- Always verify before transitioning tickets
- Prefer manual confirmation over auto-execution
- Add comments explaining changes for audit trail
- Don't close tickets without confirming deployment status
