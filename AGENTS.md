# Agent Operating Guide

This project uses two reusable layers:
- **Personas** in `personas/` (how to think/respond)
- **Skills** in `skills/` (how to execute specific tasks)

## Execution Order

1. **Check for skill triggers first.** If the request starts with a slash command (`/pr`, `/dd`, etc.), match it against `skills/README.md`. If a skill matches, invoke it directly — no persona switch is needed. The skill runs with whatever persona is currently active (or none). Note: some skills are persona-scoped (e.g., `/11` requires `engineering-leader`) — check the skill's own definition for requirements.
2. **Determine persona.** Read `personas/personas-map.md` and match the task to the best persona. If ambiguous, ask one short clarification question. If no match, default to `tech-doc-assistant`. Load the selected persona file from `personas/`.
3. **Workspace / personalization context (when the persona requires it).** The persona file may define extra paths to read in this repository (e.g. **Personalization Sources** in `personas/engineering-leader.md` → read `.engineering-leader/*.md` when present). **Do this before the first substantive answer** for that request—do not wait for the user to @-mention those folders. If a file is missing, follow the persona’s fallback (ask once, or use template paths listed there).
4. **Determine additional skills.** Check `skills/README.md` for skills that match the task intent or file type. Multiple skills may be combined.
5. **Execute** using persona style + skill workflow(s).

Explicit user instruction always overrides automatic selection (persona or skill).

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
