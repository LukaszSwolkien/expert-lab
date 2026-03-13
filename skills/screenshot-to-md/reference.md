# Screenshot to Markdown Reference

## Extraction methods

### A) Direct OCR (clean screenshots)

Use when the image has clear, high-contrast text (typical UI screenshots, terminal captures).

```bash
python3 - <<'PY'
from pathlib import Path
import pytesseract
from PIL import Image

input_img = Path("screenshot.png")
output_md = input_img.with_suffix(".md")

image = Image.open(input_img)
text = pytesseract.image_to_string(image)

output_md.write_text(text.strip() + "\n", encoding="utf-8")
print(f"Saved: {output_md}")
PY
```

### B) Preprocessed OCR (low quality or noisy images)

Use when direct extraction produces poor results (blurry photos, low contrast, small text).

```bash
python3 - <<'PY'
from pathlib import Path
import pytesseract
from PIL import Image, ImageEnhance, ImageFilter

input_img = Path("screenshot.png")
output_md = input_img.with_suffix(".md")

image = Image.open(input_img)
image = image.convert("L")
image = ImageEnhance.Contrast(image).enhance(2.0)
image = image.filter(ImageFilter.SHARPEN)
image = image.resize((image.width * 2, image.height * 2), Image.LANCZOS)

text = pytesseract.image_to_string(image)

output_md.write_text(text.strip() + "\n", encoding="utf-8")
print(f"Saved: {output_md}")
PY
```

### C) Diagram detection and Mermaid conversion

If the input image is a diagram (or contains diagram regions), convert the visual logic to a Mermaid block instead of relying on OCR text alone.

1. Inspect the image content — look for boxes, arrows, lanes, decision diamonds, or labeled connections.
2. If the image is a **diagram**:
   - Recreate the diagram logic as a fenced Mermaid block.
   - Add an HTML comment above it referencing the source image.
   - Use OCR on text labels within the diagram to ensure accuracy.
3. If the image is **mixed** (text + diagram):
   - Extract text portions via OCR as normal Markdown.
   - Convert diagram portions to Mermaid blocks.

#### Mermaid output example

```markdown
<!-- source: architecture-screenshot.png -->
` ``mermaid
flowchart LR
    A[Collector] --> B[Processor]
    B --> C[Exporter]
    C --> D[(Splunk O11y)]
` ``
```

(The backtick spaces above are for escaping only; use real fenced blocks in output.)

## Post-processing rules

- Remove stray single characters that are obvious OCR noise.
- Merge broken words split across lines when context is clear.
- Preserve code blocks and monospaced content as fenced code blocks.
- Keep tabular data aligned or convert to Markdown tables when recognizable.

## Validation checklist

- Output file exists as `FILENAME.md`.
- Extracted text covers all visible text areas from the source image.
- No major OCR artifacts remain (random symbols, garbled words).
- Structure is preserved where recognizable (headings, lists, code).
- Low-confidence sections are flagged or noted.
- Diagrams are represented as Mermaid blocks with valid syntax.
- Each Mermaid block has an `<!-- source: FILE.ext -->` comment.
