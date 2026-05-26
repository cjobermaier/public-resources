---
name: full-style-guide
description: >-
  Run a comprehensive HashiCorp style check on a file or pasted content.
  Uses the full style guide with detailed examples and rationale.
  Best for deep reviews, edge cases, or when you need the full rule context.
  Pass a file path or paste content directly.
---

# Full Style Guide Check

Run the complete HashiCorp style guide against a document with full rule context
and examples.

## Instructions

1. If the user provided a file path, read it. Otherwise use the pasted content.
2. Use the `docs-search` skill to query `full-style-guide` for relevant rules
   as you work through the document. Run multiple targeted searches — search by
   category or specific concern (e.g. "active voice examples", "link text
   guidelines", "alert callout usage"). Pull the full rule context and examples
   for each violation you find.
3. For each violation, also query `quick-style-guide` for the matching rule to
   get its priority tag ([CRITICAL], [IMPORTANT], [STANDARD]) and whether it is
   [AUTO-FIX] or [MANUAL]. The full guide has the rationale and examples; the
   quick-reference has the severity and fix type.
4. Scan the content thoroughly across all style categories:
   - Voice and point of view
   - Tense and time references
   - Word choice and simplicity
   - Language (Latin/foreign phrases, ableist language, jargon)
   - Grammar and punctuation
   - Headings and titles
   - Links
   - Alerts and callouts
   - Content organization
   - Enterprise and releases
   - Fonts and formatting
   - UI components
   - Code blocks and commands
   - Numbers, dates, and time
5. Report findings with full context in this format:

---

### Critical issues
- **Line N** — _Rule name_
  - **Found:** exact quote from the document
  - **Rule:** explanation of the rule with rationale
  - **Fix:** corrected version

### Important issues
- (same format)

### Standard issues
- (same format)

### Auto-fixable summary
A consolidated list of every [AUTO-FIX] item with exact before → after replacements
so the user can apply them in one pass.

---

6. Omit sections with no violations.
7. Cite specific examples from the style guide where helpful.
8. End with:
   - Total issues (critical / important / standard)
   - Count of auto-fixable vs manual-review items
   - One sentence on the document's overall style health

## Tips

- When a violation has a nuanced fix, show two or three alternative rewrites.
- If the user passes `--fix`, apply all [AUTO-FIX] corrections directly to the
  file (or return the corrected text if content was pasted).
- For large documents, process section by section and summarize at the end.
