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
