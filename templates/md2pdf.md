# Converting Markdown to PDF with Mermaid and LaTeX

Instructions for AI: how to convert Markdown files containing Mermaid diagrams and LaTeX math formulas to PDF with proper rendering.

---

## Requirements

- Node.js (npm/npx)
- Python 3

---

## Conversion Process

### Step 1: Render Mermaid diagrams to SVG

```bash
# Install mermaid-cli (if needed)
npx --yes @mermaid-js/mermaid-cli -i diagram.mmd -o diagram.svg
```

### Step 2: Render LaTeX formulas to HTML (KaTeX)

LaTeX formulas must be converted server-side to HTML before generating PDF.

### Step 3: Generate PDF

```bash
npx --yes md-to-pdf output.md
```

---

## Complete Script

Create file `render-to-pdf.mjs`:

```javascript
import { readFileSync, writeFileSync, mkdirSync, existsSync } from 'fs';
import { execSync } from 'child_process';
import katex from 'katex';

// === CONFIGURATION ===
const INPUT_FILE = 'document.md';  // <-- change to your input file
const OUTPUT_DIR = 'mermaid-images';

// === MAIN LOGIC ===
let content = readFileSync(INPUT_FILE, 'utf-8');

// 1. Render Mermaid diagrams to SVG
if (!existsSync(OUTPUT_DIR)) {
    mkdirSync(OUTPUT_DIR);
}

let diagramCount = 0;
const mermaidBlocks = content.match(/```mermaid\n[\s\S]*?\n```/g) || [];

for (const block of mermaidBlocks) {
    diagramCount++;
    const mermaidCode = block.replace(/```mermaid\n/, '').replace(/\n```$/, '');
    const mmdFile = `${OUTPUT_DIR}/diagram-${diagramCount}.mmd`;
    const svgFile = `${OUTPUT_DIR}/diagram-${diagramCount}.svg`;
    
    writeFileSync(mmdFile, mermaidCode);
    execSync(`npx --yes @mermaid-js/mermaid-cli -i ${mmdFile} -o ${svgFile} -b white -w 700`, 
             { stdio: 'pipe' });
    
    content = content.replace(block, `![Diagram ${diagramCount}](${svgFile})`);
}

console.log(`Rendered ${diagramCount} Mermaid diagrams`);

// 2. Render LaTeX formulas (display mode: $$...$$)
content = content.replace(/\$\$([^$]+)\$\$/g, (match, tex) => {
    try {
        return `<div class="math-display">${katex.renderToString(tex.trim(), { 
            displayMode: true, 
            throwOnError: false,
            strict: false
        })}</div>`;
    } catch (e) {
        console.error('Math error:', tex.substring(0, 40));
        return match;
    }
});

// 3. Render LaTeX formulas (inline mode: $...$)
content = content.replace(/\$([^$\n]+)\$/g, (match, tex) => {
    try {
        return katex.renderToString(tex.trim(), { 
            displayMode: false, 
            throwOnError: false,
            strict: false
        });
    } catch (e) {
        return match;
    }
});

console.log('Rendered LaTeX formulas');

// 4. Add front matter with CSS for md-to-pdf
const frontMatter = `---
css: |
  @import url('https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css');
  body {
    font-family: "Times New Roman", Times, Georgia, serif;
    font-size: 11pt;
    line-height: 1.5;
    color: #000;
    max-width: 170mm;
    margin: 0 auto;
  }
  h1 { font-size: 18pt; text-align: center; margin-bottom: 20pt; }
  h2 { font-size: 14pt; border-bottom: 1px solid #333; padding-bottom: 4pt; margin-top: 20pt; }
  h3 { font-size: 12pt; margin-top: 14pt; }
  table { border-collapse: collapse; width: 100%; margin: 12pt 0; font-size: 10pt; }
  th, td { border: 1px solid #333; padding: 5pt 8pt; }
  th { background: #f0f0f0; }
  p { text-align: justify; margin: 8pt 0; }
  ul, ol { margin: 8pt 0; padding-left: 24pt; }
  img { max-width: 85%; display: block; margin: 16pt auto; }
  hr { border: none; border-top: 1px solid #999; margin: 20pt 0; }
  code { font-family: monospace; font-size: 10pt; background: #f5f5f5; padding: 1px 4px; }
  .math-display { margin: 14pt 0; text-align: center; overflow-x: auto; }
  .katex { font-size: 1.1em; }
pdf_options:
  format: A4
  margin:
    top: 22mm
    right: 22mm
    bottom: 22mm
    left: 22mm
  printBackground: true
---

`;

// 5. Save intermediate file
const outputMd = INPUT_FILE.replace('.md', '-rendered.md');
writeFileSync(outputMd, frontMatter + content);
console.log(`Created: ${outputMd}`);

// 6. Generate PDF
const outputPdf = INPUT_FILE.replace('.md', '.pdf');
execSync(`npx --yes md-to-pdf ${outputMd}`, { stdio: 'inherit' });

// 7. Rename PDF and remove intermediate file
const renderedPdf = outputMd.replace('.md', '.pdf');
execSync(`mv ${renderedPdf} ${outputPdf}`);
execSync(`rm ${outputMd}`);

console.log(`\nPDF created: ${outputPdf}`);
```

---

## Usage

```bash
# 1. Install KaTeX dependency
npm install katex

# 2. Edit INPUT_FILE in the script (or pass as argument)

# 3. Run conversion
node render-to-pdf.mjs
```

---

## Alternative: Python + subprocess

If you prefer Python:

```python
#!/usr/bin/env python3
import re
import subprocess
from pathlib import Path

def convert_md_to_pdf(input_file: str):
    """Convert Markdown with Mermaid and LaTeX to PDF."""
    
    content = Path(input_file).read_text(encoding='utf-8')
    output_dir = Path('mermaid-images')
    output_dir.mkdir(exist_ok=True)
    
    # 1. Mermaid → SVG
    mermaid_pattern = r'```mermaid\n(.*?)\n```'
    matches = list(re.finditer(mermaid_pattern, content, re.DOTALL))
    
    for i, match in enumerate(matches, 1):
        mmd_file = output_dir / f'diagram-{i}.mmd'
        svg_file = output_dir / f'diagram-{i}.svg'
        
        mmd_file.write_text(match.group(1))
        subprocess.run([
            'npx', '--yes', '@mermaid-js/mermaid-cli',
            '-i', str(mmd_file), '-o', str(svg_file), '-b', 'white', '-w', '700'
        ], capture_output=True)
        
        content = content.replace(match.group(0), f'![Diagram {i}]({svg_file})', 1)
    
    # 2. Save intermediate file (without LaTeX rendering - requires Node.js)
    rendered_md = input_file.replace('.md', '-rendered.md')
    
    # Front matter for md-to-pdf
    front_matter = '''---
css: |
  body { font-family: "Times New Roman", serif; font-size: 11pt; line-height: 1.5; }
  h1 { font-size: 18pt; text-align: center; }
  h2 { font-size: 14pt; border-bottom: 1px solid #333; }
  table { border-collapse: collapse; width: 100%; margin: 12pt 0; }
  th, td { border: 1px solid #333; padding: 5pt 8pt; }
  img { max-width: 85%; display: block; margin: 16pt auto; }
pdf_options:
  format: A4
  margin: { top: 22mm, right: 22mm, bottom: 22mm, left: 22mm }
---

'''
    
    Path(rendered_md).write_text(front_matter + content, encoding='utf-8')
    
    # 3. Convert to PDF
    subprocess.run(['npx', '--yes', 'md-to-pdf', rendered_md])
    
    # 4. Cleanup
    output_pdf = input_file.replace('.md', '.pdf')
    Path(rendered_md.replace('.md', '.pdf')).rename(output_pdf)
    Path(rendered_md).unlink()
    
    print(f'PDF: {output_pdf}')

if __name__ == '__main__':
    import sys
    convert_md_to_pdf(sys.argv[1] if len(sys.argv) > 1 else 'document.md')
```

**Note:** The Python version does not render LaTeX server-side. For full formula support, use the Node.js version with KaTeX.

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Formulas display as raw text | Make sure KaTeX is installed and script renders formulas before generating PDF |
| Mermaid doesn't render | Check if `npx @mermaid-js/mermaid-cli` works; may require Chrome/Chromium |
| PDF is empty | Check if YAML front matter is valid (indentation matters!) |
| Missing special characters | Add `<meta charset="UTF-8">` or use a font with full Unicode support |

---

## Summary

1. **Mermaid** → render to SVG via `@mermaid-js/mermaid-cli`
2. **LaTeX** → render to HTML via `katex` (Node.js, server-side)
3. **Markdown → PDF** → use `md-to-pdf` with front matter CSS
4. **Order**: Mermaid → LaTeX → PDF

This method works without installing LaTeX (texlive/xelatex) and produces high-quality PDF with properly rendered formulas and diagrams.
