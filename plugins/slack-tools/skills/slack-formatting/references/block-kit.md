# Block Kit Reference

Block Kit is Slack's JSON-based UI framework for rich message layouts. Messages are built from arrays of block objects passed in the `blocks` parameter of `chat.postMessage`.

The Slack MCP tools do not expose a `blocks` parameter directly. This reference is useful for understanding Slack's structured message capabilities, reading Block Kit payloads, and for cases where the Slack API is called directly.

**Limits:** 50 blocks per message, 100 per modal/home tab. `block_id` max 255 characters, must be unique per message.

**Prototyping:** Use Block Kit Builder at https://app.slack.com/block-kit-builder

## Block Types

### section

The primary content block. Displays text with optional two-column fields and one accessory element.

| Field | Type | Required | Constraint |
|---|---|---|---|
| `type` | string | yes | `"section"` |
| `text` | text object | preferred | 1-3000 chars; required if `fields` absent |
| `fields` | text object[] | conditional | max 10 items, each max 2000 chars; renders 2 columns |
| `accessory` | block element | no | one element alongside text |
| `block_id` | string | no | max 255 chars |

```json
{
  "type": "section",
  "text": {
    "type": "mrkdwn",
    "text": "A message *with bold* and _italic_ text."
  }
}
```

Section with fields (two-column key/value layout):

```json
{
  "type": "section",
  "fields": [
    { "type": "mrkdwn", "text": "*Priority*\nHigh" },
    { "type": "mrkdwn", "text": "*Status*\nOpen" }
  ]
}
```

Section with accessory button:

```json
{
  "type": "section",
  "text": {
    "type": "mrkdwn",
    "text": "Deploy *v2.4.1* is ready for review."
  },
  "accessory": {
    "type": "button",
    "text": { "type": "plain_text", "text": "Approve" },
    "style": "primary",
    "value": "approve_deploy",
    "action_id": "approve_btn"
  }
}
```

### header

Large bold plain-text heading. `text` must be `plain_text` type (mrkdwn not supported).

| Field | Type | Required | Constraint |
|---|---|---|---|
| `type` | string | yes | `"header"` |
| `text` | plain_text object | yes | max 150 chars |

```json
{
  "type": "header",
  "text": { "type": "plain_text", "text": "Deployment Status" }
}
```

### divider

A horizontal rule. Only `type` is required.

```json
{ "type": "divider" }
```

### context

Small secondary text and/or images in a compact row. Useful for metadata, timestamps, attribution.

| Field | Type | Required | Constraint |
|---|---|---|---|
| `type` | string | yes | `"context"` |
| `elements` | image elements and/or text objects | yes | max 10 items |

```json
{
  "type": "context",
  "elements": [
    {
      "type": "image",
      "image_url": "https://example.com/avatar.png",
      "alt_text": "user avatar"
    },
    {
      "type": "mrkdwn",
      "text": "Posted by *Jane* | <!date^1609459200^{date_short}|Jan 1, 2021>"
    }
  ]
}
```

### actions

A row of interactive elements (buttons, selects, date pickers, overflow menus).

| Field | Type | Required | Constraint |
|---|---|---|---|
| `type` | string | yes | `"actions"` |
| `elements` | block element[] | yes | max 25 elements |

```json
{
  "type": "actions",
  "elements": [
    {
      "type": "button",
      "text": { "type": "plain_text", "text": "Approve" },
      "style": "primary",
      "value": "approve",
      "action_id": "approve_btn"
    },
    {
      "type": "button",
      "text": { "type": "plain_text", "text": "Reject" },
      "style": "danger",
      "value": "reject",
      "action_id": "reject_btn"
    }
  ]
}
```

### image (block)

Full-width image with optional title. Distinct from the image element used inside sections/contexts.

| Field | Type | Required | Constraint |
|---|---|---|---|
| `type` | string | yes | `"image"` |
| `alt_text` | string | yes | max 2000 chars |
| `image_url` | string | yes* | max 3000 chars; PNG/JPG/GIF |
| `slack_file` | object | yes* | `{ "url": "..." }` or `{ "id": "F..." }` |
| `title` | plain_text object | no | max 2000 chars |

*One of `image_url` or `slack_file` is required.

```json
{
  "type": "image",
  "title": { "type": "plain_text", "text": "Architecture diagram" },
  "image_url": "https://example.com/diagram.png",
  "alt_text": "System architecture"
}
```

### video

Embeds a video player. Requires HTTPS URLs. `video_url` must be in the app's allowed unfurl domains.

