---
name: quick-style-guide
description: >-
  Run a quick HashiCorp style check on a file or pasted content.
  Uses the machine-optimized quick-reference rules (priority-tagged,
  auto-fix vs manual). Reports violations grouped by severity.
  Pass a file path or paste content directly.
---

# Quick Style Guide Check

Run the HashiCorp style quick-reference against a document and report violations.

## Instructions

1. If the user provided a file path, read it. Otherwise use the pasted content.
2. Use the `docs-search` skill to query `quick-style-guide` for relevant rule
   categories as you work through the document. Run multiple targeted searches —
   one broad pass (e.g. "active voice tense word choice"), then focused searches
   for specific concerns (e.g. "latin phrases", "ableist language", "headings").
3. Query `quick-style-guide` for **"Detection Patterns Summary"** to get the
   regex-based AUTO-FIX patterns. Use these for fast, consistent detection of
   common violations (e.g. "will" in procedural steps, "enables/allows you",
   "via", editorializing words).
4. Query `quick-style-guide` for **"Quick Validation Checklist"** and run a
   final pass against each checklist item to catch anything missed in earlier
   steps.
5. Scan the content against the rules. Focus on **[CRITICAL]** and
   **[IMPORTANT]** priority rules first, then **[STANDARD]**.
6. Report findings in this format:

---

### Critical issues
<!-- [CRITICAL] violations — fix these first -->
- **Line N** — _Rule name_: description of the violation → suggested fix

### Important issues
<!-- [IMPORTANT] violations -->
- **Line N** — _Rule name_: description → suggested fix

### Standard issues
<!-- [STANDARD] violations -->
- **Line N** — _Rule name_: description → suggested fix

### Auto-fixable summary
List every [AUTO-FIX] violation with a one-line fix so the user can batch-apply them.

---

7. If there are no violations in a category, omit that section.
8. End with a short summary: total issues found, how many are auto-fixable.

## Tips

- For [AUTO-FIX] items, show the exact before/after text.
- For [MANUAL] items, explain *why* it violates the rule, not just what to change.
- If the user passes `--fix`, apply all [AUTO-FIX] corrections directly to the
  file (or return the corrected text if content was pasted).
