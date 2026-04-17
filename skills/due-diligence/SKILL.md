---
name: due-diligence
description: Verify factual claims in technical documents with evidence-based analysis, confidence scoring, and decision-risk guidance. Triggered by the `/dd` command.
---

# Due Diligence — Claim Verification

## When to use

Triggered explicitly by `/dd {SCOPE}` where SCOPE describes what to verify — specific sections, claims, assumptions, or the entire document.

This skill can be invoked from any persona or with no persona active. Only process inputs that begin with the `/dd` command.

Typical inputs:
- A referenced document with a request to verify its factual claims
- Specific sections or assertions the user wants fact-checked
- Assumptions or projections that need evidence validation before a decision

## Execution Flow

### Step 1: Scope and Claim Extraction

- Identify the document (or section) under review and its domain.
- Extract all substantive claims — statements presented as facts, metrics, projections, or design rationale.
- Classify each claim into one of four types:

| Type | Definition | Example |
|---|---|---|
| **Factual** | Verifiable statement about the current or past state of affairs | "Kafka supports exactly-once semantics since v0.11" |
| **Assumption** | Premise accepted without proof within the document | "We assume peak traffic will not exceed 10k RPS" |
| **Interpretation** | Author's reasoning or inference drawn from other data | "This architecture reduces latency because fewer hops are involved" |
| **Recommendation** | Proposed action or design choice | "We recommend using Flink over Kafka Streams for this workload" |

### Step 2: Evidence Collection

For each extracted claim, gather supporting or contradicting evidence using a two-tier approach:

1. **Document-internal evidence** (primary): other sections of the same document, referenced appendices, cited sources within the document.
2. **External authoritative sources** (secondary): official documentation, RFCs, vendor specs, peer-reviewed papers, reputable technical media. Avoid low-credibility sources.

Cross-verify critical facts across at least two credible sources when possible. Capture exact URLs or document section references for every piece of evidence used.

### Step 3: Verification Matrix

Build a verification table for the extracted claims:

| # | Claim Summary | Type | Status | Confidence | Evidence / Citation |
|---|---|---|---|---|---|
| 1 | [Concise claim] | Factual / Assumption / Interpretation / Recommendation | Verified / Partially verified / Unverified / Conflicting evidence | High / Medium / Low | [Source: document section or external link] |

**Status definitions:**

- **Verified** — supported by at least two independent credible sources or unambiguous document-internal evidence.
- **Partially verified** — some supporting evidence exists but coverage is incomplete or a single source only.
- **Unverified** — no evidence found to confirm or deny; the claim stands unsupported.
- **Conflicting evidence** — credible sources contradict the claim or each other.

### Step 4: Gap and Risk Analysis

After completing the matrix, synthesize findings into:

1. **Key risks** — claims with `Conflicting evidence` or `Unverified` status that could affect decisions.
2. **Stale assumptions** — assumptions that may have been valid at writing time but are outdated given current information.
3. **Missing evidence** — areas where the document makes consequential claims without citing sources.
4. **Decision impact** — which unverified or conflicting claims carry the highest stakes for the decision at hand.

### Step 5: Risk Score and Guidance

- Provide a document confidence score (1–10; 1 = low confidence / many unverified claims, 10 = high confidence / well-evidenced).
- Provide a one-line rationale for the score.
- List 2–5 concrete "verify next" actions the reader should take before relying on the document's conclusions.

## Output Format

- Present the verification matrix as a Markdown table.
- Follow with key risks, stale assumptions, and missing evidence as concise bullet lists.
- End with the confidence score, rationale, and "verify next" actions.
- Each fact or counter-evidence must include a source citation (inline Markdown link or document section reference). If information cannot be verified, explicitly state "Unable to verify".
- Clearly label every piece of evidence as `[Document]` (from the analyzed document) or `[External]` (from outside sources).

## Tone and Style

- Neutral, investigative, and evidence-based.
- Precise and structured; avoid promotional or speculative wording.
- Prioritize verifiable and authoritative sources (official documentation, specs, standards, reputable technical media).
- Distinguish clearly between what the document states and what external evidence shows.
- Avoid speculation, assumptions, or fabricated details.

## Validation Checklist

- [ ] Only inputs starting with `/dd` are processed.
- [ ] All substantive claims are extracted and classified.
- [ ] Evidence is labeled as `[Document]` or `[External]`.
- [ ] Verification matrix is complete with status and confidence for each claim.
- [ ] Gap and risk analysis covers key risks, stale assumptions, and missing evidence.
- [ ] Confidence score with rationale and "verify next" actions are provided.
- [ ] No speculation or fabricated evidence.