| Field | Type | Required |
|---|---|---|
| `type` | string | yes (`"video"`) |
| `title` | plain_text object | yes (max 200 chars) |
| `video_url` | string | yes |
| `thumbnail_url` | string | yes |
| `alt_text` | string | yes |
| `description` | plain_text object | preferred (max 200 chars) |

### input

Collects user input in modals. Not used in regular chat messages.

| Field | Type | Required |
|---|---|---|
| `type` | string | yes (`"input"`) |
| `label` | plain_text object | yes (max 2000 chars) |
| `element` | block element | yes |
| `hint` | plain_text object | no (max 2000 chars) |
| `optional` | boolean | no (default false) |

## Rich Text Blocks

The `rich_text` block type provides structured formatting as an alternative to mrkdwn strings. It supports true lists, nested content, and code with syntax highlighting.

### Structure

```json
{
  "type": "rich_text",
  "elements": [/* rich text sub-elements */]
}
```

### Sub-elements

**rich_text_section** - A paragraph of inline content:

```json
{
  "type": "rich_text_section",
  "elements": [
    { "type": "text", "text": "Hello ", "style": { "bold": true } },
    { "type": "text", "text": "world" }
  ]
}
```

**rich_text_list** - Bulleted or ordered list with nesting support:

```json
{
  "type": "rich_text_list",
  "style": "bullet",
  "elements": [
    {
      "type": "rich_text_section",
      "elements": [{ "type": "text", "text": "Item one" }]
    },
    {
      "type": "rich_text_section",
      "elements": [{ "type": "text", "text": "Item two" }]
    }
  ]
}
```

Fields: `style` (`"bullet"` or `"ordered"`), `indent` (nesting level), `offset` (starting number for ordered), `border`.

**rich_text_preformatted** - Code block with optional syntax highlighting:

```json
{
  "type": "rich_text_preformatted",
  "elements": [{ "type": "text", "text": "const x = 1;" }],
  "language": "javascript"
}
```

**rich_text_quote** - Block quote with left border:

```json
{
  "type": "rich_text_quote",
  "elements": [{ "type": "text", "text": "Quoted text here" }]
}
```

### Inline Element Types

Used within `rich_text_section`:

| Type | Key Fields | Notes |
|---|---|---|
| `text` | `text`, `style: {bold, italic, strike, code, underline}` | Basic text with optional styling |
| `link` | `url`, `text` (optional), `style` | Hyperlink |
| `user` | `user_id` | Renders as @mention |
| `channel` | `channel_id` | Renders as #channel |
| `usergroup` | `usergroup_id` | Renders as @group |
| `emoji` | `name`, `unicode` (optional) | Emoji name without colons |
| `broadcast` | `range`: `"here"`, `"channel"`, `"everyone"` | @here/@channel/@everyone |
| `date` | `timestamp`, `format`, `url`, `fallback` | Localized date |
| `color` | `value` (hex) | Color swatch |

## Interactive Elements

### Button

| Field | Type | Required | Constraint |
|---|---|---|---|
| `type` | string | yes | `"button"` |
| `text` | plain_text object | yes | max 75 chars |
| `action_id` | string | no | max 255 chars |
| `value` | string | no | max 2000 chars |
| `url` | string | no | opens in browser; max 3000 chars |
| `style` | string | no | `"primary"` (green), `"danger"` (red), or omit for default |
| `confirm` | confirm object | no | "Are you sure?" dialog |

### Select Menus

All variants share: `action_id`, `confirm`, `focus_on_load`, `placeholder` (plain_text, max 150 chars).

| Type | `type` value | Key field |
|---|---|---|
| Static list | `static_select` | `options` (max 100) or `option_groups` |
| Dynamic/external | `external_select` | `min_query_length` |
| User list | `users_select` | `initial_user` |
| Conversation list | `conversations_select` | `initial_conversation` |
| Channel list | `channels_select` | `initial_channel` |

Multi-select variants exist for each (`multi_static_select`, etc.) adding `initial_options[]` and `max_selected_items`.

### Overflow Menu

Ellipsis button revealing a small menu. Max 5 options. Supports `url` on options.

### Date Picker

`type: "datepicker"`. `initial_date` in `YYYY-MM-DD` format.

### Checkboxes

`type: "checkboxes"`. Max 10 options. Options support mrkdwn descriptions.

### Radio Button Group

`type: "radio_buttons"`. Max 10 options. One selected at a time.

### Plain-Text Input

`type: "plain_text_input"`. Fields: `multiline` (boolean), `min_length`, `max_length` (0-3000), `initial_value`, `placeholder`.

## Composition Objects

### Text Object

Used everywhere text appears in Block Kit.

