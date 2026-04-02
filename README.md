# Expert Lab

Turn long technical documents into guided AI conversations, so you stay engaged and stop losing important details.

A curated collection of personas and reusable skills for document-centered AI work. IDE-agnostic - works with Cursor, VS Code, or any agent that reads Markdown instructions.

## Quick Start (30 seconds)

1. Put a document in your workspace (for example, `Project-Plan-v3.docx`).
2. Reference it in chat:

```text
@Project-Plan-v3.docx
```

3. The agent reads `AGENTS.md`, selects a persona from `personas/personas-map.md`, activates the right conversion skill, and starts the review flow.
4. Continue naturally with prompts like `next`, `go deeper`, `skip to risks`, or `summarize this section`.

## What makes this different

- Document-first workflow instead of one-shot summaries.
- Persona routing for the right review lens (understand, verify, critique, refine).
- Skill-based file conversion so docs become clean Markdown for better reasoning.
- Works with plain Markdown instructions - no build step and no platform lock-in.

## Document Review Mode

When you reference a document without extra instructions, the agent enters review mode:

1. Divide the document into logical, self-contained parts.
2. Present the first part with a clear summary.
3. Wait for your signal (`next`, `move on`, `continue`).
4. Present the next part and repeat.
5. Offer a short recap at the end.

This keeps context across interruptions and makes long documents easier to review.

## Persona flow

Start with the default persona, then move to specialized personas when needed.

| Persona | Role | Command |
|---|---|---|
| [Tech Doc Assistant](personas/tech-doc-assistant.md) | Default: understand complex documents in progressive depth | Upload or reference a document |
| [Due Diligence Expert](personas/due-diligence-expert.md) | Verify claims, evidence, and decision risk in documents | `/dd` |
| [Communication Expert](personas/communication-expert.md) | Refine clarity and audience fit for document or message content | `/pr`, `/pr-int`, `/pr-us`, `/pr-india`, `/pr-ee` |
| [MELT Expert](personas/melt-expert.md) | Domain-specific review for telemetry and observability documents | `/melt` |
| [Agentic Observability](personas/agentic-observability.md) | Critique architecture documents for dual-agent observability design | `/o11y` |
| [**Engineering Leader**](personas/engineering-leader.md) | **Decision critic and engineering management sparring partner** | Auto-routed by topic; `/11` for 1:1 prep |

Default persona when no match is found: `tech-doc-assistant`.
Use `/none` to skip persona selection entirely.

### Engineering Leader — why this persona matters

Most management mistakes are not made because the manager is incompetent. They are made because the manager acted too fast — on incomplete data, without input from the team, or with communication that landed wrong.

The **Engineering Leader** persona exists to slow you down at the right moment:

- **Decision critic** — challenge your plan before you commit. It asks what data you have, whether you talked to the people affected, whether you are solving the real problem or a symptom, and whose problem this actually is.
- **Communication up vs. down** — framing a message to your VP and announcing a change to your team are fundamentally different tasks. This persona knows the difference and will call out when your framing is wrong for the audience.
- **1:1 and feedback prep** (`/11`) — turns raw observations into structured agendas and SBI+I feedback drafts, so you walk into the conversation prepared.
- **Survey design** — when you say "the team doesn't trust X," it will ask: how do you know? Then help you build a short anonymous survey to get real data before you act on assumptions.
- **Operational awareness** — recognizes that incidents, bugfixes, telemetry changes, and resource optimization often cluster around on-call duty, not scattered focus. Prevents you from misreading a team member's work.

It is blunt, data-driven, and will not agree with you to be polite. That is the point.

### Special persona: Prompt Builder Expert

| Persona | Role | Command |
|---|---|---|
| [Prompt Builder Expert](personas/prompt-builder-expert.md) | Create and improve prompts and persona definitions for new domains or goals | `/builder` |

This persona helps the framework evolve by generating new high-quality personas and prompt patterns.

## Skills

Skills are persona-agnostic and can be combined with any persona.

| Skill | Description |
|---|---|
| [Confluence DOC to Markdown](skills/confluence-doc-to-md/SKILL.md) | Convert Confluence-exported `.doc` (MIME/HTML) to Markdown |
| [DOCX to Markdown](skills/docx-to-md/SKILL.md) | Convert Word documents to structured Markdown |
| [PDF to Markdown](skills/pdf-to-md/SKILL.md) | Convert PDF to Markdown (text-first, OCR fallback) |
| [Screenshot to Markdown](skills/screenshot-to-md/SKILL.md) | Extract text from screenshots via OCR |
| [Markdown to PDF](skills/md-to-pdf/SKILL.md) | Export Markdown to PDF with Mermaid and LaTeX support |
| [Markdown to DOCX](skills/md-to-docx/SKILL.md) | Convert Markdown back to Word format |
| [Mermaid Diagrams](skills/mermaid-diagrams/SKILL.md) | Create architecture, flow, and sequence diagrams |
| [1:1 Planning](skills/one-on-one/SKILL.md) | Prepare structured 1:1 agendas and draft actionable feedback (`/11`) |
| [Survey Builder](skills/survey-builder/SKILL.md) | Build situation-specific feedback surveys (360, pulse, trust diagnostics) |

## Repository structure

```text
personas/               # how the assistant thinks and responds
skills/                 # how the assistant executes specific tasks
AGENTS.md               # execution rules: persona + skill selection
```

## Installing skills

Agent runtimes discover skills from `~/.codex/skills/`. To make the repo skills available, either **symlink** or **copy** them.

### Option A: Symlink (edits in repo are picked up automatically)

```bash
mkdir -p ~/.codex/skills
for skill in confluence-doc-to-md docx-to-md md-to-docx md-to-pdf mermaid-diagrams one-on-one pdf-to-md screenshot-to-md survey-builder; do
  ln -sfn "$(pwd)/skills/${skill}" ~/.codex/skills/${skill}
done
```

### Option B: Copy (independent snapshot)

```bash
mkdir -p ~/.codex/skills
for skill in confluence-doc-to-md docx-to-md md-to-docx md-to-pdf mermaid-diagrams one-on-one pdf-to-md screenshot-to-md survey-builder; do
  cp -R "skills/${skill}" ~/.codex/skills/
done
```

After installing, restart Cursor (or start a new agent session) so the skill list is reloaded.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
