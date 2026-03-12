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
5. Build Markdown:
   - Title as `#`
   - Main sections as `##`
   - Subsections as `###` / `####`
   - Bullets and numbered lists as Markdown lists
   - Links as `[text](url)`
6. Validate:
   - No missing links
   - Readable spacing between paragraphs
   - Output file saved in the expected location

## Constraints

- Preserve all hyperlinks whenever relation IDs are present.
- Prefer deterministic transformation over stylistic rewriting.
- Keep content faithful to source document wording.

## Safety notes

- Treat `.docx` as untrusted input.
- Prefer safe XML parsing with limits for size/depth when possible.
- Do not embed credentials, API keys, or tokens in generated output.

## Additional resources

- Detailed extraction snippets and command examples: [reference.md](reference.md)
