# Marp Splunk Baseline Reference

This reference is self-contained and tied to the skill bundle (not any temporary project path).

## Source-of-truth inside this skill

- Theme baseline: `skills/marp-splunk-presentation/assets/splunk.css`
- Asset guidance: `skills/marp-splunk-presentation/assets/README.md`
- Skill workflow: `skills/marp-splunk-presentation/SKILL.md`

When using this skill in a new repo, copy `assets/splunk.css` into the target deck theme folder and then wire Marp config to that copied path.

## Current baseline decisions

1. **Slides are content-only**
   - Branding should not be hardcoded inside individual slides.
   - Reusable branding belongs in `splunk.css`.

2. **Theme is external and named `splunk`**
   - target deck frontmatter should use:
     - `marp: true`
     - `theme: splunk`
     - `paginate: true`

3. **Default visual style**
   - White slide background.
   - Fixed top header gradient from orange to pink.
   - Black headings and dark body text for readability.

4. **Footer logo behavior**
   - Splunk logo is added globally via CSS `section::before`.
   - Positioned on the bottom-right and lifted above page numbers.
   - For current white background baseline, use black logo variant (`...-k.png`) in the deck assets folder.

5. **Preview reliability**
   - Keep both `.marprc.yml` and `.vscode/settings.json` in sync with the same copied theme path.
   - If preview shows default theme, verify theme registration and reload preview/window.

## Validation command

```bash
marp --no-stdin "<your-slides>.md" -o "<your-slides>.preview.html"
```

Delete the generated preview file when done validating.

## Export formats

Use these output switches based on the requested artifact:

- `--pdf` -> PDF deck
- default output (`.html`) -> HTML deck
- `--pptx` -> PowerPoint file
- `--notes` -> speaker notes text file
- `--image png|jpeg` -> first slide image
- `--images png|jpeg` -> all slides as image sequence
