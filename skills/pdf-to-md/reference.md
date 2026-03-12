# PDF to Markdown Reference

## Typical extraction paths

### A) Text-based PDF (preferred)

Use `pdfplumber` or `pypdf` to extract text in reading order.

```bash
python3 - <<'PY'
from pathlib import Path
import pdfplumber

input_pdf = Path("file.pdf")
output_md = input_pdf.with_suffix(".md")

chunks = []
with pdfplumber.open(input_pdf) as pdf:
    for idx, page in enumerate(pdf.pages, 1):
        text = page.extract_text() or ""
        chunks.append(f"## Page {idx}\n\n{text.strip()}\n")

output_md.write_text("\n".join(chunks).strip() + "\n", encoding="utf-8")
print(f"Saved: {output_md}")
PY
```

### B) Scanned PDF (OCR fallback)

Use OCR only when direct text extraction returns near-empty content.

```bash
python3 - <<'PY'
from pathlib import Path
from pdf2image import convert_from_path
import pytesseract

input_pdf = Path("file.pdf")
output_md = input_pdf.with_suffix(".md")

pages = convert_from_path(str(input_pdf))
chunks = []
for idx, image in enumerate(pages, 1):
    text = pytesseract.image_to_string(image)
    chunks.append(f"## Page {idx}\n\n{text.strip()}\n")

output_md.write_text("\n".join(chunks).strip() + "\n", encoding="utf-8")
print(f"Saved: {output_md}")
PY
```

## Post-processing rules

- Merge hard-wrapped lines into paragraphs.
- Keep bullets/numbering if markers are explicit.
- Convert URLs/emails to Markdown links where possible.
- Keep page separators only if they add clarity.

## Validation checklist

- Output file exists as `FILENAME.md`.
- No missing major sections compared to source PDF.
- Paragraphs are readable (no severe line-break noise).
- Links are preserved when present in source.
- OCR path used only when necessary (to avoid noisy text).
