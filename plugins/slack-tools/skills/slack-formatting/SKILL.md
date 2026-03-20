---
name: slack-formatting
description: Reference for Slack message formatting. Use when sending Slack messages, creating Slack drafts, scheduling Slack messages, composing Slack canvases, or when the user asks about Slack formatting, mrkdwn syntax, or Block Kit.
---

# Slack Message Formatting Reference

## MCP Tool Overview

The Slack MCP tools accept a `message` string parameter. The MCP layer translates standard markdown into Slack's native mrkdwn format before posting. Write messages using the conventions below.

| Tool | Purpose | Key Parameters |
|---|---|---|
| `slack_send_message` | Send immediately | `channel_id`, `message`, `thread_ts`, `reply_broadcast` |
| `slack_send_message_draft` | Save as draft | `channel_id`, `message`, `thread_ts` |
| `slack_schedule_message` | Send later | `channel_id`, `message`, `post_at` (Unix timestamp) |
| `slack_create_canvas` | Create document | `title`, `content` (markdown) |

Limits: 5000 characters per text element. Cannot post to externally shared (Slack Connect) channels.

## Message Formatting

The MCP tools accept standard markdown syntax:

| Effect | Syntax | Renders as |
|---|---|---|
| Bold | `**text**` | **text** |
| Italic | `_text_` | _text_ |
| Strikethrough | `~text~` | ~~text~~ |
| Inline code | `` `text` `` | `text` |
| Code block | ` ```code``` ` | fenced code block |
| Blockquote | `> text` | indented quote |
| Link | `[label](url)` | clickable link |

### Lists

Bullet lists and numbered lists work with standard markdown:

```
- Item one
- Item two
- Item three
```

```
1. First
2. Second
3. Third
```

### Code Blocks

Triple-backtick fenced blocks. Language hints are not rendered by Slack but are accepted:

````
```python
def hello():
    print("Hello")
```
````

### Blockquotes

Prefix lines with `>`:

```
> This is a quoted line
> This continues the quote
```

### Section Spacing

Slack collapses blank lines in messages. To create visible blank lines between sections, insert a line containing only a zero-width space (Unicode U+200B: `\u200B`). Regular empty lines and lines with only normal spaces are stripped.

```
**Section One**
Content here
\u200B
**Section Two**
More content
```

This produces a visible gap between sections that Slack does not collapse.

## Special Slack Elements

These Slack-native constructs work within MCP message strings.

### Mentions

| Target | Syntax | Notes |
|---|---|---|
| User by ID | `<@U012AB3CD>` | Always use IDs, never display names |
| Channel by ID | `<#C123ABC456>` | Links to the channel |
| @here | `<!here>` | Notifies active channel members |
| @channel | `<!channel>` | Notifies all channel members |
| @everyone | `<!everyone>` | Notifies entire workspace |

Use `slack_search_users` to find user IDs and `slack_search_channels` to find channel IDs before mentioning.

### Emoji

Use Slack colon syntax: `:tada:`, `:white_check_mark:`, `:warning:`, `:rocket:`, `:eyes:`.

Useful emoji for status messages:

| Emoji | Code | Use for |
|---|---|---|
| :white_check_mark: | `:white_check_mark:` | Success, completed |
| :x: | `:x:` | Failed, error |
| :warning: | `:warning:` | Warning, caution |
| :rocket: | `:rocket:` | Deploy, launch |
| :eyes: | `:eyes:` | Needs review |
| :hourglass: | `:hourglass:` | In progress, waiting |
| :bulb: | `:bulb:` | Idea, tip |
| :rotating_light: | `:rotating_light:` | Alert, urgent |
| :memo: | `:memo:` | Notes, documentation |
| :link: | `:link:` | Reference, URL |
| :tada: | `:tada:` | Celebration |
| :fire: | `:fire:` | Hot, critical |

### Date Formatting

Renders timestamps in the viewer's local timezone:

```
<!date^UNIX_TIMESTAMP^TOKEN_STRING|fallback_text>
```

Tokens:

| Token | Example Output |
|---|---|
| `{date_num}` | 2024-02-18 |
| `{date}` | February 18th, 2024 |
| `{date_short}` | Feb 18, 2024 |
| `{date_long}` | Tuesday, February 18th, 2024 |
| `{date_pretty}` | today / yesterday / February 18th, 2024 |
| `{time}` | 6:39 AM |
| `{time_secs}` | 6:39:45 AM |
| `{ago}` | 3 minutes ago |

