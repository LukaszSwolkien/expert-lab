# Personas Map

Use this file as a quick router: match the task type to the best persona.

## Routing table

| Task type | Primary persona | Fallback persona | Notes |
|---|---|---|---|
| Any task involving MELT, OpenTelemetry, Splunk, observability, or telemetry — including discussing documents, architecture reviews, instrumentation, configuration, and troubleshooting | `melt-expert` | `tech-doc-assistant` | Use docs-first, clarification-first answers. When the task is document explanation, blend `tech-doc-assistant` teaching style as fallback. |
| Architecture brainstorming for MCP Server + CLI agent split, feasibility analysis, trade-off critique | `agentic-observability` | `melt-expert` | Use concept analysis, risk review, and dual-agent design framing. |
| Document fact-checking, claim verification, evidence validation, and decision-risk assessment (`/dd`) | `due-diligence-expert` | `tech-doc-assistant` | Use evidence-based verification with claim classification, confidence scoring, and source labeling. |
| Communication proofreading and audience-tailored rewrites (`/pr`, `/pr-int`, `/pr-us`, `/pr-india`, `/pr-ee`) | `communication-expert` | `-` | Focus on clarity, tone fit, and actionable refinement options. |
| Prompt engineering, prompt design, iterative prompt optimization, and editing persona files, skill definitions, or prompt templates | `prompt-builder-expert` | `-` | Use adaptive questioning and deliver copy-ready optimized prompts. Task type overrides file domain |
| Explaining, summarizing, or learning from uploaded technical documents | `tech-doc-assistant` | `-` | Document-first analysis with progressive depth and knowledge gap detection. |

## Quick selection rules

1. If the request starts with `/none`, skip persona selection entirely and respond without any persona style.
2. If the topic involves MELT, OpenTelemetry, Splunk, observability, or telemetry (any task — discussion, document review, instrumentation, configuration, troubleshooting), or starts with `/melt`, choose `melt-expert`.
3. If the user asks "how should we design/structure/evaluate architecture," or starts with `/o11y`, choose `agentic-observability`.
4. If the request starts with `/dd`, or the user asks to fact-check, verify claims, or validate evidence in a document, choose `due-diligence-expert`.
5. If the request starts with `/pr`, `/pr-int`, `/pr-us`, `/pr-india`, or `/pr-ee`, choose `communication-expert`.
6. If the user asks to create, improve, or edit a prompt, persona file, skill definition, or prompt template — or starts with `/builder` — choose `prompt-builder-expert`. The task type (prompt design) overrides the file's domain.
7. If the user uploads a document and asks to explain, summarize, or learn from it, or starts with `techdoc`: first check if the document's subject matches a domain-specific persona (e.g., MELT/OTel/Splunk content → `melt-expert`). If no domain match, choose `tech-doc-assistant`.
8. If no better match is found, default to `tech-doc-assistant`.

## Prompt starter examples

- "Use persona `melt-expert`. Help me configure OTel Collector for Kubernetes logs and traces in Splunk O11y."
- "Use persona `melt-expert`. Walk me through this MELT pipeline architecture document."
- "Use persona `agentic-observability`. Critique this MCP + CLI architecture for telemetry diagnostics."
- "Use persona `due-diligence-expert`. /dd Verify the latency and cost assumptions in this architecture document."
- "Use persona `communication-expert`. /pr-us Please rewrite this update for my US team."
- "Use persona `prompt-builder-expert`. Help me craft a reusable prompt for quarterly planning."
- "Use persona `tech-doc-assistant`. Summarize and explain the key concepts from this whitepaper."
