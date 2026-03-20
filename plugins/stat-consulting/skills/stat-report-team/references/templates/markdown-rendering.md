# Markdown Rendering Pitfalls

Reference for the report composer. Apply these fixes before finalizing any markdown output that will be rendered in a viewer with LaTeX or HTML interpretation enabled.

---

## Dollar Signs

**Problem:** A bare `$` triggers LaTeX math mode in many markdown renderers. Two dollar signs open inline math; a single `$` at the start of a span can still cause rendering artifacts.

**Fix:** Escape as `\$`.

**Example:**
- Before: `The median price was $4.99`
- After: `The median price was \$4.99`

For bulk replacement, use the Edit tool with `replace_all: true` rather than a sed command.

---

## Pipe Characters in Tables

**Problem:** A literal `|` inside a table cell is interpreted as a column delimiter and breaks the table layout.

**Fix:** Escape as `\|` inside cell content.

**Example:**
- Before: `| Stores | Walmart | Target | Walmart|Target |`
- After: `| Stores | Walmart | Target | Walmart\|Target |`

---

## Angle Brackets

**Problem:** `<` and `>` are interpreted as HTML tags in most markdown renderers. Unknown tags may be silently stripped or cause parse errors.

**Fix:** Escape as `&lt;` and `&gt;`, or wrap in a code span.

**Example:**
- Before: `Values in range <1.0 are excluded`
- After: `Values in range &lt;1.0 are excluded`
- Alternative: `` Values in range `<1.0` are excluded ``

---

## Underscores in Identifiers

**Problem:** Underscores inside words trigger italic emphasis parsing in some renderers (`my_variable_name` renders as `my<em>variable</em>name`).

**Fix:** Escape as `\_` or wrap the identifier in a code span.

**Example:**
- Before: `The column product_unit_price contains the raw value`
- After: `The column `product_unit_price` contains the raw value`
- Alternative: `The column product\_unit\_price contains the raw value`

Prefer code spans for variable names, column headers, and file paths -- they communicate intent and sidestep the escaping issue entirely.

---

## Bulk Character Substitution

Use the Edit tool with `replace_all: true` to substitute characters across a file. Do not use `sed` for this -- sed commands require shell escaping on top of regex escaping, which introduces its own errors.

**Pattern:**
1. Read the file.
2. Call Edit with `old_string: "$"`, `new_string: "\\$"`, `replace_all: true`.
3. Verify the result by reading the affected lines.

Apply substitutions in a deliberate order: escape dollar signs before processing table pipes, so a `$|` sequence is not double-processed.
