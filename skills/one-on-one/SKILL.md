---
name: one-on-one
description: Prepare structured 1:1 agendas and deliver actionable feedback from raw input (observations, performance notes, topics). Triggered by the `/11` command in the engineering-leader persona.
---

# 1:1 Planning & Feedback

## When to use

Triggered explicitly by `/11 {CONTEXT}` within the `engineering-leader` persona.

Typical inputs:
- Raw observations about a team member's recent work or behavior
- Performance notes or review cycle input
- A list of topics the user wants to cover in an upcoming 1:1
- A situation the user needs to address with a direct report

## Execution Flow

### Step 1: Classify the Intent

Determine what the user needs from the input:

| Intent | Signal in input |
|---|---|
| **Agenda prep** | Mentions upcoming 1:1, lists topics, asks "what should I cover" |
| **Feedback drafting** | Describes a situation, behavior, or pattern they want to address |
| **Both** | Mixed input — plan the meeting and draft the feedback |

If intent is unclear, ask one short question: "Are you preparing for a 1:1, drafting feedback to deliver, or both?"

### Step 2: Agenda Prep (if applicable)

Produce a ready-to-use 1:1 agenda structured as:

1. **Check-in** (2 min) — one suggested opening question based on context (not generic "how are you?")
2. **Their topics** — placeholder for the direct report's items (always first)
3. **Your topics** — organized from the user's input, each with:
   - Topic name
   - What you want to learn or decide
   - One concrete question to ask
4. **Development / growth** (5 min) — one forward-looking question tied to the person's trajectory
5. **Actions & follow-ups** — empty section to fill during the meeting

Keep the agenda to one screen. If the user listed more than 5 topics, flag that it is too many for one 1:1 and recommend prioritization.

### Step 3: Feedback Drafting (if applicable)

Structure feedback using the **SBI+I model** (Situation → Behavior → Impact → Intent):

1. **Situation** — when and where the behavior occurred (be specific, not "lately" or "sometimes")
2. **Behavior** — what the person did or said, described factually without judgment words
3. **Impact** — the concrete effect on the team, project, or stakeholder
4. **Intent check** — a question to understand their perspective before concluding ("What was your thinking here?", "How did you see this playing out?")

Then provide:
- **Suggested opening line** — how to start the conversation naturally, without making it feel like a tribunal
- **If they get defensive** — one redirect sentence to keep the conversation productive
- **Desired outcome** — what a good resolution looks like, stated as a concrete behavioral change

### Step 4: Critical Review

Before delivering the output, review it through these lenses:

- **Is the feedback specific enough?** If it uses words like "attitude," "proactive," or "ownership" without concrete examples, rewrite it.
- **Is the agenda realistic?** If it would take more than 30 minutes, cut or defer items.
- **Is the tone right for the person?** Apply cross-cultural awareness from the persona if the direct report's cultural context is known.
- **Are you avoiding the hard part?** If the user's input hints at a serious issue but the agenda/feedback dances around it, call that out explicitly.

## Output Format

Use clear Markdown sections. For agenda: numbered items with sub-bullets. For feedback: labeled SBI+I blocks with the suggested lines in blockquotes.

## Validation Checklist

- Feedback uses observable behaviors, not personality labels.
- Agenda fits in 25-30 minutes.
- At least one question designed to listen, not to tell.
- Hard topics are addressed directly, not buried at the end.
- Output is ready to use as-is, not a template to fill in.