| Field | Type | Required | Notes |
|---|---|---|---|
| `type` | string | yes | `"plain_text"` or `"mrkdwn"` |
| `text` | string | yes | 1-3000 chars |
| `emoji` | boolean | no | `plain_text` only; converts `:emoji:` names |
| `verbatim` | boolean | no | `mrkdwn` only; disables auto-linkification |

### Option Object

Used in selects, checkboxes, radio buttons, overflow menus.

| Field | Type | Required | Constraint |
|---|---|---|---|
| `text` | text object | yes | max 75 chars |
| `value` | string | yes | max 150 chars |
| `description` | plain_text object | no | max 75 chars |
| `url` | string | no | overflow menus only; max 3000 chars |

### Confirmation Dialog Object

| Field | Type | Required | Constraint |
|---|---|---|---|
| `title` | plain_text object | yes | max 100 chars |
| `text` | plain_text object | yes | max 300 chars |
| `confirm` | plain_text object | yes | button label; max 30 chars |
| `deny` | plain_text object | yes | button label; max 30 chars |
| `style` | string | no | `"primary"` or `"danger"` |

## Legacy Attachments

Deprecated in favor of Block Kit but still supported. Appear as secondary content below the main message.

**Maximum:** 20 attachments per message.

### Color Stripe

The `color` field controls the left-edge stripe:
- `"good"` - green
- `"warning"` - yellow
- `"danger"` - red
- `"#439FE0"` - any hex color

### Fields

| Field | Description |
|---|---|
| `fallback` | Plain text summary for notifications |
| `color` | Left border color |
| `pretext` | Text above the attachment block |
| `author_name` | Small attribution text |
| `author_link` | Hyperlinks author_name |
| `author_icon` | 16x16px image beside author |
| `title` | Large bold heading |
| `title_link` | Makes title a hyperlink |
| `text` | Main body (collapses after 700+ chars or 5+ line breaks) |
| `fields` | Array of `{title, value, short}` objects |
| `image_url` | Full-width image at bottom (max 360x500px) |
| `thumb_url` | Small thumbnail on right (max 75px) |
| `footer` | Small contextual text (max 300 chars) |
| `footer_icon` | 16x16px image beside footer |
| `ts` | Unix timestamp in footer |
| `mrkdwn_in` | Array of field names to process as mrkdwn |

Example:

```json
{
  "attachments": [
    {
      "color": "#36a64f",
      "fallback": "Deploy #142 passed",
      "title": "Deploy #142 passed",
      "title_link": "https://ci.example.com/builds/142",
      "text": "All tests passed. Deployed to production.",
      "fields": [
        { "title": "Environment", "value": "production", "short": true },
        { "title": "Duration", "value": "1m 23s", "short": true }
      ],
      "footer": "CI System",
      "ts": 1609459200,
      "mrkdwn_in": ["text"]
    }
  ]
}
```

## chat.postMessage Key Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `channel` | string | required | Channel ID, user ID, or DM conversation ID |
| `text` | string | -- | Message body or fallback for block messages (recommended max 4000 chars) |
| `blocks` | JSON array | -- | Block Kit blocks; replaces `text` as visual content |
| `attachments` | JSON array | -- | Legacy attachments |
| `mrkdwn` | boolean | `true` | Enable mrkdwn parsing in `text` |
| `thread_ts` | string | -- | Parent message timestamp for threaded reply |
| `reply_broadcast` | boolean | `false` | Show thread reply in channel too |
| `unfurl_links` | boolean | `true` | Auto-expand link previews |
| `unfurl_media` | boolean | `true` | Auto-expand media previews |
| `username` | string | -- | Override bot display name (needs `chat:write.customize` scope) |
| `icon_emoji` | string | -- | Override bot icon with emoji (e.g., `:robot_face:`) |

When `blocks` is present, `text` serves only as the notification/push text and screen reader fallback. Always set a meaningful `text` value when using blocks.

## Best Practices

**Message structure:**
1. `header` block for the subject/title
2. `section` blocks for body content with mrkdwn text
3. `section` with `fields` for key/value metadata
4. `divider` blocks to separate logical sections
5. `context` blocks for secondary info (timestamps, attribution)
6. `actions` block last if user action is needed

**Block Kit tips:**
- Always set `block_id` on interactive blocks; regenerate on every message update
- Keep `action_id` values stable across updates
- Use `confirm` dialogs on destructive `danger`-style buttons
- Prefer `rich_text` blocks over mrkdwn sections for true lists, nested content, or syntax-highlighted code
- `context` blocks render at smaller font size; use for metadata, not primary content
- The `text` field on `chat.postMessage` is the notification string; write it as a one-line summary
