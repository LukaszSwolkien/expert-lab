---
name: confluence-doc-to-md
description: Converts Confluence-exported .doc files (MIME/HTML format) to Markdown while preserving headings, tables, lists, links, and embedded images. Use when the user provides a .doc file exported from Confluence, or when a .doc file contains MIME multipart HTML content rather than binary Word format.
---

# Confluence DOC to Markdown

## When to use

Use this skill when the task involves converting `.doc` files exported from Confluence into Markdown. These files are **not** binary Word documents — they are MIME multipart messages wrapping HTML content with base64-encoded image attachments.

### How to detect this format

Run `file INPUT.doc`. If the output contains `news or mail text, ASCII text` (not `Microsoft Word`), the file is a Confluence MIME export and this skill applies. If the output says `Microsoft Word` or `Composite Document File`, use a different conversion approach.

## Quick workflow

1. Confirm input/output paths:
   - Input: `FILE.doc`
   - Output: `FILE.md` placed per the file output rules (if source is in a `docs/` subfolder, output goes one level up)
   - Assets: `assets/<FILENAME>/` relative to the output `.md` file
2. Parse the MIME multipart message using Python `email` module.
3. Extract the `text/html` part and decode it (typically `quoted-printable`).
4. Convert HTML to Markdown with `pandoc` using `--to markdown` (not `gfm`) to force conversion of Confluence HTML tables into Markdown table syntax (typically grid-table format).
5. Extract embedded binary parts (base64) — map each `Content-Location` hash to its HTML `<img src="...">` reference.
6. Classify images by matching `src` hashes to `alt` text in the HTML:
   - Skip Confluence decorator icons (e.g., `alt="(blue star)"`, `alt="(warning)"`, `alt="(info)"`).
   - Identify meaningful images by their `alt` text or context.
7. Save meaningful images to `assets/<FILENAME>/` with descriptive filenames derived from `alt` text.
8. Classify each saved image:
   - **Diagram** (flowcharts, architecture diagrams, sequence diagrams, state machines, etc.): convert to a Mermaid code block. Add `<!-- source: assets/FILENAME/imageN.png -->` above the block. Do **not** add an image link.
   - **Non-diagram** (photos, screenshots, charts that cannot be represented in Mermaid): embed as `![alt](assets/FILENAME/imageN.png)`.
9. Post-process generated Markdown:
   - Remove pandoc attribute suffixes like `{.external-link ...}` and heading IDs `{#...}` unless explicitly needed.
   - Normalize escaped punctuation (`\"`, `\#`, `\@`) where it harms readability.
   - Remove placeholder line-break markers in table cells (`\`) when they do not carry meaning.
   - Replace escaped arrows (`--\>`) with `-->` when they represent flow steps.
   - Normalize non-breaking spaces and collapse excessive blank lines.
10. Convert HTML to Markdown:
   - `<h1>` → `#`, `<h2>` → `##`, etc.
   - `<table>` → Markdown tables with header separator row
   - `<ul>/<ol>/<li>` → Markdown lists
   - `<a href>` → `[text](url)`
   - `<p>` → paragraph breaks
   - `<code>/<pre>` → fenced code blocks
   - Strip `<style>`, `<script>`, and Confluence-specific markup
11. Handle Confluence-specific elements:
   - Expand macro containers (info/warning/note panels) into blockquotes
   - Convert status lozenges to inline text
   - Preserve Confluence links (even if pointing to internal wiki)
   - Remove Confluence CSS classes and Office XML namespaces
12. Validate:
    - No raw HTML tags in final output (except intentional HTML comments for Mermaid source references)
    - Every meaningful embedded image is accounted for (either as Mermaid or as image link)
    - Tables render correctly (column counts match across rows)
    - Heading levels are monotonic
    - Readable spacing between sections
    - Output file saved in the expected location

## Readability normalization (important)

Confluence tables often convert into very noisy multi-row or grid-border markdown. Normalize them for human readability:

1. Convert grid-border table artifacts (`+-----+`) to standard pipe-table form where possible.
2. Keep one primary row per entity/account/project.
3. If a row has long procedural or policy text spread across continuation rows:
   - keep concise summary in the table cell, and
   - move detailed steps into a short subsection directly below the table (bullet list).
4. Preserve all links, IDs, group names, and warning statements exactly.
5. Prefer explicit column names (`Notes`, `Access`, etc.) over empty headers.

This makes markdown readable without losing source data.

## Source-vs-output verification protocol

After conversion (and after any readability refactor), run a strict completeness check:

1. **Headings check**
   - Compare source `<h2>` set vs markdown `##` set.
   - Normalize HTML entities (`&nbsp;`) before comparing.
2. **Link coverage check**
   - Extract all source `href` values and all markdown links.
   - Confirm no source links are missing.
3. **Critical token check**
   - Confirm presence of high-signal tokens from source:
     - AWS account IDs (`\\b\\d{12}\\b`)
     - UUIDs
     - Okta groups (`sg-*`)
     - ticket keys (e.g., `CAS-8847`)
4. **Semantic spot-check**
   - Compare source and markdown plain-text renderings.
   - Treat table-layout diffs as expected noise; manually verify any potentially missing policy/warning sentences.
5. **Final assertion**
   - If anything is missing, restore from source before finalizing.

Always report verification outcome explicitly (what was checked + whether any gaps were fixed).

## Preferred automation command

For repeatable conversions, use a small Python converter that:
- reads MIME `.doc` via `email.parser.BytesParser`
- extracts `text/html`
- runs `pandoc --from html-native_divs-native_spans --to markdown --wrap=none`
- applies post-processing cleanup listed above
- runs source-vs-output verification (headings, links, critical tokens)

This avoids the common failure mode where `gfm` target leaves raw Confluence `<table>` and `<div>` HTML in output.

## Constraints

- Preserve all hyperlinks found in `<a href>` tags.
- Prefer deterministic transformation over stylistic rewriting.
- Keep content faithful to source document wording.
- Keep original image formats when extracting assets.
- Diagrams must be converted to Mermaid and must not appear as image links in the final output.
- Each Mermaid block must include an HTML comment identifying the source asset file.
- Confluence decorator icons (blue star, warning, info, etc.) should be omitted from the output entirely.

## Additional resources

- Detailed extraction snippets and command examples: [reference.md](reference.md)
