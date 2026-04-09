# Expert Lab

A collection of AI personas and reusable skills for professional work — leadership decisions, technical review, domain expertise, and document intelligence.

Personas define how the AI thinks (decision critic, domain expert, communication coach). Skills define repeatable execution (1:1 prep, surveys, document conversion, presentations). You describe the problem; the system picks the right advisor and toolset. No prompt engineering required.

Works with Cursor, VS Code, or any agent that reads Markdown instructions.

## What it covers

- **Leadership & decision support** — stress-test plans, prep 1:1s, frame communication up and down, design surveys to replace assumptions with data.
- **Document intelligence** — section-by-section conversations that preserve nuance instead of shallow summaries; convert between formats (PDF, DOCX, Confluence, screenshots).
- **Domain expertise** — observability, telemetry, and OpenTelemetry review with a dedicated MELT persona; architecture critique for agentic systems.
- **Communication** — audience-tailored rewrites across regions and stakeholders.
- **Due diligence** — fact-checking, claim verification, and decision-risk assessment.
- **Framework evolution** — a prompt-builder persona that helps you create new personas and skills for your own domains.

## Quick Start

The quality of output depends on the quality of input. One-liner prompts get generic answers. Rich context — with specifics, data, and constraints — gets responses that are actually useful.

### Leadership sparring — meeting transcript feedback

After a meeting, feed in the transcript and ask for feedback on how you ran it:

```text
@meeting-transcript.vtt
Please share your feedback based on this meeting — what was decided,
what was avoided, and how I ran it as a leader.
```

The agent routes to `engineering-leader` and analyzes the transcript: were decisions actually made or just discussed? Did you let someone dominate? Did you avoid the hard topic? What should you do differently next time?

This also works with other rich inputs — a reorg plan with team sizes and constraints, a draft announcement to your team, or an escalation message to your VP. The more context you provide, the sharper the pushback.

### 1:1 prep with `/11`

Feed in concrete observations — sprint activity, feedback from peers, specific incidents:

```text
/11 Summary of Ryszard's last sprint:
- Closed 3 high-priority bugs in the GCP integration wizard (WIF config crash on edit)
- Finished the gRPC executor pool migration that was stuck for a year — deployed to rc0
- Investigated a lab0 incident at 11 PM on her own initiative
- I've received feedback from two team members that Ryszard sometimes dismisses their
  suggestions without explanation. This is creating friction.
I want to acknowledge the strong delivery but also address the feedback loop issue.
```

The output is a structured 1:1 agenda with SBI feedback drafts, goals for the conversation, and a challenge on whether you have enough evidence to raise the interpersonal point.

### Document conversation

Reference a document without extra instructions:

```text
@Architecture-Review-Q2.pdf
```

The agent enters review mode: divides the document into logical parts, presents each with a summary, and waits for you to go deeper, skip, or move on. Context is preserved across interruptions. You can steer with `next`, `go deeper`, `skip to risks`, or ask specific questions about the current section.

## What makes this different

- **Sparring partner, not a yes-machine.** The engineering-leader persona challenges first, agrees later — only when convinced. It will not validate your idea to be polite.
- **Persona routing.** Describe the problem; the system picks the right advisor. No prompt engineering required.
- **Document conversations over summaries.** Section-by-section dialogue that preserves nuance instead of stripping it.
- **Reusable skills.** Leadership workflows (1:1 prep, surveys, presentations) and document conversions are codified and repeatable.
- **Plain Markdown instructions.** No build step, no platform lock-in.

## Personas

Personas define how the assistant thinks and responds. The system auto-selects from `personas/personas-map.md`, or you can force one directly.

### Leadership & decision support

| Persona | Role | Command |
|---|---|---|
| [**Engineering Leader**](personas/engineering-leader.md) | Decision critic and sparring partner — challenges plans, preps 1:1s, stress-tests communication | Auto-routed by topic; `/11` for 1:1 prep |
| [Communication Expert](personas/communication-expert.md) | Audience-tailored rewrites by region and stakeholder | `/pr`, `/pr-int`, `/pr-us`, `/pr-india`, `/pr-ee` |
| [Due Diligence Expert](personas/due-diligence-expert.md) | Verify claims, evidence, and decision risk | `/dd` |

### Technical & domain expertise

| Persona | Role | Command |
|---|---|---|
| [Tech Doc Assistant](personas/tech-doc-assistant.md) | Understand complex documents in progressive depth (default persona) | Reference a document |
| [MELT Expert](personas/melt-expert.md) | Domain review for telemetry, OpenTelemetry, and observability | `/melt` |
| [Agentic Observability](personas/agentic-observability.md) | Critique architecture for dual-agent observability design | `/o11y` |

