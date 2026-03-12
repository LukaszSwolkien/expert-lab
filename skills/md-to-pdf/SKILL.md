---
name: md-to-pdf
description: Converts Markdown to PDF with Mermaid diagrams and LaTeX formulas rendered correctly. Use when the user asks for .md to .pdf export, printable docs from Markdown, or PDF generation with Mermaid and math support.
---

# Markdown to PDF (Mermaid + LaTeX)

## When to use

Use this skill when Markdown must be exported to PDF with correct rendering of:
- Mermaid diagrams
- Inline and block LaTeX formulas

## Prerequisites

- Node.js with `npm`/`npx`
- `@mermaid-js/mermaid-cli`
- `md-to-pdf`
- `katex` (for server-side formula rendering)

## Quick workflow

1. Parse Markdown file and find Mermaid code blocks.
2. Render each Mermaid block to SVG via `@mermaid-js/mermaid-cli`.
3. Replace Mermaid blocks with image references to generated SVG files.
4. Render LaTeX:
   - `$$...$$` as display math
   - `$...$` as inline math
5. Add front matter for PDF CSS and page settings.
6. Generate PDF with `md-to-pdf`.
7. Validate final PDF (diagrams visible, formulas rendered, no broken links/images).

## Constraints

- Keep conversion deterministic and reproducible.
- Avoid changing the document semantics while rendering.
- Prefer local assets generated during conversion over remote dynamic rendering.

## Safety notes

- Do not hardcode secrets/tokens in scripts or markdown front matter.
- Validate input file path and fail clearly when file does not exist.

## Additional resources

- Detailed script template and troubleshooting: [reference.md](reference.md)
