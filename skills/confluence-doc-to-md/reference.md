# Confluence DOC to Markdown Reference

## Format detection

```bash
file "input.doc"
# Confluence export: "news or mail text, ASCII text, with CRLF line terminators"
# Binary Word:       "Microsoft Word" or "Composite Document File"
```

## Extraction flow

### 1) Parse MIME and list parts

```python
import email, quopri

with open("input.doc", "r") as f:
    msg = email.message_from_string(f.read())

for i, part in enumerate(msg.walk()):
    ct = part.get_content_type()
    cl = part.get("Content-Location", "")
    cte = part.get("Content-Transfer-Encoding", "")
    print(f"Part {i}: type={ct} | CTE={cte} | Location={cl}")
```

### 2) Extract HTML content

```python
html_content = ""
for part in msg.walk():
    if part.get_content_type() == "text/html":
        payload = part.get_payload(decode=False)
        cte = part.get("Content-Transfer-Encoding", "")
        if "quoted-printable" in cte:
            payload = quopri.decodestring(payload.encode()).decode("utf-8", errors="replace")
        html_content = payload
        break
```

### 3) Map image references

Match `<img src="HASH" alt="NAME">` tags in HTML to MIME parts by `Content-Location`:

```python
import re, base64

# Build hash -> MIME part lookup
cid_parts = {}
for part in msg.walk():
    cl = part.get("Content-Location", "")
    if cl.startswith("file:///C:/"):
        hashname = cl.replace("file:///C:/", "")
        cid_parts[hashname] = part

# Find image tags with alt text
img_tags = re.findall(r"<img[^>]+>", html_content)
for tag in img_tags:
    src = re.search(r'src=["\']([^"\']+)["\']', tag)
    alt = re.search(r'alt=["\']([^"\']*)["\']', tag)
    src_val = src.group(1) if src else ""
    alt_val = alt.group(1) if alt else ""
    print(f"src={src_val[:60]} | alt={alt_val}")
```

### 4) Extract embedded images

```python
import os

assets_dir = "assets/FILENAME"
os.makedirs(assets_dir, exist_ok=True)

# Skip Confluence decorator icons
SKIP_ALTS = {"(blue star)", "(warning)", "(info)", "(error)", "(tick)", "(cross)", "(question)", "(plus)", "(minus)", "(star)"}

for hashname, filename in image_map.items():
    if hashname in cid_parts:
        part = cid_parts[hashname]
        payload = part.get_payload(decode=True)
        if payload is None:
            raw_payload = part.get_payload(decode=False)
            payload = base64.b64decode(raw_payload)
        filepath = os.path.join(assets_dir, filename)
        with open(filepath, "wb") as f:
            f.write(payload)
```

### 5) Convert HTML to Markdown

Use a custom `HTMLParser` subclass to walk the DOM:

