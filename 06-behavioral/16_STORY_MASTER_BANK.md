# Amazon SDE II — 16-Story Master Bank

**Updated:** 2026-11-13  
**Purpose:** Fix the 16 distinct incident slots before drafting the remaining stories. One incident has one primary LP; no cross-LP reuse.

> **Hard constraint from the user:** every behavioral story must come from professional work experience only — Birlasoft or Cognizant. Academic, capstone, personal, simulated, and fictional practice incidents are excluded from the 16-story interview bank.

## Status legend

- **Locked / approved:** incident core is fixed and ready for rehearsal.
- **Working:** real work incident core exists; wording or follow-up evidence still needs finishing.
- **Grounded candidate:** resume/current notes establish the professional body of work and result, but the final STAR incident still needs a precise trigger/decision/verification sequence.
- **Needs factual confirmation:** do not invent the missing historical event.

## The 16 professional-work-only slots

| # | Primary LP | Incident / professional source | Source / measurable result | Status | Collision rule |
|---:|---|---|---|---|---|
| 1 | Insist on the Highest Standards | Engineering Agent — independent verification of AI-generated changes in Birlasoft work context | Documented Engineering Agent run/evaluation | **Locked / approved** | Same initiative as old Story 17; never count twice |
| 2 | Customer Obsession | QA-discovered invalid/incomplete order-input handling at Birlasoft | Birlasoft QA/backend work; no isolated 42% attribution | **Locked core; wording under revision** | Do not reuse as Ownership |
| 3 | Ownership | Specification-first inventory availability + order-status MCP integrations at Birlasoft | Delivered two tool-backed capabilities and handover | **Locked / approved core** | Keep coding-agent alignment motive |
| 4 | Deliver Results | Verizon holiday/iPhone promotional cart release at Cognizant | Customer-facing release | **Working** | Distinct from production-debugging stories |
| 5 | Think Big | CRM Core JAR reusable business components at Cognizant | Resume reports **33% lower coupling** | **Working** | Do not reuse as Frugality or Invent and Simplify |
| 6 | Dive Deep | CRM API performance investigation at Cognizant | Resume reports **41% lower average response time** | **Approved for rehearsal** | Performance incident stays Dive Deep |
| 7 | Earn Trust | AI operations / handover dashboard at Birlasoft | Resume reports **45% lower troubleshooting time** | **Finalized for rehearsal** | Dashboard belongs here |
| 8 | Bias for Action | Cognizant distributed cart/bundle investigation using ELK + correlation IDs | Current notes establish the distributed tracing/reproduction sequence | **Finalized safe rehearsal version** | Uses reversible investigation, not a fictional rollback |
| 9 | Invent and Simplify | Distinct professional simplification from Birlasoft AI-service or Cognizant REST/data-processing work | Resume reports **34% lower AI-channel processing time** and **27% lower backend data-processing errors** across separate work areas | **Needs factual reconstruction** | Must not reuse Story 3 MCP delivery or Story 5 Core JAR |
| 10 | Learn and Be Curious | Learning and applying Testcontainers/WireMock for realistic integration testing at Birlasoft | Resume establishes JUnit/Testcontainers/WireMock/Docker/EKS integration testing | **Grounded candidate** | Need the real first application / learning trigger |
| 11 | Are Right, A Lot | CRM deduplication / matching-rule decision at Cognizant | Resume reports **36% lower duplicate-record creation** | **Grounded candidate** | Need actual ambiguous-case decision; fictional shared-phone example is excluded |
| 12 | Success and Scale Bring Broad Responsibility | Securing AI-triggered commerce actions with OAuth2/JWT/RBAC/scoped MCP tools at Birlasoft | Resume-grounded security work | **Grounded candidate** | Need exact personally implemented security boundary; no invented breach |
| 13 | Dive Deep — second incident | A separate Cognizant production/regression/security debugging incident | Resume confirms production/regression/security debugging work | **Needs factual reconstruction** | Must be distinct from Story 8 cart/bundle incident and Story 6 performance incident |
| 14 | Insist on the Highest Standards — second incident | Frontend/backend validation quality across CRM releases at Cognizant | Resume reports **29% fewer frontend-backend validation defects** | **Grounded candidate** | Separate incident from Engineering Agent |
| 15 | Ownership — second incident | Kafka-driven catalog/order synchronization across Redis/PostgreSQL at Birlasoft | Resume-grounded pipeline ownership | **Grounded candidate** | Must be distinct from Story 3 MCP tools |
| 16 | Have Backbone; Disagree and Commit | Real work disagreement with lead/client/QA/developer | Not yet established | **Needs factual confirmation** | Do not use the fictional async-vs-sync CRM story |

## Work-experience-only rule

The following are explicitly **not eligible** for the 16-story behavioral bank:

- Patient Queue Management academic/project work
- physiotherapy/capstone CI/CD project
- simulated results
- fictional practice incidents from the old story guide
- personal projects unless the user explicitly confirms they were part of professional work

If a resume project contains a strong metric, it may still be discussed in a resume/project question, but it is **not** counted as one of the 16 work-experience behavioral incidents.

## Story 7 — latest handover core

The strongest reason for the dashboard is **operability after R&D handover**, not simply detecting a failed API.

**Core sequence:** small R&D team builds AI/MCP solution → receiving team is newer to AI/tool-calling architecture → documentation, KT, Jira and raw logs exist but still require tribal knowledge → provide one project-level operational view backed by existing application failure information → show failure area plus correlation/request identifier → use the identifier for deeper log investigation → include the workflow in KT/handover → reduce dependence on original developers.

**Architecture boundary:** the dashboard is a first-level triage surface. It does not replace Jira or deep logs.

**Data-source wording for now:** use **existing centralized application logs / operational data exposed through the backend**, without naming CloudWatch as historical fact unless confirmed.

## Build order for the remaining stories

1. Story 9 — recover one distinct professional simplification for Invent and Simplify.
2. Story 10 — Learn and Be Curious: Birlasoft integration testing.
3. Story 11 — Are Right, A Lot: Cognizant CRM deduplication.
4. Story 12 — Broad Responsibility: Birlasoft AI commerce security.
5. Story 13 — recover a second, distinct Cognizant debugging incident for Dive Deep.
6. Story 14 — second Highest Standards: Cognizant frontend/backend validation.
7. Story 15 — second Ownership: Birlasoft Kafka synchronization.
8. Story 16 — Have Backbone after a real workplace disagreement is confirmed.

## Unresolved factual questions

Two mapped slots plus Story 13 still need specific workplace reconstruction before they can be finalized:

- **Invent and Simplify:** a distinct simplification incident that is not the Core JAR or the MCP Ownership incident.
- **Have Backbone:** a real disagreement with a lead/client/QA/developer, including the competing positions and final decision.

Do not fill these with fictional scenarios.

## Metric-defense rule

For every resume metric used in the interview, prepare:

**metric definition → source → before/after basis → comparable workload/window → calculation → attribution → technical cause**

The resume itself confirms the reported professional metrics but does not, by itself, establish every baseline, window, or causal mechanism. fileciteturn181file0 fileciteturn181file1
