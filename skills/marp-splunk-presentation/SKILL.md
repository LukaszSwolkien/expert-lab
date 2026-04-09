---
name: marp-splunk-presentation
description: Build, style, and export Marp slide decks with a separate Splunk-branded CSS theme and local logo assets. Use when creating or updating presentation files (.md), setting up theme wiring, adding Splunk footer/header branding, troubleshooting Marp preview/theme loading, or exporting slides to PDF/HTML/PPTX/notes/images.
---

# Marp + Splunk Presentation

## Slash commands

- `/pptx <file>` — export the given Marp `.md` file to PPTX
- `/pdf <file>` — export the given Marp `.md` file to PDF

When the user sends a slash command, skip all other steps and go straight to export (Step 8) using `--allow-local-files`. Place the output next to the source file with the matching extension.

## When to use

Use this skill when the user asks to:
- create or refine a Marp presentation
- apply Splunk-branded styling via external CSS
- keep styling separate from slide content
- add Splunk logos/assets and footer/header branding
- fix Marp preview showing default theme
- export slides to PDF, HTML, PPTX, notes, or images

## Skill-contained assets

This skill lives at `~/.codex/skills/marp-splunk-presentation/` and ships with:

- Theme CSS: `assets/splunk.css`
- Logo variants: `assets/logo-splunk-acc-rgb-k.png`, `assets/logo-splunk-acc-rgb-w.png`
- Asset notes: `assets/README.md`
- Baseline guidance: [reference.md](reference.md)

Always reference these assets in place. Do not copy them into deck project folders.

## Execution flow

### Step 1: Confirm slide frontmatter

Ensure the deck has:

```yaml
---
marp: true
theme: splunk
paginate: true
---
```

Keep theme and layout logic out of slide markdown whenever possible.

### Step 2: Keep styling in standalone CSS

Implement visual identity in the skill's `assets/splunk.css`:
- Declare theme metadata: `/* @theme splunk */`
- Import base Marp theme: `@import "gaia";`
- Configure colors, typography, table style, header bar, footer logo

Do not inline CSS into `slides.md`.

### Step 3: Use local logo assets

Prefer local assets over remote URLs.

If logo files are packed in zip or provided by design:
1. list archive content
2. extract only required files to your target deck assets folder
3. pick variant by background:
   - dark background -> white logo (`...-w.png`)
   - light background -> black logo (`...-k.png`)

### Step 4: Theme wiring for CLI and preview

Point both configs to the skill's own CSS using the expanded `$HOME` path (Marp CLI does not expand `~`):

1. `.marprc.yml`
```yaml
themeSet:
  - "$HOME/.codex/skills/marp-splunk-presentation/assets/splunk.css"
```

2. `.vscode/settings.json`
```json
{
  "markdown.marp.themes": [
    "$HOME/.codex/skills/marp-splunk-presentation/assets/splunk.css"
  ]
}
```

Replace `$HOME` with the actual absolute home path.

Since the path is machine-specific, `.marprc.yml` should not be committed — add it to `.gitignore`.

### Step 5: Default footer logo pattern

For global branding, use `section::before` in CSS instead of repeating image markdown:
- `position: absolute`
- bottom-right or bottom-left as requested
- keep clear of pagination (`section::after`) by adjusting bottom offset and slide padding
- use `pointer-events: none`

If it overlaps page numbers, move logo up and/or reduce width.

### Step 6: Header gradient pattern

For Splunk-style decks, prefer a fixed top gradient bar:
- white base slide background
- top bar gradient from orange to pink
- dark body text for contrast

### Step 7: Validate render

Always render after substantive changes:

```bash
marp --no-stdin "<your-slides>.md" -o "<your-slides>.preview.html"
```

Then remove preview artifact if it was only for validation.

### Step 8: Export to requested format

When the user asks to export, use the matching Marp command template:

- **PDF**
```bash
marp --no-stdin "<your-slides>.md" --pdf -o "<your-slides>.pdf"
```

- **HTML**
```bash
marp --no-stdin "<your-slides>.md" -o "<your-slides>.html"
```

- **PPTX**
```bash
marp --no-stdin "<your-slides>.md" --pptx -o "<your-slides>.pptx"
```

If the deck uses local assets (logos, local CSS `url(...)`, local background images), prefer:

```bash
marp --no-stdin "<your-slides>.md" --pptx --allow-local-files -o "<your-slides>.pptx"
```

- **Speaker notes text**
```bash
marp --no-stdin "<your-slides>.md" --notes -o "<your-slides>-notes.txt"
```

- **First slide image**
```bash
marp --no-stdin "<your-slides>.md" --image png -o "<your-slides>-slide1.png"
```

- **All slides as images**
```bash
marp --no-stdin "<your-slides>.md" --images png -o "<your-output-dir>"
```

If theme loading is inconsistent in CLI, add explicit theme-set:

```bash
marp --no-stdin --theme-set "<your-theme-dir>/splunk.css" "<your-slides>.md" --pdf -o "<your-slides>.pdf"
```

## Troubleshooting quick checks

If custom theme is not applied:
1. Verify `theme: splunk` in slide frontmatter.
2. Verify theme metadata uses CSS comment syntax: `/* @theme splunk */`.
3. Verify `.marprc.yml` and `.vscode/settings.json` include the theme path.
4. Reload editor window / reopen Marp preview.

If logo is missing:
1. Check logo file exists in your deck assets folder.
2. Check CSS `url(...)` path is correct relative to slide render context.
3. Prefer local png path over remote svg for reliability.

If Marp warns that local file access is blocked:
1. Re-run export with `--allow-local-files`.
2. Re-check relative paths in CSS and markdown.

If export fails with browser startup errors (for example `TargetCloseError`):
1. Re-run in a less restricted environment (outside strict sandbox/CI isolation).
2. Retry the same command after confirming headless browser execution is allowed.

## Output expectations

After applying this skill:
- slide markdown remains content-focused
- branding is centralized in `splunk.css`
- logos/assets are local and deterministic
- CLI render and editor preview both show the same theme

## Reference baseline

Use the current production-ready presentation setup as the baseline:
- [reference.md](reference.md)
