# Agent Operating Guide

This project uses two reusable layers:
- **Personas** in `personas/` (how to think/respond)
- **Skills** in `skills/` (how to execute specific tasks)

## Persona Selection (Opt-In)

Persona selection is **off by default**. Do NOT automatically read `personas/personas-map.md` or apply any persona unless the user explicitly activates one using the commands listed in "User Control Commands" below.

When persona selection IS activated:
1. Read `personas/personas-map.md`.
2. Match the user request to the best persona from the routing table.
3. If classification is ambiguous, ask one short clarification question.
4. If no better match is found, default to `tech-doc-assistant`.
5. Load the selected persona file from `personas/`.
6. Explicit user instruction always overrides automatic persona selection.

## Skill Selection

- Skills are persona-agnostic and can be used with any persona.
- Select skills from `skills/` based on task intent and file type.
- Multiple skills may be combined in one task when useful.

## Execution Order

1. Check if persona selection was activated by the user (see "User Control Commands"). If not, skip persona entirely and respond normally.
2. If activated, determine persona (`personas-map` -> persona file).
3. Determine required skills (`skills/`).
4. Execute task using persona style (if active) + skill workflow.

## User Control Commands

Respect these instructions when provided by the user:
- "Use persona `X`" or `/persona X` -> force persona `X`.
- `/persona` (without a name) or "Auto persona" -> auto-select persona from `personas/personas-map.md`.
- "Use skills `A`, `B`" -> force listed skills.
- `/none` -> explicitly disable persona for this message (even if one was active earlier).

## File Output Placement

- Keep source documents (input files) separated from generated output files.
- When source files reside in a `docs/` or `input/` subfolder, place generated files one level up from that folder (i.e., in the parent directory).
- Never mix generated artifacts into the same directory as the source documents.

## Document Review Mode

When the user references a document without additional instructions, treat it as a request to enter Document Review Mode:

1. Divide the document into logical, self-contained parts (sections or groups of related sections).
2. Present the first part with a clear summary and invite the user to discuss it.
3. Wait for the user to signal they are done with the current part (e.g., "next", "move on", "continue").
4. Then present the next part and repeat until the entire document has been covered.
5. At the end, offer a brief recap of all parts if useful.

## Output Discipline

- Only declare execution context when a persona or skill is actively in use:
  - `Persona: <persona-name>` and/or `Skills: <skill-a>, <skill-b>`
- If no persona is active, do NOT add a `Persona:` header -- just respond normally.
- If persona/skills change during the conversation, explicitly announce the new selection in the next response.
- If automatic routing is used, state that selection came from `personas/personas-map.md`.
- Follow the selected persona's response format and constraints.
- Keep answers concise unless the user asks for depth.
- When a skill is used, follow its validation checklist before final response.
