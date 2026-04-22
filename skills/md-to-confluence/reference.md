# Markdown to Confluence — reference

## Pandoc examples

HTML fragment (typical for browser copy-paste):

```bash
pandoc notes.md -f gfm -t html -o notes-confluence.html
open notes-confluence.html
```

Self-contained file (local images as `data:` URIs when using `--embed-resources`):

```bash
pandoc report.md -f gfm -t html --embed-resources -o report-confluence.html
```

If the source is not GFM, try:

```bash
pandoc legacy.md -f markdown -t html -o legacy-confluence.html
```

## Word round-trip (often best paste)

1. Run the [md-to-docx](../md-to-docx/SKILL.md) workflow to produce `file.docx`.
2. Open in Microsoft Word, Google Docs, or Apple Pages.
3. Select all → copy → paste into Confluence.

Use this when HTML paste loses list or table structure.

## Confluence Cloud editor tips

- After paste, use the **clear formatting** or normalize styles if the page inherited odd fonts from Word/HTML.
- If the space uses a **template**, paste into the body area after the template placeholders are set.
- Large pages: paste in sections to isolate formatting issues.

## API / automation (optional)

Publishing without paste typically requires:

- Confluence Cloud: REST API, API token, site URL, and space/key.
- Content format: Confluence **storage format** (XHTML), not the editor’s visual markdown.

Community tools (evaluate security and maintenance before production use) may convert Markdown to storage format and call the API. The agent should not invent API payloads or store credentials; use org-approved clients and secret storage.

## Limitations recap

- Mermaid and similar: replace with image exports or Confluence-native diagrams.
- Internal Confluence `wiki` links: will not be generated from Markdown; fix links after creation or use the API with known page IDs.
