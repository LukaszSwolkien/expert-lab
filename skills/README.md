# Project Skills Index

This directory contains reusable skills for document conversion and extraction workflows.

## Available skills

### `docx-to-md`
- Path: `skills/docx-to-md/SKILL.md`
- Purpose: Convert Word (`.docx`) documents to structured Markdown.
- Typical triggers: "convert docx to md", "extract links from docx", "word to markdown".

### `md-to-docx`
- Path: `skills/md-to-docx/SKILL.md`
- Purpose: Convert Markdown to Word (`.docx`) with preserved structure and links.
- Typical triggers: "md to docx", "markdown to word", "convert md to word document".

### `md-to-pdf`
- Path: `skills/md-to-pdf/SKILL.md`
- Purpose: Convert Markdown to PDF with Mermaid and LaTeX rendering.
- Typical triggers: "md to pdf", "markdown export", "render mermaid in pdf", "latex in pdf".

### `pdf-to-md`
- Path: `skills/pdf-to-md/SKILL.md`
- Purpose: Convert PDF to Markdown (text-first with OCR fallback).
- Typical triggers: "pdf to markdown", "extract text from pdf", "convert scanned pdf to md".

### `confluence-doc-to-md`
- Path: `skills/confluence-doc-to-md/SKILL.md`
- Purpose: Convert Confluence-exported `.doc` files (MIME/HTML format) to structured Markdown with embedded images and Mermaid diagrams.
- Typical triggers: "convert confluence doc to md", "confluence export to markdown", ".doc file from confluence".

### `screenshot-to-md`
- Path: `skills/screenshot-to-md/SKILL.md`
- Purpose: Extract text from screenshots and images via OCR and output as Markdown.
- Typical triggers: "read text from screenshot", "extract text from image", "screenshot to markdown".

### `one-on-one`
- Path: `skills/one-on-one/SKILL.md`
- Purpose: Prepare structured 1:1 agendas and draft actionable feedback from raw observations or notes.
- Typical triggers: `/11` command within the `engineering-leader` persona.

### `survey-builder`
- Path: `skills/survey-builder/SKILL.md`
- Purpose: Build situation-specific feedback surveys (360, team pulse, trust diagnostics) with tailored questions, segmentation, and interpretation guides.
- Typical triggers: Need to collect structured feedback before a people or team decision; `engineering-leader` persona identifies assumptions about team dynamics without supporting data.

## Conventions used in these skills

- `SKILL.md` contains concise operational workflow and constraints.
- `reference.md` contains command examples and validation checklists.
- Output files are expected next to the source document unless specified otherwise.

## Maintenance notes

- Keep `SKILL.md` concise and task-focused.
- Put longer examples/scripts in `reference.md`.
- Avoid hardcoded credentials, tokens, and secrets in all examples.
