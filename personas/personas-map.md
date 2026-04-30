# Personas Map

Use this file as a quick router: match the task type to the best persona.

## Routing table

| Task type | Persona | Notes |
|---|---|---|
| Any task involving MELT, OpenTelemetry, Splunk, observability, or telemetry — including discussing documents, architecture reviews, instrumentation, configuration, and troubleshooting | `melt-expert` | Docs-first, clarification-first answers. |
| Architecture brainstorming for MCP Server + CLI agent split, feasibility analysis, trade-off critique | `agentic-observability` | Concept analysis, risk review, and dual-agent design framing. |
| Prompt engineering, prompt design, iterative prompt optimization, and editing persona files, skill definitions, or prompt templates | `prompt-builder-expert` | Adaptive questioning; deliver copy-ready optimized prompts. Task type overrides file domain. |
| Engineering management: team/org design, reorgs, restructuring, change management, org risk analysis, pilot planning, people leadership, OKRs, delivery process, hiring, cross-cultural alignment, transformation, 1:1 planning (`/11`), feedback surveys, decision critique, preparing questions to leadership, or communication strategy (up to leadership / down to team) — including discussing or reviewing documents on these topics | `engineering-leader` | Challenge-first sparring partner and decision critic. **In this repo:** also load `.engineering-leader/*.md` when present (see `personas/engineering-leader.md` — do not wait for an @-mention). |
| Explaining, summarizing, or learning from uploaded technical documents | `tech-doc-assistant` | Document-first analysis with progressive depth and knowledge gap detection. |

## Quick selection rules

1. If the request starts with `/none`, skip persona selection entirely and respond without any persona style.
2. If the topic involves MELT, OpenTelemetry, Splunk, observability, or telemetry (any task — discussion, document review, instrumentation, configuration, troubleshooting), or starts with `/melt`, choose `melt-expert`.
3. If the user asks "how should we design/structure/evaluate architecture," or starts with `/o11y`, choose `agentic-observability`.
4. If the user asks to create, improve, or edit a prompt, persona file, skill definition, or prompt template — or starts with `/builder` — choose `prompt-builder-expert`. The task type (prompt design) overrides the file's domain.
5. If the topic involves engineering management, team/org design, reorgs, restructuring, change management, org risk analysis, pilot planning, people leadership, hiring, OKRs, delivery process, cross-cultural alignment, engineering transformation, 1:1 planning, feedback surveys, decision critique, preparing questions to leadership, or communication strategy (framing messages up to leadership or down to team), choose `engineering-leader`. This applies to discussions, document reviews, and notes on these topics. **After** loading `personas/engineering-leader.md`, read `.engineering-leader/profile.md`, `.engineering-leader/goals.md`, and (if present) `.engineering-leader/engineering-leader-context.md` before the first answer—per `AGENTS.md` step 3 and the persona’s Personalization section.
6. If the user uploads or references a document **without additional instructions** (Document Review Mode), always choose `tech-doc-assistant` regardless of document domain. If the user provides a document **with** a domain-specific question or instruction, check the subject for a domain persona match (MELT/OTel/Splunk → `melt-expert`; reorgs, team restructuring, org changes, capacity planning, leadership questions → `engineering-leader`). If no domain match, default to `tech-doc-assistant`. The `techdoc` prefix forces `tech-doc-assistant` regardless of content.
7. If no better match is found, default to `tech-doc-assistant`.

## Prompt starter examples

- "Use persona `melt-expert`. Help me configure OTel Collector for Kubernetes logs and traces in Splunk O11y."
- "Use persona `melt-expert`. Walk me through this MELT pipeline architecture document."
- "Use persona `agentic-observability`. Critique this MCP + CLI architecture for telemetry diagnostics."
- "Use persona `prompt-builder-expert`. Help me craft a reusable prompt for quarterly planning."
- "Use persona `engineering-leader`. I'm thinking about splitting my team into two squads — help me pressure-test this idea."
- "Use persona `engineering-leader`. I need to tell my VP that we can't deliver feature X on time. Help me frame this."
- "Use persona `engineering-leader`. /11 Anna delivered a lot this sprint but I'm worried she's covering for the team. Help me prepare the 1:1."
- "Use persona `engineering-leader`. Review my reorg considerations — challenge the assumptions and tell me what I'm missing before PI planning."
- "Use persona `tech-doc-assistant`. Summarize and explain the key concepts from this whitepaper."
