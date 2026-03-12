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

## Mapping rules

- `w:pStyle` title-like style -> `#`
- Heading 1 -> `##`
- Heading 2 -> `###`
- Heading 3 -> `####`
- `w:numPr` -> bullet/numbered list
- Hyperlinks -> `[anchor](url)` using relation map

## Validation checklist

- Every extracted `rId` used in text resolves to URL in `.rels`.
- No raw XML tags in final Markdown.
- Heading levels are monotonic and readable.
- Output saved as `FILENAME.md` next to source.
