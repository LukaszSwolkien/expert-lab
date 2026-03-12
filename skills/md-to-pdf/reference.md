# Markdown to PDF Reference

## Baseline commands

```bash
# Mermaid -> SVG
npx --yes @mermaid-js/mermaid-cli -i diagram.mmd -o diagram.svg

# Markdown -> PDF
npx --yes md-to-pdf output.md
```

## Typical render order

1. Mermaid blocks -> SVG files
2. LaTeX blocks/inline -> KaTeX HTML
3. Add front matter CSS/page options
4. Run `md-to-pdf` on rendered markdown

## Minimal dependencies

```bash
npm install katex
```

## Validation checklist

- Mermaid diagrams are visible in PDF.
- Math is rendered (no raw `$...$` / `$$...$$` left).
- PDF page format and margins match expected style.
- No missing image paths.

## Troubleshooting

- Mermaid does not render:
  - Check `npx --yes @mermaid-js/mermaid-cli --help`
  - Ensure browser runtime dependencies for Mermaid CLI are available
- Formulas still visible as raw text:
  - Verify KaTeX rendering happens before `md-to-pdf`
- PDF appears empty:
  - Validate YAML front matter syntax and indentation