Tokens combine: `{date_short} at {time}` renders as "Feb 18, 2024 at 6:39 AM".

Example: `<!date^1392734382^{date_short} at {time}|Feb 18, 2014 at 6:39 AM>`

Add a link: `<!date^1392734382^{date_short}^https://example.com|Feb 18, 2014>`

## Message Composition Patterns

Use `\u200B` lines between sections for visual spacing (see Section Spacing above).

### Status Update

```
:rocket: **Deploy v2.4.1 to production**
\u200B
_Environment:_ production
_Branch:_ `release/2.4.1`
_Status:_ :white_check_mark: Complete
\u200B
> All health checks passing. No rollback needed.
```

### Notification with Action Items

```
:warning: **Database migration required**
\u200B
The `users` table schema changed in PR #482. Before deploying:
\u200B
1. Run `./scripts/migrate.sh` on staging
2. Verify row counts match
3. Deploy to production
\u200B
_Ping <@U012AB3CD> when ready._
```

### Summary Report

```
:memo: **Weekly Sprint Summary**
\u200B
_Completed:_ 12 tickets
_In Progress:_ 4 tickets
_Blocked:_ 1 ticket
\u200B
**Highlights:**
- Shipped user profile redesign
- Fixed payment processing timeout
- Onboarded new API partner
\u200B
**Blockers:**
- :rotating_light: Waiting on vendor SSL cert renewal (ETA: Thursday)
```

### Incident Alert

```
:rotating_light: **P1 Incident: API Latency Spike**
\u200B
_Detected:_ <!date^1710900000^{date_short} at {time}|Mar 20, 2024 at 12:00 AM>
_Service:_ api-gateway
_Impact:_ 95th percentile latency > 2s (normal: 200ms)
\u200B
**Current status:** Investigating. Initial triage points to database connection pool exhaustion.
\u200B
_Incident channel:_ <#C123ABC456>
```

### Code Review Request

```
:eyes: **Review requested: Refactor auth middleware**
\u200B
_MR:_ [!482 - Refactor auth middleware](https://gitlab.example.com/project/-/merge_requests/482)
_Author:_ <@U012AB3CD>
_Changes:_ 4 files, +120 / -85
\u200B
Key changes:
- Extract token validation into standalone module
- Add refresh token rotation
- Remove deprecated session cookie path
```

## Canvas Formatting

The `slack_create_canvas` tool uses standard markdown with specific constraints:

- Headers must not exceed depth 3 (`###` max). Deeper headers are truncated to `###`.
- Links must be full HTTP URLs. No relative links.
- Tables: escape `|` inside cell content with `\|`.
- Write in full paragraphs, not bullet lists (unless explicitly requested).
- Do not duplicate the canvas title in the content body.

## Thread Replies

To reply in a thread, set `thread_ts` to the parent message's timestamp string (e.g., `"1234567890.123456"`).

Set `reply_broadcast: true` to also show the reply in the main channel.

## Slack's Native mrkdwn Format

The MCP tools handle translation, but when reading Slack content or debugging formatting, note that Slack's native mrkdwn differs from standard markdown:

| Feature | Standard Markdown | Slack mrkdwn |
|---|---|---|
| Bold | `**text**` | `*text*` |
| Italic | `_text_` or `*text*` | `_text_` |
| Links | `[label](url)` | `<url\|label>` |
| Images | `![alt](url)` | Not supported inline |

Slack mrkdwn does not support: nested formatting, tables, headings, horizontal rules, or images inline.

Characters that must be HTML-escaped in raw mrkdwn: `&` to `&amp;`, `<` to `&lt;`, `>` to `&gt;`.

## Limitations

The Slack MCP tools only accept a `message` string. They do not expose a `blocks` parameter, so Block Kit JSON cannot be used. All formatting must be done with the markdown syntax and special elements documented above.

## Additional Resources

- **`references/block-kit.md`** - Full Block Kit reference covering block types, interactive elements, composition objects, rich text blocks, and legacy attachments. This is for direct Slack API usage only; Block Kit is not available through the MCP tools.
