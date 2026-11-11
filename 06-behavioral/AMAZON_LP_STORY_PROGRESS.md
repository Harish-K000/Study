# Amazon SDE II — Leadership Principle Story Progress

**Updated:** 2026-11-11  
**Purpose:** One source of truth for which Leadership Principles currently have a distinct story core, what is covered versus approved, and what remains to build.

> Governing rule: **covered is not the same as approved/finalized.** Preserve locked incident cores. Do not reuse the same incident across different primary LPs. Variants of the same incident are allowed only within its primary LP.

## Current count

- **7 distinct story cores currently in play**
- **3 locked/canonical repo stories:** Highest Standards, Customer Obsession, Ownership
- **2 working stories:** Deliver Results, Think Big
- **2 newly approved rehearsal stories:** Dive Deep — CRM API performance; Earn Trust — AI operations transparency
- Current target: **16 distinct interview-ready incidents** so four interviews do not force repeated story reuse.
- Therefore, from the current seven-story bank, **9 more distinct incidents** are still needed to reach 16.
- Major LPs should ideally have **two distinct incidents each**. Do not satisfy this by relabeling or reusing the same incident across LPs.

## Fixed / current primary LP mapping

| Primary LP | Story core | Status | Question progress / note |
|---|---|---|---|
| **Insist on the Highest Standards** | Engineering Agent verification workflow | **Locked core** | Primary assignment is fixed. Story 1 and old Story 17 are the same underlying initiative; never count them as two incidents. |
| **Customer Obsession** | QA-discovered invalid/incomplete order-input handling at Birlasoft | **Locked core; wording under revision** | Canonical handoff says Q1–Q6 were covered. Newer answer standard says CO Q1 is under revision. Preserve both facts: covered historically does not mean the current wording is approved. |
| **Ownership** | Specification-first inventory-availability and order-status MCP integrations | **Locked core** | Q1–Q2 covered. **Next canonical checkpoint: Ownership Q3 — “Took a project end to end.”** |
| **Deliver Results** | Verizon holiday/iPhone promotional cart release | **Working story drafted in current conversation** | Distinct Cognizant incident. Needs full Bar Raiser drill and final approval before calling it complete. |
| **Think Big** | CRM Core JAR reusable business components | **Working story drafted in current conversation** | Work-grounded Cognizant modularization. Result framing discussed; still needs final technical/follow-up drill. |
| **Dive Deep** | CRM API performance investigation | **Approved for rehearsal** | Cognizant CRM performance work; 41% response-time improvement is resume-grounded. Exact SQL/index/root-cause sequence remains a rehearsal reconstruction unless later tied to specific historical evidence. Saved at `stories/06_Dive_Deep_CRM_API_Performance.md`. |
| **Earn Trust** | AI operations transparency dashboard | **Approved for rehearsal** | Birlasoft dashboard work is resume-grounded; 45% troubleshooting-time reduction is resume-grounded. Exact before/after measurement window is not preserved in current notes. Saved at `stories/07_Earn_Trust_AI_Operations_Transparency.md`. |

## What “done” means

Use these labels consistently:

- **Locked core:** incident and primary LP are fixed; do not relabel or replace without a direct user correction.
- **Covered:** we have practiced one or more interview questions using the story.
- **Approved/final:** wording and factual framing are accepted for interview delivery.
- **Working story:** viable incident drafted, but not yet fully drilled/approved.
- **Candidate source:** real work area that may become a story only after we reconstruct a specific incident.

## Current story-bank target

The target is now **16 distinct interview-ready incidents**.

We have **7** current story cores, so **9 more** are needed.

The purpose of the larger bank is to support four interviews without forcing the same incident into multiple LPs. Major LPs should have **two distinct incidents where possible**.

## Best next gaps to fill

Before creating second stories for every already-covered LP, prioritize missing primary-LP coverage with genuine incidents:

1. **Have Backbone; Disagree and Commit** — a real technical/product disagreement with evidence, respectful challenge, decision, and full commitment afterward.
2. **Bias for Action** — a distinct time-sensitive decision under incomplete information. Do not reuse the holiday Deliver Results incident.
3. **Second Dive Deep story** — preferably a real production/debugging investigation distinct from the CRM performance incident.
4. **Learn and Be Curious / Invent and Simplify / Are Right, A Lot / Frugality** — choose from genuine incidents after the higher-priority gaps above.

## Candidate sources already present in the background material

These are **not fixed stories yet**; they are places to recover a real incident from existing experience:

- Cognizant production troubleshooting / regression or environment issue → possible **Dive Deep**
- CRM duplicate-detection work → possible **Are Right, A Lot** or **Dive Deep**, depending on the real incident
- CRM API performance work → possible **Frugality** or **Dive Deep** if the actual bottleneck and measurement can be reconstructed
- Capstone deployment/process improvement → possible **Invent and Simplify** or **Deliver Results**
- Real disagreement with lead/client/QA/dependent team → possible **Have Backbone**
- The older Cognizant API-readiness Earn Trust scenario is retired from the active bank; use it only as reference history, not as an approved story.

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

**Next story to build: Have Backbone; Disagree and Commit.**

Story 6 — Dive Deep CRM API Performance and Story 7 — Earn Trust AI Operations Transparency are approved for rehearsal and saved. Rehearsal/follow-up drilling will happen later, as requested.

Do not restart already approved/covered stories unless explicitly asked.

## Sources retrieved from current GitHub main

- `06-behavioral/README.md`
- `06-behavioral/INTERVIEW_ANSWER_STANDARD.md`
- `06-behavioral/AMAZON_SDE2_CHAT_HANDOFF.md`
- `06-behavioral/Final_Story_Amazon_Leadership.md`
- `06-behavioral/modules/01_Customer_Obsession_Complete_Study_Module.md`

Latest direct user corrections in the current conversation govern conflicts with older generated story-guide material.
