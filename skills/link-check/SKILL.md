---
name: link-check
description: >-
  Check every link in a Markdown or MDX file. Finds broken links (404 and
  other errors), redirects, links that land on legacy or archived docs,
  missing anchors, and vague link text. Suggests replacements only after
  confirming the new URL is live. Pass a file path. Add --fix to apply the
  suggested fixes after the report.
---

# Link Check

Check every link in a document, classify each one, and suggest fixes that
are confirmed against the live web. Never guess a replacement URL.

## Instructions

### 1. Extract the links

Read the file. Collect every link, with its line number and link text:

- Inline links: `[text](url)` and `[text](url "title")`
- Reference-style links: `[text][ref]` with `[ref]: url` definitions
- In-page anchors: `[text](#anchor)`
- Bare URLs: `<https://example.com>`

Skip links inside fenced code blocks.

### 2. Check external links

For each `http` or `https` URL, run curl. It follows redirects and prints
the final status code, the final URL, and how many redirects it followed:

```bash
curl -s -o /dev/null -L --max-time 15 -A "Mozilla/5.0 (link-check)" \
  -w "%{http_code} %{num_redirects} %{url_effective}\n" "<url>"
```

Remove the `#fragment` from the URL before you run curl, and check the
fragment in step 4. Run the checks in parallel when there are many links.
If a request times out or returns `000`, retry it once before you mark it
broken.

### 3. Check in-page anchors

For each `#anchor` link, build the list of heading slugs in the same file
and confirm the anchor is in it. To build a slug from a heading:
make it lowercase, remove punctuation except hyphens, and replace spaces with
hyphens. Also accept explicit `id="..."` attributes in the file.

### 4. Check external anchors

For each external URL with a `#fragment`, confirm the target page contains
that id:

```bash
curl -sL --max-time 15 "<url-without-fragment>" | grep -o 'id="<fragment>"'
```

If grep finds nothing, list the ids that are on the page so you can suggest
the closest match:

```bash
curl -sL --max-time 15 "<url-without-fragment>" | grep -o 'id="[^"]*"'
```

### 5. Classify each link

Assign one status to every link:

| Status | Meaning |
|--------|---------|
| `OK` | Returns 200 with no redirects, and any anchor exists |
| `REDIRECT` | Returns 200 after a redirect to the same content at a new URL. Update the link to the final URL. |
| `OUTDATED` | Redirects to, or already points at, legacy, archived, or older-version docs. Examples: a `legacy.` or `archive.` subdomain, a version-number subdomain or path such as `18.react.dev`, or a page that says it is no longer maintained. |
| `BROKEN` | Returns 4xx or 5xx, or fails after a retry |
| `ANCHOR MISSING` | The page loads, but the `#anchor` does not exist on it |
| `VAGUE TEXT` | The link text does not describe the target, such as "click here", "here", "this link", "read more", or a bare URL |

A link can have two statuses. For example, a link can be `OK` and `VAGUE TEXT`.

When you mark a link `OUTDATED`, use the WebFetch tool to read the
final page and quote the sentence that shows it is legacy or archived.

### 6. Find replacements

For every link that is not `OK`, suggest a fix. Base each suggestion on a
page you actually loaded:

- `REDIRECT`: suggest the final URL from curl.
- `ANCHOR MISSING`: suggest the closest id from the target page's id list.
- `BROKEN` or `OUTDATED`: find the current page that covers the same topic.
  Good sources are links on the legacy page that point to the new docs, the
  site's navigation, or the site's sitemap. Use the WebFetch tool to read
  the candidate page and confirm it covers what the link text promises.
- `VAGUE TEXT`: suggest link text that names the target page, based on the
  target page's title. Show the full rewritten sentence so it still reads
  correctly. For example, "To learn more, [click here](url)." becomes
  "To learn more, read [Page title](url)."

Before you suggest any new URL, run the curl check from step 2 on it and
confirm it returns 200 with no redirects. If you cannot confirm a
replacement, write `No verified replacement` and explain what a person
should check. Never suggest a URL you did not load.

### 7. Report

Report findings in this format:

---

### Link check: `<file>`

**N links checked: N OK, N need attention**

| Line | Link text | Status | Problem | Suggested fix |
|------|-----------|--------|---------|---------------|
| 10 | Refs and the DOM guide | BROKEN | 404 Not Found | `https://example.com/new-page` (verified 200) |

### Evidence

For each `OUTDATED` link and each `BROKEN` or `OUTDATED` replacement, add one
line with the URL you loaded and a short quote from that page that supports
the fix.

---

Leave `OK` links out of the table. End with: "Reply **apply fixes** to update
the file, or tell me which rows to skip."

### 8. Apply fixes

Apply fixes only when the user asks, or when the user passed `--fix`.
Change only the link URL or link text. Do not rewrite surrounding sentences
unless new link text no longer fits the sentence. Skip any row marked
`No verified replacement`. After you apply the fixes, list each change as
`Line N: old → new`, and tell the user to run `/link-check <file>` again to
confirm the result.
