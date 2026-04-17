---
name: comms-proofreader
description: Evaluate and refine messages for different audiences (International, US, India, Eastern Europe) with grammar scoring, cultural fit analysis, and audience-tailored rewrites. Triggered by `/pr` commands.
---

# Communication Proofreader & Refiner

## When to use

Triggered explicitly by one of the `/pr` commands:

- **`/pr {MESSAGE}`** — Full evaluation with all four audience options
- **`/pr-int {MESSAGE}`** — International audience only
- **`/pr-us {MESSAGE}`** — American audience (West Coast) only
- **`/pr-india {MESSAGE}`** — Indian audience only
- **`/pr-ee {MESSAGE}`** — Eastern European audience only

The text after the command **is** the message to evaluate and refine.

This skill can be invoked from any persona or with no persona active. Only process inputs that begin with one of the above commands.

## Execution Flow

### Step 1: Evaluate the Message

Rate the message from 1 to 10 in these categories:

- English grammar
- Spelling and typos
- Ease of understanding
- Conciseness
- Actionability

For each category, provide a brief teacher-style summary pointing out mistakes or areas for improvement.

Provide an overall rating and describe the cultural fit of the message, specifying the audience for whom it is best suited.

### Step 2: Propose Refined Message Options

Generate refined message options exclusively in English. Each should be positive, concise, easy to understand, and actionable. Use Slack emoticons appropriately.

**If the command is `/pr`:**
Generate **all four** refined message options:
1. For an International audience
2. For an American audience (West Coast)
3. For an Indian audience
4. For an Eastern European audience

**If the command is `/pr-int`, `/pr-us`, `/pr-india`, or `/pr-ee`:**
Generate **only one** refined message option for the specified audience.

## Audience Style Guidelines

- **International**: Neutral, clear, universally understood English; avoid idioms and cultural references
- **American (West Coast)**: Casual yet professional, optimistic, collaborative, solution-oriented
- **Indian**: Respectful, detailed, professional with warmth, clear action items
- **Eastern European**: Direct, respectful, practical, with encouragement and/or a fun note; no excessive politeness

## Output Format

1. **Evaluation table** — one row per category with score and teacher-style note.
2. **Overall rating** — single score with cultural fit description.
3. **Refined option(s)** — labeled by audience, ready to copy-paste.

## Validation Checklist

- [ ] Only messages starting with a `/pr` command are processed.
- [ ] Evaluation covers all five categories with specific, actionable notes.
- [ ] Refined options match the requested audience(s).
- [ ] Refined text is positive, concise, and actionable.
- [ ] Cultural fit assessment is included in the evaluation.
