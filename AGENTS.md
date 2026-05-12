# Agent Operating Guide

This project uses two reusable layers:
- **Personas** in `personas/` (how to think/respond)
- **Skills** in `skills/` (how to execute specific tasks)

## Baseline Local Context

This repository has mandatory local context in `.engineering-leader/`. Treat those files as the source of truth; do not duplicate or summarize their contents in this guide.

Before every substantive answer, read and apply these files when present and non-empty:

- `.engineering-leader/profile.md`
- `.engineering-leader/goals.md`
- `.engineering-leader/engineering-leader-context.md`

This baseline context is mandatory regardless of the selected persona. Specialist personas provide domain correctness and execution style; the `.engineering-leader/` files provide the local context.

If the files cannot be read, say so briefly and continue with the best available context. If the user explicitly asks to skip personalization context, honor that request for the turn.

## Execution Order

1. **Load baseline local context.** Read `.engineering-leader/profile.md`, `.engineering-leader/goals.md`, and `.engineering-leader/engineering-leader-context.md` when present and non-empty. Do this before deciding how to answer.
2. **Check for skill triggers and skill applicability.** Before using shell commands, web search, direct APIs, ad-hoc scripts, or other manual methods, check whether an appropriate skill exists. Inspect `skills/README.md` and, when the runtime provides one, the session's available Skills list. Treat all of these as skill triggers: slash commands (`/pr`, `/dd`, etc.), explicit "use skill" instructions, named skills, matching file types, matching task intent, or a domain/tool workflow covered by a skill. If a skill matches, load and follow the skill's `SKILL.md` before trying other methods. If an obvious skill is available but you intentionally skip it, state why briefly before proceeding.
   - Slash-command skills still have priority: if the request starts with a slash command, match it against `skills/README.md` and invoke it directly — no persona switch is needed. The skill runs with whatever persona is currently active (or none). Some skills are persona-scoped; check the skill's own definition for requirements.
3. **Determine persona.** Read `personas/personas-map.md` and match the task to the best persona. If ambiguous, ask one short clarification question. If no match, use the default persona defined in `personas/personas-map.md`. Load the selected persona file from `personas/`.
   - Apply `personas/personas-map.md` as the routing source of truth. Do not duplicate persona names, routing criteria, or persona-specific assumptions in this guide.
4. **Workspace / persona-specific context.** The selected persona file may define extra paths to read in this repository. Read those paths in addition to the baseline `.engineering-leader/` context. If a required file is missing, follow the persona's fallback (ask once, or use template paths listed there).
   - **Mandatory local context gate:** the `.engineering-leader/` files are not optional prompt flavor. Do not rely only on open tabs, recent files, conversation memory, or documents in `my-projects/`. If this context was skipped, say so briefly, read it immediately, and correct the answer before proceeding.
5. **Determine additional skills.** Check `skills/README.md` for skills that match the task intent or file type. Multiple skills may be combined.
6. **Execute** using persona style + skill workflow(s).

Explicit user instruction always overrides automatic selection (persona or skill).

## User Control Commands

Respect these instructions when provided by the user:
- "Use persona `X`" -> force persona `X`.
- "Use skills `A`, `B`" -> force listed skills.
- "Auto persona" -> choose persona from `personas/personas-map.md`.
- `/none` -> skip persona selection entirely; respond without any persona style. This does not skip the baseline `.engineering-leader/` context unless the user explicitly asks to skip personalization context.

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
