# Expert Lab

A curated collection of expert personas and reusable skills for daily AI-assisted work. IDE-agnostic — works with Cursor, VS Code, or any agent that reads Markdown instructions.

## Structure

```
personas/               # how the assistant thinks and responds
  personas-map.md       # routing table and quick selection rules
  melt-expert.md
  agentic-observability.md
  due-diligence-expert.md
  communication-expert.md
  prompt-builder-expert.md
  tech-doc-assistant.md
skills/                 # how the assistant executes specific tasks
  confluence-doc-to-md/
  docx-to-md/
  md-to-docx/
  md-to-pdf/
  mermaid-diagrams/
  pdf-to-md/
  screenshot-to-md/
AGENTS.md               # execution rules: persona + skill selection
```

## Personas

| Persona | Description | Command |
|---|---|---|
| [MELT Expert](personas/melt-expert.md) | OpenTelemetry and Splunk/AppDynamics instrumentation | `/melt` |
| [Agentic Observability](personas/agentic-observability.md) | Architecture brainstorming for dual-agent observability systems | `/o11y` |
| [Due Diligence Expert](personas/due-diligence-expert.md) | Job-offer and recruiter credibility verification | `/dd`, `/dd-cred` |
| [Communication Expert](personas/communication-expert.md) | Proofreading and audience-tailored rewrites | `/pr`, `/pr-int`, `/pr-us`, `/pr-india`, `/pr-ee` |
| [Prompt Builder Expert](personas/prompt-builder-expert.md) | Adaptive prompt creation and optimization | `/builder` |
| [Tech Doc Assistant](personas/tech-doc-assistant.md) | Document-first technical educator with progressive depth | upload a document |

Default persona when no match is found: `prompt-builder-expert`.
Use `/none` to skip persona selection entirely.

## Skills

| Skill | Description |
|---|---|
| [Confluence DOC to Markdown](skills/confluence-doc-to-md/SKILL.md) | Convert Confluence-exported `.doc` (MIME/HTML) to Markdown |
| [DOCX to Markdown](skills/docx-to-md/SKILL.md) | Convert Word documents to structured Markdown |
| [Markdown to DOCX](skills/md-to-docx/SKILL.md) | Convert Markdown to Word documents |
| [Markdown to PDF](skills/md-to-pdf/SKILL.md) | Export Markdown to PDF with Mermaid and LaTeX support |
| [Mermaid Diagrams](skills/mermaid-diagrams/SKILL.md) | Create Mermaid diagrams for architecture, flow, and sequence views |
| [PDF to Markdown](skills/pdf-to-md/SKILL.md) | Convert PDF to Markdown (text-first, OCR fallback) |
| [Screenshot to Markdown](skills/screenshot-to-md/SKILL.md) | Extract text from screenshots via OCR to Markdown |

Skills are persona-agnostic and can be combined with any persona.

## How to use

1. The agent reads `AGENTS.md` and auto-selects a persona from `personas/personas-map.md`.
2. Override when needed: `Use persona X`, `Use skills A, B`, or use a command shortcut.
3. Execution order: select persona, select skills, execute with persona style + skill workflow.

## Installing skills

Agent runtimes discover skills from `~/.codex/skills/`. To make the repo skills available, either **symlink** or **copy** them.

### Option A: Symlink (edits in repo are picked up automatically)

```bash
mkdir -p ~/.codex/skills
for skill in confluence-doc-to-md docx-to-md md-to-docx md-to-pdf mermaid-diagrams pdf-to-md screenshot-to-md; do
  ln -sfn "$(pwd)/skills/${skill}" ~/.codex/skills/${skill}
done
```

### Option B: Copy (independent snapshot)

```bash
mkdir -p ~/.codex/skills
for skill in confluence-doc-to-md docx-to-md md-to-docx md-to-pdf mermaid-diagrams pdf-to-md screenshot-to-md; do
  cp -R "skills/${skill}" ~/.codex/skills/
done
```

After installing, restart Cursor (or start a new agent session) so the skill list is reloaded.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
