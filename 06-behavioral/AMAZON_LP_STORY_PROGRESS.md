# Amazon SDE II — Leadership Principle Story Progress

**Updated:** 2026-11-12  
**Purpose:** One source of truth for which Leadership Principles have distinct incident slots, what is approved versus merely mapped, and what remains to make the full 16-story bank interview-ready.

> Governing rule: **covered is not the same as approved/finalized.** Preserve locked incident cores. Do not reuse the same incident across different primary LPs. Variants of the same incident are allowed only within its primary LP.

## Current count

- **16 distinct incident slots are now mapped.**
- **7 current story cores are already locked/working/approved.**
- **8 additional slots are grounded in resume/current-work evidence and now have a fixed primary LP/source.**
- **1 slot — Have Backbone; Disagree and Commit — still needs one real disagreement event before it can become a truthful story.**
- The target remains **16 interview-ready incidents** for four interviews without forced story reuse.

## Fixed 16-story map

The detailed source-of-truth map is now:

06-behavioral/16_STORY_MASTER_BANK.md

| # | Primary LP | Story core | Status |
|---:|---|---|---|
| 1 | Insist on the Highest Standards | Engineering Agent independent verification | **Locked / approved** |
| 2 | Customer Obsession | QA-discovered invalid/incomplete order handling | **Locked core; wording under revision** |
| 3 | Ownership | Spec-first inventory + order-status MCP delivery | **Locked / approved core** |
| 4 | Deliver Results | Verizon holiday/iPhone promotional cart release | **Working** |
| 5 | Think Big | CRM Core JAR reusable components | **Working** |
| 6 | Dive Deep | CRM API performance investigation | **Approved for rehearsal** |
| 7 | Earn Trust | AI operations / handover dashboard | **Approved core; handover framing tightened** |
| 8 | Bias for Action | Capstone Docker/CI-CD deployment improvement | **Grounded candidate** |
| 9 | Invent and Simplify | Patient Queue severity-based workflow | **Grounded candidate** |
| 10 | Learn and Be Curious | Testcontainers/WireMock integration testing | **Grounded candidate** |
| 11 | Are Right, A Lot | CRM deduplication/matching rules | **Grounded candidate** |
| 12 | Success and Scale Bring Broad Responsibility | OAuth2/JWT/RBAC/scoped MCP tool security | **Grounded candidate** |
| 13 | Dive Deep — second incident | Cognizant distributed cart/bundle debugging with ELK/correlation IDs | **Grounded candidate** |
| 14 | Insist on the Highest Standards — second incident | Frontend/backend CRM validation quality | **Grounded candidate** |
| 15 | Ownership — second incident | Kafka-driven catalog/order synchronization | **Grounded candidate** |
| 16 | Have Backbone; Disagree and Commit | Real technical/product disagreement | **Needs one factual confirmation** |

## Story 7 revision

Story 7 now uses the stronger handover/KT rationale:

- small R&D-style team builds AI/MCP system;
- solution must be handed over to a team newer to AI/tool calling;
- docs, Jira and raw logs remain useful but still require architectural knowledge;
- lightweight project dashboard gives one operational starting point;
- existing logs remain the deep-debugging source;
- request/correlation context links first-level triage to deeper investigation;
- the troubleshooting workflow becomes part of KT/handover;
- reported result remains **45% lower operational troubleshooting time**.

Saved at:

06-behavioral/stories/07_Earn_Trust_AI_Operations_Transparency.md

Do not name CloudWatch as the historical log source unless confirmed. Current safe wording is **existing centralized application logs / operational data exposed through the backend**.

## What “done” means

- **Locked core:** incident and primary LP fixed.
- **Covered:** one or more questions practiced.
- **Approved/final:** wording and factual framing accepted for interview delivery.
- **Working:** viable real incident drafted, but not fully drilled.
- **Grounded candidate:** body of work and result are real/grounded; exact STAR trigger/decision/verification still needs reconstruction.
- **Needs factual confirmation:** required historical event does not yet exist in the evidence; do not invent it.

## Remaining build sequence

1. Story 8 — Bias for Action: capstone CI/CD.
2. Story 9 — Invent and Simplify: Patient Queue.
3. Story 10 — Learn and Be Curious: integration testing.
4. Story 11 — Are Right, A Lot: CRM deduplication.
5. Story 12 — Broad Responsibility: AI commerce security.
6. Story 13 — second Dive Deep: Cognizant distributed cart debugging.
7. Story 14 — second Highest Standards: frontend/backend validation.
8. Story 15 — second Ownership: Kafka synchronization.
9. Story 16 — Have Backbone after a real disagreement is confirmed.

## One unresolved factual gap

For Story 16, we need one real event where the candidate disagreed with a lead/client/teammate over a technical or product decision, explained the evidence/trade-off, and then either changed the outcome or fully committed to the final decision.

The old async-vs-sync CRM story is fictional and remains excluded as employment history.

## Locked facts to preserve

### Customer Obsession

- QA rate-limit/prompt-injection testing exposed invalid or incomplete order-input handling.
- Compared valid and invalid requests.
- Traced Java/Spring Boot backend flow.
- Corrected affected backend handling and nearby related validation.
- Re-tested with QA.
- No invented exact field, annotation/class, HTTP response, production rollout, or isolated 42% attribution.

### Ownership

- Inventory availability and order status MCP integrations.
- Specification-first approach was motivated by Codex/Claude Code output not always aligning with explicit requirements.
- Wrote contracts/requirements/acceptance criteria, guided agent-generated implementation, personally reviewed alignment, handled requirement changes through lead/client approval, followed testing, delivery and handover.
- No invented team-wide adoption, dramatic failure, automated spec enforcement, or productivity metric.

### Highest Standards

- Engineering Agent verification remains the primary incident.
- Execution evidence and review are the key mechanism.
- Story 1 and old Story 17 are one initiative, not two stories.

## Retrieval status

Current main was re-read before this update:

- 06-behavioral/README.md
- 06-behavioral/INTERVIEW_ANSWER_STANDARD.md
- 06-behavioral/AMAZON_SDE2_CHAT_HANDOFF.md
- 06-behavioral/Final_Story_Amazon_Leadership.md
- 06-behavioral/stories/07_Earn_Trust_AI_Operations_Transparency.md

The modules directory still contains only the Customer Obsession module; there is no Earn Trust or Have Backbone module to retrieve.

Latest direct user corrections govern conflicts with older generated story-guide material.
