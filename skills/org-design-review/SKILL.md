---
name: org-design-review
description: Review engineering org design, team topology, reorg proposals, ownership boundaries, squad splits, capacity models, hiring plans, and cross-team dependency structures. Use when the user wants to pressure-test or design how teams, roles, responsibilities, and interfaces should work.
---

# Org Design Review

## Overview

Use this skill to pressure-test organization structure against the actual flow of work. Optimize for ownership clarity, delivery throughput, resilience, and transition risk.

## Workflow

### Step 1: Map The Current System

Capture or infer:

- Teams and headcount.
- Product/service ownership.
- Main delivery streams.
- Recurring dependencies and handoffs.
- Pain signals: delays, incidents, unclear ownership, morale, quality, missed commitments.
- Constraints: hiring, time zones, seniority mix, business deadlines.

If the user has no evidence that the current structure is failing, challenge that first.

### Step 2: Stress-Test The Proposed Model

Evaluate:

- **Work flow:** Does the structure match how value actually moves?
- **Ownership:** Is every critical system, customer journey, or capability owned?
- **Interfaces:** Where are handoffs unavoidable, and are they explicit?
- **Decision rights:** Who decides priorities, architecture, incidents, and trade-offs?
- **Capacity:** Does each team have enough seniority and domain knowledge?
- **Resilience:** What breaks when one key person leaves or is unavailable?
- **Migration cost:** What delivery will slow down while the new model forms?

### Step 3: Compare Options

Do not assume the user's proposed structure is the only option. Compare at least two models when stakes are meaningful:

| Option | Best for | Fails when | Hidden cost |
|---|---|---|---|
| Current model | Stability and low transition cost | Pain is caused by structure, not execution | Problems continue |
| Proposed model | User's stated goal | Ownership or capacity gaps are unresolved | Transition drag |
| Harder alternative | The problem the user may be avoiding | Political or cultural appetite is low | Higher short-term disruption |

### Step 4: Name The Real Risks

Prioritize 1-3 risks:

- New ownership gaps.
- Split-brain architecture or product decisions.
- Senior people overloaded as glue.
- Teams renamed without changing incentives.
- Morale loss from unclear rationale.
- Dependency movement rather than dependency reduction.
- Transition plan that assumes no productivity dip.

### Step 5: Recommend A Migration Path

If recommending a change, include:

1. Decision to make now.
2. Scope of the first step or pilot.
3. Ownership changes.
4. Communication plan.
5. Metrics to watch for 30-60 days.
6. Reversal or adjustment point.

Use `leadership-communication` when the next step is announcing the change. Use `management-artifact-builder` when the next step is a team charter, RACI, or decision record.

## Output Format

- **Verdict**
- **What Problem This Actually Solves**
- **Structure Stress Test**
- **Options Compared**
- **Top Risks**
- **Recommended Path**
- **30-60 Day Watch Metrics**

For quick questions, compress to verdict, risk, and next step.

## Validation Checklist

- [ ] Current pain and evidence were checked before endorsing reorg.
- [ ] Work flow and ownership boundaries were analyzed.
- [ ] At least two options or one explicit alternative were considered.
- [ ] Capacity and seniority assumptions were challenged.
- [ ] Transition risk and communication needs were named.
- [ ] Recommendation includes metrics and an adjustment point.
