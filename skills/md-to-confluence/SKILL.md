---
name: md-to-confluence
description: Converts Markdown to Confluence-friendly HTML (or Word) for copy-paste or import into a new Confluence page. Use when the user wants to publish Markdown to Confluence, paste content from .md into the editor, or avoid manual reformatting when creating a wiki page.
---

# Markdown to Confluence (copy-paste / import)

## When to use

Use this skill when the user has a `.md` file and wants the content in **Confluence** with minimal manual restructuring—especially **copy-paste into the Confluence editor** or a small **import** step.

## Better options to mention first (pick by situation)

1. **Markdown → Word → Confluence** (often the best paste fidelity): use the project skill [md-to-docx](../md-to-docx/SKILL.md), open the `.docx` in Word or Pages, copy all, paste into Confluence. The editor usually preserves headings, lists, links, and tables well.
2. **Programmatic publish** (repeatable, team spaces): use Confluence REST API and a converter to **Confluence storage format** (e.g. community tools such as `md2cf` in Python—evaluate for your environment; requires credentials and API permissions). No copy-paste, better for CI/CD.
3. **This skill’s default** (no Word, still paste-friendly): **Markdown → HTML** with `pandoc`, open the HTML in a browser, **copy from the browser** and paste into Confluence. Rich HTML paste generally works better than pasting raw Markdown.

If the user only needs a different file format and already uses Word, prefer **md-to-docx** instead of duplicating that workflow here.

## Prerequisites

- `pandoc` available (`brew install pandoc` on macOS)
- For HTML route: a browser to copy rendered content from

## Quick workflow (HTML for copy-paste)

1. Confirm paths (follow project file-output rules: if the source lives under `docs/`, place generated files one level up as required by the repo).
2. From GFM-style Markdown, generate an **HTML fragment** (no full `<html>` wrapper required, but either works):

   ```bash
   pandoc INPUT.md -f gfm -t html --embed-resources -o INPUT-confluence.html
   ```

   Omit `--embed-resources` if you do not need self-contained output (simpler for a quick paste).

3. Open `INPUT-confluence.html` in a browser, select all (in the page body), copy, paste into the Confluence page editor.
4. In Confluence, fix anything the editor mangles (rare: nested lists, some table edge cases). Adjust heading levels if the first `#` should not become a page title duplicate.

## Code blocks and tables

- Fenced code blocks become `<pre><code>`; Confluence usually keeps them as code-like blocks after paste. If not, use Confluence’s code macro for critical snippets.
- GitHub-flavored tables: use `-f gfm` so tables convert predictably.

## What does not translate automatically

- **Mermaid** (or other diagram languages): not rendered by Confluence from a paste. Options: export diagram as image and attach, or use a Confluence diagram app, or describe in plain text.
- **Raw HTML in Markdown**: passed through; Confluence may strip or alter some tags after paste—verify in the editor.
- **Confluence-only macros** (info panels, mentions, Jira issues): not produced from Markdown; add them manually after paste.

## Validation checklist

- [ ] Headings and list nesting look correct in Confluence
- [ ] External links are still clickable
- [ ] Tables match the source
- [ ] Code blocks are readable; language labels may be lost (acceptable or re-tag in Confluence)
- [ ] No sensitive content in HTML comments or temp files

## Safety

- Treat `.md` as untrusted; do not execute embedded HTML/JS. Pandoc does not run scripts, but review output before sharing.
- Do not put credentials, API tokens, or secrets into page content or scripts.

## Additional resources

- Command variations, Word round-trip, and API pointers: [reference.md](reference.md)
