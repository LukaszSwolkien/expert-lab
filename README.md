# Expert Lab

A curated collection of expert personas and reusable skills for daily AI-assisted work.

## Purpose

Expert Lab helps turn generic AI chats into focused, high-signal workflows by combining:
- **Personas** (`personas/`) - how the assistant should think and respond for a given task type
- **Skills** (`skills/`) - how the assistant should execute specific operational workflows

The routing logic and execution rules are defined in `AGENTS.md` and `personas/personas-map.md`.

## Personas

- [MELT Expert](personas/melt-expert.md) - OpenTelemetry and Splunk/AppDynamics instrumentation support.
- [Agentic Observability Architect](personas/agentic-observability.md) - Architecture brainstorming and feasibility reviews for dual-agent observability systems.
- [Due Diligence Expert](personas/due-diligence-expert.md) - Job-offer and recruiter credibility verification using evidence-based analysis.
- [Communication Expert](personas/communication-expert.md) - Proofreading and audience-tailored communication refinement.
- [Prompt Builder Expert](personas/prompt-builder-expert.md) - Adaptive prompt creation and optimization workflow.
- [Tech Doc Assistant](personas/tech-doc-assistant.md) - Document-first technical educator with progressive depth and knowledge gap detection.

## Skills

- [DOCX to Markdown](skills/docx-to-md/SKILL.md)
- [Markdown to DOCX](skills/md-to-docx/SKILL.md)
- [Markdown to PDF](skills/md-to-pdf/SKILL.md)
- [PDF to Markdown](skills/pdf-to-md/SKILL.md)
- [Screenshot to Markdown](skills/screenshot-to-md/SKILL.md)

## How to use

1. Start in auto-routing mode using `personas/personas-map.md`.
2. Override explicitly when needed (`Use persona X`, `Use skills A, B`).
3. Let the agent follow `AGENTS.md` execution order:
   - select persona
   - select relevant skills
   - execute with persona style + skill workflow

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
