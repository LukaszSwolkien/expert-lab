# Markdown to DOCX Reference

## Conversion methods

### A) Pandoc (recommended)

Simplest path for most Markdown-to-DOCX conversions.

```bash
pandoc "file.md" -o "file.docx"
```

With a custom reference template for consistent styling:

```bash
pandoc "file.md" --reference-doc="template.docx" -o "file.docx"
```

### B) python-docx (programmatic)

Use when pandoc is not available or fine-grained control over styles is needed.

```bash
python3 - <<'PY'
from pathlib import Path
from docx import Document
from docx.shared import Pt
import re

input_md = Path("file.md")
output_docx = input_md.with_suffix(".docx")

doc = Document()
lines = input_md.read_text(encoding="utf-8").splitlines()

heading_map = {1: "Title", 2: "Heading 1", 3: "Heading 2", 4: "Heading 3"}

for line in lines:
    stripped = line.strip()
    if not stripped:
        continue

    heading_match = re.match(r'^(#{1,4})\s+(.*)', stripped)
    if heading_match:
        level = len(heading_match.group(1))
        text = heading_match.group(2)
        style = heading_map.get(level, "Heading 3")
        doc.add_heading(text, level=level)
        continue

    if stripped.startswith(("- ", "* ")):
        doc.add_paragraph(stripped[2:], style="List Bullet")
        continue

    number_match = re.match(r'^\d+\.\s+(.*)', stripped)
    if number_match:
        doc.add_paragraph(number_match.group(1), style="List Number")
        continue

    doc.add_paragraph(stripped)

doc.save(str(output_docx))
print(f"Saved: {output_docx}")
PY
```

## Mapping rules

- `#` -> Title style
- `##` -> Heading 1
- `###` -> Heading 2
- `####` -> Heading 3
- `- ` / `* ` -> List Bullet style
- `1. ` -> List Number style
- `[text](url)` -> Word hyperlink
- Fenced code blocks -> monospaced paragraph or code style
- Plain paragraphs -> Normal style

## Validation checklist

- Output file exists as `FILENAME.docx` next to source.
- All Markdown headings map to correct Word heading styles.
- Hyperlinks are preserved and clickable.
- Bullet and numbered lists render with correct formatting.
- Code blocks are visually distinct from body text.
- No raw Markdown syntax visible in the final document.
