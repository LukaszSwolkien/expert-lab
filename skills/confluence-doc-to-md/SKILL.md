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
4. Extract embedded binary parts (base64) — map each `Content-Location` hash to its HTML `<img src="...">` reference.
5. Classify images by matching `src` hashes to `alt` text in the HTML:
   - Skip Confluence decorator icons (e.g., `alt="(blue star)"`, `alt="(warning)"`, `alt="(info)"`).
   - Identify meaningful images by their `alt` text or context.
6. Save meaningful images to `assets/<FILENAME>/` with descriptive filenames derived from `alt` text.
7. Classify each saved image:
   - **Diagram** (flowcharts, architecture diagrams, sequence diagrams, state machines, etc.): convert to a Mermaid code block. Add `<!-- source: assets/FILENAME/imageN.png -->` above the block. Do **not** add an image link.
   - **Non-diagram** (photos, screenshots, charts that cannot be represented in Mermaid): embed as `![alt](assets/FILENAME/imageN.png)`.
8. Convert HTML to Markdown:
   - `<h1>` → `#`, `<h2>` → `##`, etc.
   - `<table>` → Markdown tables with header separator row
   - `<ul>/<ol>/<li>` → Markdown lists
   - `<a href>` → `[text](url)`
   - `<p>` → paragraph breaks
   - `<code>/<pre>` → fenced code blocks
   - Strip `<style>`, `<script>`, and Confluence-specific markup
9. Handle Confluence-specific elements:
   - Expand macro containers (info/warning/note panels) into blockquotes
   - Convert status lozenges to inline text
   - Preserve Confluence links (even if pointing to internal wiki)
   - Remove Confluence CSS classes and Office XML namespaces
10. Validate:
    - No raw HTML tags in final output (except intentional HTML comments for Mermaid source references)
    - Every meaningful embedded image is accounted for (either as Mermaid or as image link)
    - Tables render correctly (column counts match across rows)
    - Heading levels are monotonic
    - Readable spacing between sections
    - Output file saved in the expected location

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
