# Amazon SDE II — Leadership Principle Story Progress

**Updated:** 2026-11-11  
**Purpose:** One source of truth for which Leadership Principles currently have a distinct story core, what is covered versus approved, and what remains to build.

> Governing rule: **covered is not the same as approved/finalized.** Preserve locked incident cores. Do not reuse the same incident across different primary LPs. Variants of the same incident are allowed only within its primary LP.

## Current count

- **5 distinct story cores currently in play**
- **3 locked/canonical repo stories:** Highest Standards, Customer Obsession, Ownership
- **2 newer working stories from the current interview-prep conversation:** Deliver Results, Think Big
- Practical story-bank target from earlier prep guidance: **8–10 strong distinct incidents**
- Therefore, to reach that practical bank: **3–5 more distinct incidents**
- If we apply a strict rule of **2 distinct stories for each of the 5 currently fixed LPs**, we need **5 additional second stories** for those LPs.
- If more LPs are later designated “major,” add two distinct incidents for each newly designated major LP, less any already-fixed story for that LP.

## Fixed / current primary LP mapping

| Primary LP | Story core | Status | Question progress / note |
|---|---|---|---|
| **Insist on the Highest Standards** | Engineering Agent verification workflow | **Locked core** | Primary assignment is fixed. Story 1 and old Story 17 are the same underlying initiative; never count them as two incidents. |
| **Customer Obsession** | QA-discovered invalid/incomplete order-input handling at Birlasoft | **Locked core; wording under revision** | Canonical handoff says Q1–Q6 were covered. Newer answer standard says CO Q1 is under revision. Preserve both facts: covered historically does not mean the current wording is approved. |
| **Ownership** | Specification-first inventory-availability and order-status MCP integrations | **Locked core** | Q1–Q2 covered. **Next canonical checkpoint: Ownership Q3 — “Took a project end to end.”** |
| **Deliver Results** | Verizon holiday/iPhone promotional cart release | **Working story drafted in current conversation** | Distinct Cognizant incident. Needs full Bar Raiser drill and final approval before calling it complete. |
| **Think Big** | CRM Core JAR reusable business components | **Working story drafted in current conversation** | Work-grounded Cognizant modularization. Result framing discussed; still needs final technical/follow-up drill. |

## What “done” means

Use these labels consistently:

- **Locked core:** incident and primary LP are fixed; do not relabel or replace without a direct user correction.
- **Covered:** we have practiced one or more interview questions using the story.
- **Approved/final:** wording and factual framing are accepted for interview delivery.
- **Working story:** viable incident drafted, but not yet fully drilled/approved.
- **Candidate source:** real work area that may become a story only after we reconstruct a specific incident.

## Current practical story-bank math

### Option A — efficient 8–10 story bank

We have **5** current story cores.

- To reach **8**: need **3 more**
- To reach **10**: need **5 more**

This is the most practical near-term target.

### Option B — strict two stories for every currently fixed LP

Current fixed LP set:

1. Insist on the Highest Standards
2. Customer Obsession
3. Ownership
4. Deliver Results
5. Think Big

Each currently has **1 distinct incident**, so a strict 2-per-LP rule requires **5 more distinct incidents** — one second story for each LP.

Do **not** satisfy this by reusing one incident under another LP.

## Best next gaps to fill

Before creating second stories for every already-covered LP, prioritize missing primary-LP coverage with genuine incidents:

1. **Dive Deep** — preferably a real production/debugging investigation with symptom → evidence → root cause → fix → verification.
2. **Earn Trust** — a real mistake, overcommitment, communication miss, or recovery where I owned the problem and changed my process.
3. **Have Backbone; Disagree and Commit** — a real technical/product disagreement with evidence, respectful challenge, decision, and full commitment afterward.
4. **Bias for Action** — a distinct time-sensitive decision under incomplete information. Do not reuse the holiday Deliver Results incident if we keep the no-cross-LP rule.
5. **Learn and Be Curious / Invent and Simplify / Are Right, A Lot / Frugality** — choose from genuine incidents only after the higher-priority gaps above are covered.

## Candidate sources already present in the background material

These are **not fixed stories yet**; they are places to recover a real incident from existing experience:

- Cognizant production troubleshooting / regression or environment issue → possible **Dive Deep**
- CRM duplicate-detection work → possible **Are Right, A Lot** or **Dive Deep**, depending on the real incident
- CRM API performance work → possible **Frugality** or **Dive Deep** if the actual bottleneck and measurement can be reconstructed
- Capstone deployment/process improvement → possible **Invent and Simplify** or **Deliver Results**
- Real disagreement with lead/client/QA/dependent team → possible **Have Backbone**
- Real mistake or premature readiness statement → possible **Earn Trust**

No candidate source becomes a story until the specific incident is grounded.

## Locked story facts to preserve

### Customer Obsession

- QA rate-limit/prompt-injection testing exposed invalid or incomplete order-input handling.
- Compared valid and invalid requests.
- Traced Java/Spring Boot backend flow.
- Corrected affected backend handling and nearby related validation.
- Re-tested with QA.
- No invented exact field, annotation/class, HTTP response, production rollout, or isolated 42% attribution.
- Engineering Agent verification detail is not central to the story unless a follow-up makes it relevant.

### Ownership

- Inventory availability and order status MCP integrations.
- Specification-first approach was motivated by Codex/Claude Code output not always aligning with explicit requirements.
- Wrote contracts/requirements/acceptance criteria, guided agent-generated implementation, personally reviewed alignment, handled requirement changes through lead/client approval, followed testing, delivery and handover.
- No invented team-wide adoption, dramatic failure, automated spec enforcement, or productivity metric.

### Highest Standards

- Engineering Agent verification remains the primary incident.
- Execution evidence and review are the key mechanism.
- Story 1 and Story 17 are one initiative, not two stories.

## Immediate continuation checkpoint

**Resume Ownership Q3 — “Tell me about a time you took a project end to end.”**

Do not restart Customer Obsession or Ownership Q1–Q2 unless explicitly asked.

## Sources retrieved from current GitHub main

- `06-behavioral/README.md`
- `06-behavioral/INTERVIEW_ANSWER_STANDARD.md`
- `06-behavioral/AMAZON_SDE2_CHAT_HANDOFF.md`
- `06-behavioral/Final_Story_Amazon_Leadership.md`
- `06-behavioral/modules/01_Customer_Obsession_Complete_Study_Module.md`

Latest direct user corrections in the current conversation govern conflicts with older generated story-guide material.
