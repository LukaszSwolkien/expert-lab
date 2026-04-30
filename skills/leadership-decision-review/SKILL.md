---
name: leadership-decision-review
description: Pressure-test engineering leadership decisions before action. Use for decision critique, delivery/process changes, technical strategy trade-offs, transformation proposals, people/performance signal review, or requests to challenge a management plan, identify risks, validate assumptions, or recommend next steps.
---

# Leadership Decision Review

## Overview

Use this skill to turn a leadership idea into a sharper decision. Challenge the diagnosis first when the problem is unclear; challenge the plan when the problem is clear.

## Workflow

### Step 1: Classify The Decision

Identify the decision type:

| Type | Focus |
|---|---|
| Delivery/process | Cycle time, quality, dependencies, planning, ownership |
| People/performance | Evidence, direct conversations, role expectations, fairness |
| Technical strategy | Business trade-offs, capacity, operating model, sequencing |
| Transformation | Real pain vs trend-chasing, adoption cost, measurable outcome |
| General decision | Assumptions, risks, alternatives, reversibility |

If the user is asking mainly about team topology, reorgs, ownership boundaries, or capacity model design, use `org-design-review` instead.

### Step 2: Run The Diagnostic Check

Before critiquing the plan, verify the problem:

- **Evidence:** What data, incidents, trends, or examples support the problem?
- **Voice:** Have the affected people been asked directly?
- **Cause:** Is this the root problem or a symptom?
- **Ownership:** Is this actually the user's problem to solve?

If the answers are missing, do not endorse action. State the evidence gap and recommend the smallest useful evidence loop: 1:1 conversations, team discussion, metrics review, or a targeted survey when feedback needs scale, anonymity, or segmentation (`survey-builder` skill).

### Step 3: Challenge The Plan

Pressure-test the decision:

1. **Restate bluntly** - Rephrase the real problem and strip away comforting framing.
2. **Attack assumptions** - Name what the user is taking for granted and why it may be wrong.
3. **Poke holes** - Test failure mode, ownership, incentives, team impact, capacity, and decision authority.
4. **Surface a harder alternative** - Offer at least one option the user may have dismissed too quickly.
5. **Name the real risks** - Flag the 1-3 risks that are politically inconvenient, culturally sensitive, or organizationally uncomfortable.
6. **Block weak action** - If evidence is weak, recommend evidence collection before execution.

### Step 4: Apply People-Signal Guardrails

When reviewing a person's behavior, performance, or activity trail:

- Separate facts from interpretation.
- Ask whether the user has spoken directly with the person.
- Avoid personality labels; use observable behaviors and impact.
- Treat operational work carefully: incidents, bugfixes, telemetry, cleanup, and resource tuning often form one chain. Ask whether the person was on-call, responding to an incident, or covering an ownership gap before judging focus or performance.

Use `one-on-one` when the next step is a 1:1 agenda or feedback draft. Use `survey-builder` when the user needs broader evidence before acting.

### Step 5: Recommend When Ready

Once the problem is clear enough:

1. Give a clear recommendation.
2. Explain the rationale in practical engineering-management terms.
3. Provide 3-5 sequenced action steps.
4. List 1-3 watch-outs to monitor after implementation.

## Output Format

Use concise Markdown sections:

- **Verdict**
- **Blunt Restatement**
- **Weak Assumptions**
- **Risks**
- **Harder Alternative**
- **Recommendation** (only when ready)
- **Next Steps**

For quick gut checks, compress to 5-10 lines. For high-stakes decisions, use the full structure.

## Validation Checklist

- [ ] Problem diagnosis was checked before plan critique.
- [ ] Evidence gaps were named plainly.
- [ ] At least one harder alternative was offered.
- [ ] Real risks and trade-offs were stated.
- [ ] People/performance claims used observable evidence.
- [ ] Recommendation, if given, included actionable next steps.
