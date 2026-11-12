# Amazon SDE II — 16-Story Master Bank

**Updated:** 2026-11-12  
**Purpose:** Fix the 16 distinct incident slots before drafting the remaining stories. One incident has one primary LP; no cross-LP reuse.

## Status legend

- **Locked / approved:** incident core is fixed and ready for rehearsal.
- **Working:** real incident core exists; wording or follow-up evidence still needs finishing.
- **Grounded candidate:** resume/current notes establish the body of work and result, but the final STAR incident still needs a precise trigger/decision/verification sequence.
- **Needs one factual confirmation:** do not invent the missing historical event.

## The 16 distinct incident slots

| # | Primary LP | Incident | Source / measurable result | Status | Collision rule |
|---:|---|---|---|---|---|
| 1 | Insist on the Highest Standards | Engineering Agent — independent verification of AI-generated changes | Documented Engineering Agent run/evaluation | **Locked / approved** | Same initiative as old Story 17; never count twice |
| 2 | Customer Obsession | QA-discovered invalid/incomplete order-input handling | Birlasoft QA/backend work; no isolated 42% attribution | **Locked core; wording under revision** | Do not reuse as Ownership |
| 3 | Ownership | Specification-first inventory availability + order-status MCP integrations | Birlasoft; delivered two tool-backed capabilities and handover | **Locked / approved core** | Keep coding-agent alignment motive |
| 4 | Deliver Results | Verizon holiday/iPhone promotional cart release | Cognizant customer-facing release | **Working** | Distinct from any production-debugging story |
| 5 | Think Big | CRM Core JAR reusable business components | Cognizant; resume reports **33% lower coupling** | **Working** | Do not reuse as Frugality |
| 6 | Dive Deep | CRM API performance investigation | Cognizant; resume reports **41% lower average response time** | **Approved for rehearsal** | Performance incident stays Dive Deep |
| 7 | Earn Trust | AI operations / handover dashboard for receiving team | Birlasoft; resume reports **45% lower troubleshooting time** | **Approved core; handover framing being tightened** | Dashboard belongs here, not Deliver Results |
| 8 | Bias for Action | Capstone deployment automation — move from manual deployment to Docker-based CI/CD | Physiotherapy capstone; **45 min → under 8 min** | **Grounded candidate** | Use the deployment-change incident only here |
| 9 | Invent and Simplify | Patient Queue Management — severity-based queueing / workflow design | Project; simulated **~30% lower wait time**, **25% fewer missed appointments** | **Grounded candidate** | Keep as project/simulation, never production |
| 10 | Learn and Be Curious | Learning and applying Testcontainers/WireMock for realistic integration testing | Birlasoft resume establishes JUnit/Testcontainers/WireMock/Docker/EKS integration testing | **Grounded candidate** | Need the real first application / learning trigger |
| 11 | Are Right, A Lot | CRM deduplication / matching-rule decision | Cognizant; resume reports **36% lower duplicate-record creation** | **Grounded candidate** | Need actual ambiguous-case decision; do not use fictional shared-phone example as fact |
| 12 | Success and Scale Bring Broad Responsibility | Securing AI-triggered commerce actions with OAuth2/JWT/RBAC/scoped MCP tools | Birlasoft resume-grounded security work | **Grounded candidate** | Need exact personally implemented security boundary; no invented cross-account breach |
| 13 | Dive Deep — second incident | Cognizant cart/bundle production troubleshooting using ELK + correlation IDs | README/current notes establish distributed payload tracing | **Grounded candidate** | Separate from CRM API performance |
| 14 | Insist on the Highest Standards — second incident | Frontend/back-end validation quality across CRM releases | Cognizant; resume reports **29% fewer frontend-backend validation defects** | **Grounded candidate** | Separate incident from Engineering Agent |
| 15 | Ownership — second incident | Kafka-driven catalog/order synchronization across Redis/PostgreSQL | Birlasoft resume-grounded pipeline ownership | **Grounded candidate** | Must be a distinct pipeline incident, not Story 3 MCP tools |
| 16 | Have Backbone; Disagree and Commit | Real technical/product disagreement | Not yet established | **Needs one factual confirmation** | Do not use the fictional async-vs-sync CRM story |

## Why this mix

This bank deliberately spreads the interviews across:

- current Birlasoft AI/MCP, security, Kafka, testing, dashboard and handover work;
- Cognizant CRM performance, modularization, validation and production debugging;
- one Cognizant customer-facing release;
- two clearly labeled academic/project incidents where the resume already has measurable results.

It avoids counting the same Engineering Agent initiative twice and avoids turning older fictional practice scripts into employment history.

## Story 7 — latest handover core

The strongest reason for the dashboard is **operability after R&D handover**, not simply detecting a failed API.

**Core sequence:** small R&D team builds AI/MCP solution → receiving team is newer to AI/tool-calling architecture → documentation, KT, Jira and raw logs exist but still require tribal knowledge → provide one project-level operational view backed by existing application failure information → show failure area plus correlation/request identifier → use the identifier for deeper log investigation → include the workflow in KT/handover → reduce dependence on original developers.

**Architecture boundary:** the dashboard is a first-level triage surface. It does not replace Jira or deep logs.

**Data-source wording for now:** use **existing centralized application logs / operational data exposed through the backend**, without naming CloudWatch as historical fact unless confirmed.

## Build order for the remaining stories

1. Finish Story 7 wording with the handover/KT rationale.
2. Story 8 — Bias for Action: capstone CI/CD.
3. Story 9 — Invent and Simplify: Patient Queue.
4. Story 10 — Learn and Be Curious: integration testing.
5. Story 11 — Are Right, A Lot: CRM deduplication.
6. Story 12 — Broad Responsibility: AI commerce security.
7. Story 13 — second Dive Deep: distributed cart debugging.
8. Story 14 — second Highest Standards: frontend/backend validation.
9. Story 15 — second Ownership: Kafka synchronization.
10. Story 16 — Have Backbone after one real disagreement is confirmed.

## The one unresolved factual question

For **Have Backbone; Disagree and Commit**, we still need one real event where you **disagreed with a lead/client/teammate about a technical or product decision, explained your reasoning, and then either changed the decision or fully committed to the final decision**.

Do not fill this slot with the old fictional asynchronous-vs-synchronous CRM scenario.

## Metric-defense rule

For every resume metric used in the interview, prepare:

**metric definition → source → before/after basis → comparable workload/window → calculation → attribution → technical cause**

The metric can be spoken confidently when it is the reported result for that body of work, but do not invent baseline values or measurement windows that are not preserved.
