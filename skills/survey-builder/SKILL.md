---
name: survey-builder
description: Build situation-specific feedback surveys (360, team pulse, trust diagnostics) with tailored questions, respondent segmentation, anonymity guidance, and interpretation instructions. Use when the user needs to collect structured feedback from a team or cross-functional stakeholders.
---

# Survey Builder

## When to use

Use this skill when the user needs to collect structured feedback before making a people or team decision. Typical triggers:
- Preparing a 360 feedback survey for an individual contributor or tech lead
- Diagnosing a trust, collaboration, or perception problem within or across teams
- Running a team pulse check before or after a change (reorg, process shift, new hire)
- The `engineering-leader` persona identifies a situation where the user is acting on assumptions about team dynamics without data

## Input Required

Before building a survey, collect or confirm from the user:

| Input | Why it matters |
|---|---|
| **Who is being evaluated?** (person, team, process) | Determines question focus and framing |
| **What is the concern or goal?** | Shapes which question categories to include |
| **Who are the respondents?** | Determines segmentation (internal team / external stakeholders / cross-functional) |
| **Anonymous or attributed?** | Affects question candor and design |
| **Will this be compared over time?** | If yes, use Likert scale questions for repeatability |

If any input is missing, ask one focused question before proceeding.

## Execution Flow

### Step 1: Determine Survey Type

| Type | When to use | Typical length |
|---|---|---|
| **360 feedback** | Evaluating an individual's collaboration, delivery, and impact as seen by peers, leads, and cross-functional partners | 7-10 scaled questions + 2-3 open-ended |
| **Team pulse** | Diagnosing team health, trust, or morale around a specific concern | 5-8 questions + 1-2 open-ended |
| **Trust diagnostic** | User suspects trust or perception issues but lacks evidence | 5-7 targeted questions + 1 safety question + 1-2 open-ended |

A single survey should not try to be all three. If the user's situation spans multiple types, recommend running them separately.

### Step 2: Design Respondent Segmentation

Consider whether the survey should segment respondents to reveal perception differences:

- **Internal vs. external** — people within the team vs. people outside (cross-team collaborators, product, stakeholders). This reveals how someone is perceived depending on the working relationship.
- **Function-based** — engineering vs. product vs. design vs. ops. Useful when cross-functional friction is suspected.
- **No segmentation** — for small teams or situations where anonymity would break with segments (fewer than 5 respondents per segment makes anonymity ineffective).

State the segmentation choice explicitly in the survey output and explain why.

### Step 3: Select and Tailor Questions

Draw from the question bank in [reference.md](reference.md), but **always tailor to the user's specific context**. Do not dump the full question bank. Select questions that directly address the concern or goal.

**Scaled questions (Likert 1-5):**
- Select 5-12 questions depending on survey type.
- Each question must measure something specific. State what it measures.
- Use the standard scale: 1 (Strongly disagree) → 5 (Strongly agree).
- Avoid double-barreled questions (asking two things in one question).
- Write questions as observable behaviors, not personality traits. "This person delivers on commitments" — not "This person is reliable."

**Open-ended questions (2-3 max):**
- At least one forward-looking question ("What could this person do differently...").
- At least one diagnostic question ("What is working well / not working well...").
- Keep them short. Long open-ended questions get skipped.

**Safety question (required for trust diagnostics and recommended for all):**
- One question that checks whether respondents feel safe answering honestly.
- Example: "I feel comfortable raising concerns about [topic] in this team."
- Place it early in the survey — not at the end where fatigue reduces honesty.

### Step 4: Write the Survey

Produce a ready-to-send survey with:

1. **Intro text** — 2-3 sentences explaining the purpose, confirming anonymity (if applicable), and setting expectations for time to complete (target: under 5 minutes).
2. **Scaled questions** — numbered, with the Likert scale defined once at the top.
3. **Open-ended questions** — clearly separated from scaled questions.
4. **Closing** — thank the respondent and state when/how results will be used (no individual attribution).

### Step 5: Interpretation Guide

After the survey, provide the user with:

- **What patterns to look for** — which scores or answer clusters matter for the specific concern.
- **Red flag thresholds** — e.g., if 3+ respondents score a question at 1-2, that is a signal worth acting on, not noise.
- **How to bring findings into a conversation** — frame results as patterns, not individual quotes. Never attribute answers to individuals even if you can guess.
- **Comparison guidance** — if this survey will be repeated, explain what delta matters (e.g., a 0.5-point shift on a 5-point scale across 8+ respondents is meaningful).

## Design Constraints

- **Total length:** 8-15 questions for 360, 5-8 for pulse/diagnostic. Longer surveys get abandoned or answered carelessly.
- **No generic questions.** Every question must connect to the user's stated concern. If it could appear in any survey regardless of context, it is too generic — rewrite or remove.
- **Anonymity is the default.** Only recommend attributed responses when the user explicitly needs them and the team is small enough that trust already exists.
- **Cultural awareness.** Engineering teams (especially in Poland and Eastern Europe) will not answer honestly if they suspect attribution. State anonymity explicitly in the survey intro. For teams with hierarchical culture (e.g., India), consider whether the manager's direct involvement in distribution affects candor — recommend a neutral party if needed.
- **No leading questions.** Do not frame questions to suggest a desired answer. "This person struggles with deadlines" is leading. "When this person commits to a deadline, I can rely on it without needing reminders" is neutral.

## Validation Checklist

- [ ] Survey addresses the user's specific concern, not generic team health.
- [ ] Questions are behavioral and observable, not personality-based.
- [ ] No double-barreled questions.
- [ ] Safety question included.
- [ ] Anonymity stated in intro.
- [ ] Total time to complete is under 5 minutes.
- [ ] Interpretation guide provided with actionable thresholds.
- [ ] Segmentation chosen and justified (or explicitly skipped with reason).