```python
from html.parser import HTMLParser

class HTMLToMarkdown(HTMLParser):
    def __init__(self):
        super().__init__()
        self.result = []
        self.skip = False         # True inside <style>/<script>
        self.in_table = False
        self.in_td = False
        self.current_row = []
        self.rows = []

    def handle_starttag(self, tag, attrs):
        if tag in ("style", "script"):
            self.skip = True
        elif tag in ("h1", "h2", "h3", "h4", "h5", "h6"):
            self.result.append("\n" + "#" * int(tag[1]) + " ")
        elif tag == "p":
            self.result.append("\n")
        elif tag == "br":
            self.result.append("\n")
        elif tag == "li":
            self.result.append("\n- ")
        elif tag == "table":
            self.in_table = True; self.rows = []
        elif tag == "tr":
            self.current_row = []
        elif tag in ("td", "th"):
            self.in_td = True; self.current_row.append("")
        elif tag == "a":
            href = dict(attrs).get("href", "")
            self.result.append(f"[")
            self._pending_href = href
        elif tag == "img":
            alt = dict(attrs).get("alt", "")
            # Image handling done via image_map; record placeholder
            self.result.append(f"[Image: {alt}]")

    def handle_endtag(self, tag):
        if tag in ("style", "script"):
            self.skip = False
        elif tag in ("h1", "h2", "h3", "h4", "h5", "h6"):
            self.result.append("\n")
        elif tag == "p":
            self.result.append("\n")
        elif tag in ("td", "th"):
            self.in_td = False
        elif tag == "tr":
            if self.current_row:
                self.rows.append(self.current_row)
        elif tag == "table":
            self.in_table = False
            self._flush_table()
        elif tag == "a":
            href = getattr(self, "_pending_href", "")
            self.result.append(f"]({href})")

    def handle_data(self, data):
        if self.skip:
            return
        text = data.strip()
        if not text:
            return
        if self.in_td and self.current_row:
            self.current_row[-1] += text
        else:
            self.result.append(text + " ")

    def _flush_table(self):
        if not self.rows:
            return
        max_cols = max(len(r) for r in self.rows)
        for r in self.rows:
            while len(r) < max_cols:
                r.append("")
        col_widths = [
            max(max(len(str(r[i]).strip()) for r in self.rows), 3)
            for i in range(max_cols)
        ]
        self.result.append("\n")
        for idx, row in enumerate(self.rows):
            line = "| " + " | ".join(
                str(c).strip().ljust(w) for c, w in zip(row, col_widths)
            ) + " |"
            self.result.append(line + "\n")
            if idx == 0:
                sep = "| " + " | ".join("-" * w for w in col_widths) + " |"
                self.result.append(sep + "\n")
        self.result.append("\n")

    def get_text(self):
        return "".join(self.result)
```

### 6) Classify images and convert diagrams to Mermaid

For each extracted image:

1. Inspect the image content visually (or via the document context where it is referenced).
2. If the image is a **diagram** (flowchart, sequence, architecture, state machine, ER, component, etc.):
   - Recreate the diagram logic as a fenced Mermaid block.
   - Add an HTML comment above it referencing the source asset.
   - Do **not** include an `![](assets/...)` link for this image.
3. If the image is a **non-diagram** (photo, screenshot, icon, chart that cannot be represented in Mermaid):
   - Include it as `![description](assets/FILENAME/imageN.png)`.

#### Mermaid output example

```markdown
<!-- source: assets/my-erd/component-diagram.png -->
` ``mermaid
graph TB
    subgraph Solution
        SVC[Service]
        EXT[Extension]
    end
    SVC --> DB[(Postgres)]
    SVC --> RD[(Redis)]
` ``

![Dashboard screenshot](assets/my-erd/dashboard.png)
```

(The backtick spaces above are for escaping only; use real fenced blocks in output.)

## Confluence-specific handling

| Confluence Element | Markdown Equivalent |
| --- | --- |
| `<ac:structured-macro>` info/warning/note | `> **Note/Warning:** ...` blockquote |
| Status lozenges (`<ac:parameter>`) | Inline text in **bold** |
| `<ri:attachment>` image refs | Resolve via MIME part `Content-Location` hash |
| Confluence user mentions | `@DisplayName` plain text |
| Confluence emoticons (`(blue star)`, etc.) | Omit entirely |
| Office XML namespaces (`xmlns:o`, `xmlns:w`) | Strip during parsing |

## Validation checklist

- Format detected as MIME/HTML (not binary Word).
- All MIME parts enumerated; `text/html` part decoded from quoted-printable.
- Image hash → alt text mapping complete; decorator icons excluded.
- All meaningful images extracted to `assets/<FILENAME>/` in original format.
- Diagrams represented as Mermaid blocks with valid syntax.
- No `![](assets/...)` links for images that have a Mermaid equivalent.
- Each Mermaid block has an `<!-- source: ... -->` comment.
- Tables render correctly (consistent column count, header separator row).
- No raw HTML tags in final Markdown output.
- Heading levels are monotonic and readable.
- Output file placed per file output rules (not in same dir as source `docs/` folder).
