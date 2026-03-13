# DOCX to Markdown Reference

## Minimal extraction flow

### 1) Extract paragraph text

```bash
unzip -p "file.docx" word/document.xml | python3 -c "
import sys
from xml.etree import ElementTree as ET
content = sys.stdin.read()
root = ET.fromstring(content)
ns = {'w': 'http://schemas.openxmlformats.org/wordprocessingml/2006/main'}
for para in root.findall('.//w:p', ns):
    texts = [t.text for t in para.findall('.//w:t', ns) if t.text]
    print(''.join(texts) if texts else '')
"
```

### 2) Read URL relations

```bash
unzip -p "file.docx" word/_rels/document.xml.rels
```

### 3) Read hyperlink anchors from document

```bash
unzip -p "file.docx" word/document.xml | python3 -c "
import sys
from xml.etree import ElementTree as ET
content = sys.stdin.read()
root = ET.fromstring(content)
ns = {
    'w': 'http://schemas.openxmlformats.org/wordprocessingml/2006/main',
    'r': 'http://schemas.openxmlformats.org/officeDocument/2006/relationships'
}
for link in root.findall('.//w:hyperlink', ns):
    rid = link.get('{http://schemas.openxmlformats.org/officeDocument/2006/relationships}id')
    texts = [t.text for t in link.findall('.//w:t', ns) if t.text]
    print(f'rId: {rid}, Text: {\"\".join(texts)}')
"
```

### 4) Extract embedded images

```bash
DOCX="file.docx"
FNAME=$(basename "${DOCX%.*}")
mkdir -p "assets/${FNAME}"
unzip -o "$DOCX" "word/media/*" -d /tmp/docx_extract
cp /tmp/docx_extract/word/media/* "assets/${FNAME}/"
rm -rf /tmp/docx_extract
```

### 5) Classify images and convert diagrams to Mermaid

For each extracted image:

1. Inspect the image content visually (or via the document context where it is referenced).
2. If the image is a **diagram** (flowchart, sequence, architecture, state machine, ER, Gantt, etc.):
   - Recreate the diagram logic as a fenced Mermaid block.
   - Add an HTML comment above it referencing the source asset.
   - Do **not** include an `![](assets/...)` link for this image.
3. If the image is a **non-diagram** (photo, screenshot, icon, chart that cannot be represented in Mermaid):
   - Include it as `![description](assets/FILENAME/imageN.png)`.

#### Mermaid output example

```markdown
<!-- source: assets/my-report/image3.png -->
` ``mermaid
flowchart LR
    A[Collector] --> B[Processor]
    B --> C[Exporter]
    C --> D[(Splunk O11y)]
` ``

![Architecture photo](assets/my-report/image5.png)
```

(The backtick spaces above are for escaping only; use real fenced blocks in output.)

## Mapping rules

- `w:pStyle` title-like style -> `#`
- Heading 1 -> `##`
- Heading 2 -> `###`
- Heading 3 -> `####`
- `w:numPr` -> bullet/numbered list
- Hyperlinks -> `[anchor](url)` using relation map
- Diagram images -> fenced ` ```mermaid ` block with `<!-- source: ... -->` comment
- Non-diagram images -> `![alt](assets/FILENAME/imageN.ext)`

## Validation checklist

- Every extracted `rId` used in text resolves to URL in `.rels`.
- No raw XML tags in final Markdown.
- Heading levels are monotonic and readable.
- Output saved as `FILENAME.md` next to source.
- All images from `word/media/` are extracted into `assets/<FILENAME>/` in their original format.
- Diagrams are represented as Mermaid blocks with valid syntax.
- No `![](assets/...)` links exist for images that have a Mermaid equivalent.
- Each Mermaid block has an `<!-- source: assets/FILENAME/imageN.png -->` comment.
