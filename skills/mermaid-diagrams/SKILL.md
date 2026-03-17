---
name: mermaid-diagrams
description: Create clear technical diagrams using Mermaid syntax for architecture, data flow, sequence, and dependency views. Use when the user asks for a diagram, visual architecture, flowchart, sequence, or system map in Markdown.
---

# Mermaid Diagrams

## When to use

Use this skill whenever a diagram is requested or would materially improve clarity in technical documentation.

Typical triggers:
- "diagram", "architecture diagram", "flow", "sequence", "system map"
- requests to explain telemetry/data movement across components

## Default behavior

1. Prefer Mermaid diagrams in Markdown code fences:
   - `flowchart` for architecture and data flow
   - `sequenceDiagram` for request/interaction timelines
   - `stateDiagram-v2` for lifecycle/state transitions
   - `classDiagram` or `erDiagram` for structure/relationships
2. Keep diagram text concise and node labels stable.
3. Pair each diagram with 2-4 bullets that explain how to read it.
4. If the user explicitly requests another diagram format, follow that request.

## Authoring rules

- Start simple; add detail only when it changes decisions.
- Use left-to-right flow (`LR`) for pipelines unless top-down is clearer.
- Avoid crossing arrows where possible.
- Use consistent naming between diagram nodes and surrounding text.
- Do not embed credentials, secrets, or tokens in diagram examples.
- When applying custom `fill` colors via `style` or `classDef`, always include an explicit `color` that contrasts with the fill (e.g., `color:#333` on light backgrounds). This prevents unreadable bright-on-bright text when diagrams render in dark-mode environments.

## Validation checklist

- Diagram matches the written explanation.
- Direction and arrow labels represent real data/control flow.
- No orphan nodes or ambiguous abbreviations.
- Mermaid block renders in common Markdown viewers.

## Additional resources

- Patterns and copy-ready templates: [reference.md](reference.md)