### Meta

| Persona | Role | Command |
|---|---|---|
| [Prompt Builder Expert](personas/prompt-builder-expert.md) | Create and improve prompts, persona definitions, and skill templates | `/builder` |

Default persona when no match is found: `tech-doc-assistant`.
Use `/none` to skip persona selection entirely.

### Engineering Leader — why this persona matters

Most management mistakes happen because the manager acted too fast — on incomplete data, without input from the team, or with communication that landed wrong.

The **Engineering Leader** persona exists to slow you down at the right moment:

- **Decision critic** — challenges your plan before you commit. Asks what data you have, whether you talked to the people affected, and whether you are solving the real problem or a symptom.
- **Communication up vs. down** — framing a message to your VP and announcing a change to your team are fundamentally different tasks. This persona knows the difference and calls out when your framing is wrong.
- **1:1 and feedback prep** (`/11`) — turns raw observations into structured agendas and SBI+I feedback drafts.
- **Survey design** — when you say "the team doesn't trust X," it asks: how do you know? Then helps you build a survey to get real data before you act on assumptions.
- **Operational awareness** — recognizes that incidents, bugfixes, and telemetry work cluster around on-call duty, preventing you from misreading a team member's contributions.

It is blunt, data-driven, and will not agree with you to be polite. That is the point.

## Skills

Skills are persona-agnostic and can be combined with any persona.

### Leadership workflows

| Skill | Description |
|---|---|
| [1:1 Planning](skills/one-on-one/SKILL.md) | Prepare structured 1:1 agendas and draft actionable feedback (`/11`) |
| [Survey Builder](skills/survey-builder/SKILL.md) | Build situation-specific feedback surveys (360, pulse, trust diagnostics) |
| [Marp Splunk Presentation](skills/marp-splunk-presentation/SKILL.md) | Build and export Splunk-branded slide decks (`/pdf`, `/pptx`) |

### Document conversion & diagrams

| Skill | Description |
|---|---|
| [Confluence DOC to Markdown](skills/confluence-doc-to-md/SKILL.md) | Convert Confluence-exported `.doc` (MIME/HTML) to Markdown |
| [DOCX to Markdown](skills/docx-to-md/SKILL.md) | Convert Word documents to structured Markdown |
| [PDF to Markdown](skills/pdf-to-md/SKILL.md) | Convert PDF to Markdown (text-first, OCR fallback) |
| [Screenshot to Markdown](skills/screenshot-to-md/SKILL.md) | Extract text from screenshots via OCR |
| [Markdown to PDF](skills/md-to-pdf/SKILL.md) | Export Markdown to PDF with Mermaid and LaTeX support |
| [Markdown to DOCX](skills/md-to-docx/SKILL.md) | Convert Markdown back to Word format |
| [Mermaid Diagrams](skills/mermaid-diagrams/SKILL.md) | Create architecture, flow, and sequence diagrams |

## Document Review Mode

When you reference a document without extra instructions, the agent enters review mode:

1. Divide the document into logical, self-contained parts.
2. Present the first part with a clear summary.
3. Wait for your signal (`next`, `move on`, `continue`).
4. Present the next part and repeat.
5. Offer a short recap at the end.

This keeps context across interruptions and makes long documents manageable.

## Repository structure

```text
personas/               # how the assistant thinks and responds
  personas-map.md       # routing table for automatic persona selection
skills/                 # how the assistant executes specific tasks
AGENTS.md               # execution rules: persona + skill selection
```

## Installing skills

Agent runtimes discover skills from `~/.codex/skills/`. To make the repo skills available, either **symlink** or **copy** them.

### Option A: Symlink (edits in repo are picked up automatically)

```bash
mkdir -p ~/.codex/skills
for skill in confluence-doc-to-md docx-to-md md-to-docx md-to-pdf marp-splunk-presentation mermaid-diagrams one-on-one pdf-to-md screenshot-to-md survey-builder; do
  ln -sfn "$(pwd)/skills/${skill}" ~/.codex/skills/${skill}
done
```

### Option B: Copy (independent snapshot)

```bash
mkdir -p ~/.codex/skills
for skill in confluence-doc-to-md docx-to-md md-to-docx md-to-pdf marp-splunk-presentation mermaid-diagrams one-on-one pdf-to-md screenshot-to-md survey-builder; do
  cp -R "skills/${skill}" ~/.codex/skills/
done
```

After installing, restart Cursor (or start a new agent session) so the skill list is reloaded.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
