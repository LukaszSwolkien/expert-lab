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

### C) Extract embedded images

```bash
PDF="file.pdf"
FNAME=$(basename "${PDF%.*}")
mkdir -p "assets/${FNAME}"
python3 - <<'PY'
from pathlib import Path
import pdfplumber

input_pdf = Path("file.pdf")
fname = input_pdf.stem
assets_dir = Path(f"assets/{fname}")

with pdfplumber.open(input_pdf) as pdf:
    for idx, page in enumerate(pdf.pages):
        for img_idx, img in enumerate(page.images):
            img_data = page.crop(
                (img["x0"], img["top"], img["x1"], img["bottom"])
            ).to_image()
            img_data.save(assets_dir / f"image_p{idx+1}_{img_idx+1}.png")
PY
```

### D) Classify images and convert diagrams to Mermaid

For each extracted image:

1. Inspect the image content visually or infer from surrounding PDF text context.
2. If the image is a **diagram** (flowchart, sequence, architecture, state machine, ER, Gantt, etc.):
   - Recreate the diagram logic as a fenced Mermaid block.
   - Add an HTML comment above it referencing the source asset.
   - Do **not** include an `![](assets/...)` link for this image.
3. If the image is a **non-diagram** (photo, screenshot, icon, chart that cannot be represented in Mermaid):
   - Include it as `![description](assets/FILENAME/imageN.ext)`.

#### Mermaid output example

```markdown
<!-- source: assets/my-report/image_p2_1.png -->
` ``mermaid
flowchart LR
    A[Collector] --> B[Processor]
    B --> C[Exporter]
    C --> D[(Splunk O11y)]
` ``

![Architecture photo](assets/my-report/image_p3_1.png)
```

(The backtick spaces above are for escaping only; use real fenced blocks in output.)

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
- All images from the PDF are extracted into `assets/<FILENAME>/` in their original format.
- Diagrams are represented as Mermaid blocks with valid syntax.
- No `![](assets/...)` links exist for images that have a Mermaid equivalent.
- Each Mermaid block has an `<!-- source: assets/FILENAME/imageN.ext -->` comment.
