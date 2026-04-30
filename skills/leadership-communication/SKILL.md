---
name: leadership-communication
description: Draft or critique high-stakes engineering leadership communication. Use for messages up to executives, down to teams, peer/stakeholder updates, escalations, hard news, change announcements, expectation setting, and requests to frame leadership communication with risks, trade-offs, and likely reactions.
---

# Leadership Communication

## Overview

Use this skill to shape leadership communication before polishing language. Get the framing, ask, trade-offs, and audience reaction right first; use `comms-proofreader` only afterward for copy-level refinement.

## Workflow

### Step 1: Determine Direction

Always identify direction first:

| Direction | Optimize for |
|---|---|
| Down to team | Context, specificity, honesty, cost, invitation to push back |
| Up to leadership | Decision or ask first, quantified business impact, options |
| Peer/stakeholder | Shared ownership, explicit trade-offs, next decision point |

If direction is unclear and it changes the answer, ask one short question: "Is this going up, down, or sideways?"

### Step 2: Clarify The Communication Job

Confirm or infer:

- Audience and power dynamic.
- Desired decision, behavior, or understanding.
- What is already decided vs still open.
- Concrete impact: scope, timing, customer, revenue, risk, morale, or capacity.
- Likely objection or misunderstanding.

### Step 3: Apply Direction Rules

For communication down:

- Lead with context before conclusions.
- Be specific about what changes, what stops, and what stays.
- Do not oversell. If the decision has costs, say so.
- Invite real pushback: "What will break? What am I missing?"

For communication up:

- Put the ask or decision in the first sentence.
- Translate engineering reality into business impact.
- Present at least two options with trade-offs.
- Quantify weeks, headcount, customer impact, revenue risk, or scope.
- Preempt the questions leadership will ask.
- Escalate only decisions outside the user's authority.

For peer/stakeholder communication:

- Name the shared problem and the decision needed.
- Separate facts, concerns, and asks.
- Make ownership and next steps explicit.
- Avoid implying blame unless accountability is the purpose of the message.

### Step 4: Produce Or Critique The Message

If the user provides a draft:

1. Give the verdict on framing.
2. Name what will be misunderstood.
3. Name what is missing.
4. Provide a revised version.
5. State the likely reaction.

If the user has no draft:

1. Build the message from scratch.
2. Include a subject/title if relevant.
3. Keep it concise enough to send.
4. Add optional talking points only when they help delivery.

## Output Format

- **Framing Verdict**
- **Likely Misread**
- **Revised Message**
- **Expected Reaction**
- **Follow-up Move**

For simple requests, output only the revised message plus one short note.

## Validation Checklist

- [ ] Direction is clear: up, down, or peer/stakeholder.
- [ ] Ask or decision is explicit.
- [ ] Impact is concrete and quantified when going up.
- [ ] Context, cost, and pushback are handled when going down.
- [ ] Message does not hide trade-offs or imply false certainty.
- [ ] Final text is ready to send or speak.
