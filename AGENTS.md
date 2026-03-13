# Agent Operating Guide

This project uses two reusable layers:
- **Personas** in `personas/` (how to think/respond)
- **Skills** in `skills/` (how to execute specific tasks)

## Persona Selection (Default Behavior)

1. Start by reading `personas/personas-map.md`.
2. Match the user request to the best persona from the routing table.
3. If classification is ambiguous, ask one short clarification question.
4. If no better match is found, default to `prompt-builder-expert`.
5. Load the selected persona file from `personas/`.
6. Explicit user instruction always overrides automatic persona selection.

## Skill Selection

- Skills are persona-agnostic and can be used with any persona.
- Select skills from `skills/` based on task intent and file type.
- Multiple skills may be combined in one task when useful.

## Execution Order

1. Determine persona (`personas-map` -> persona file).
2. Determine required skills (`skills/`).
3. Execute task using persona style + skill workflow.

## User Control Commands

Respect these instructions when provided by the user:
- "Use persona `X`" -> force persona `X`.
- "Use skills `A`, `B`" -> force listed skills.
- "Auto persona" -> choose persona from `personas/personas-map.md`.
- `/none` -> skip persona selection entirely; respond without any persona style.

## File Output Placement

- Keep source documents (input files) separated from generated output files.
- When source files reside in a `docs/` or `input/` subfolder, place generated files one level up from that folder (i.e., in the parent directory).
- Never mix generated artifacts into the same directory as the source documents.

## Output Discipline

- Always declare the active execution context at the start of each response:
  - `Persona: <persona-name>` or `Persona: none`
  - `Skills: <skill-a>, <skill-b>` or `Skills: none`
- If persona/skills change during the conversation, explicitly announce the new selection in the next response.
- If automatic routing is used, state that selection came from `personas/personas-map.md`.
- Follow the selected persona's response format and constraints.
- Keep answers concise unless the user asks for depth.
- When a skill is used, follow its validation checklist before final response.
