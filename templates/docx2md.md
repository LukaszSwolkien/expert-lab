# Prompt: Convert Word Document to Markdown

Use this prompt to convert a `.docx` file to Markdown with proper formatting and hyperlinks preserved.

---

## Prompt

```
Convert "[FILENAME].docx" to a Markdown file with the following requirements:

1. **Extract all text content** from the Word document preserving the document structure
2. **Preserve all hyperlinks** - extract link URLs from word/_rels/document.xml.rels and match them with their anchor text from word/document.xml
3. **Apply proper Markdown formatting:**
   - Use # for the document title (H1)
   - Use ## for main sections (H2)
   - Use ### for subsections (H3)
   - Use #### for sub-subsections (H4)
   - Convert bullet points to Markdown lists (- or *)
   - Convert numbered lists appropriately
   - Use nested lists where applicable
   - Format hyperlinks as [text](url)
4. **Maintain paragraph spacing** for readability
5. **Save the output** as "[FILENAME].md" in the same directory
```

---

## Technical Notes

For AI assistants, here's the extraction approach:

### Step 1: Extract text with structure
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

### Step 2: Extract hyperlink URLs
```bash
unzip -p "file.docx" word/_rels/document.xml.rels
```

### Step 3: Match hyperlinks with their anchor text
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
    print(f'rId: {rid}, Text: {\"".join(texts)}')
"
```

### Step 4: Create Markdown
Combine the extracted text, apply heading hierarchy based on document structure, and insert hyperlinks in `[text](url)` format.
