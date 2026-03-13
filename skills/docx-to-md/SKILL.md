---
name: docx-to-md
description: Converts .docx documents to Markdown while preserving heading structure, lists, and hyperlinks. Use when the user asks to transform Word files, extract links from .docx, or produce readable .md documentation from DOCX sources.
---

# DOCX to Markdown

## When to use

Use this skill when the task involves converting Word files (`.docx`) into Markdown with preserved structure and links.

## Quick workflow

1. Confirm input and output paths:
   - Input: `FILE.docx`
   - Output: `FILE.md` in the same directory
2. Extract text structure from `word/document.xml`.
3. Extract hyperlink relations from `word/_rels/document.xml.rels`.
4. Match hyperlink anchors (`w:hyperlink` with `r:id`) to URL mappings.
5. Extract embedded images from `word/media/` into `assets/<FILENAME>/` (where `<FILENAME>` is the docx name without extension). Keep original image formats. This keeps assets from different source files separate.
6. Classify each extracted image:
   - **Diagram** (flowcharts, sequence diagrams, architecture diagrams, state machines, etc.): convert to a Mermaid code block and embed inline in the Markdown. Do **not** add an image link for this asset. Add an HTML comment above the Mermaid block noting the source asset, e.g. `<!-- source: assets/FILENAME/image3.png -->`.
   - **Non-diagram** (photos, screenshots, icons, etc.): embed as a standard Markdown image link `![alt](assets/FILENAME/imageN.png)`.
7. Build Markdown:
   - Title as `#`
   - Main sections as `##`
   - Subsections as `###` / `####`
   - Bullets and numbered lists as Markdown lists
   - Links as `[text](url)`
   - Diagrams as fenced Mermaid blocks (` ```mermaid `)
   - Other images as `![description](assets/FILENAME/imageN.png)`
8. Validate:
   - No missing links
   - Every image in `word/media/` is accounted for (either as Mermaid or as an image link)
   - Mermaid blocks render valid syntax
   - No image links for assets that have a corresponding Mermaid diagram
   - Readable spacing between paragraphs
   - Output file saved in the expected location

## Constraints

- Preserve all hyperlinks whenever relation IDs are present.
- Prefer deterministic transformation over stylistic rewriting.
- Keep content faithful to source document wording.
- Keep original image formats when extracting assets.
- Diagrams must be converted to Mermaid and must not appear as image links in the final output.
- Each Mermaid block must include an HTML comment identifying the source asset file.

## Additional resources

- Detailed extraction snippets and command examples: [reference.md](reference.md)
