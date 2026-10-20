# Amazon SDE II — canonical chat handoff

**Updated 20 October 2026 · Resume Ownership Q3: “Took a project end to end.”**

This is the continuation checkpoint for leaving the lengthy **Engineering Agent Comparison** chat. Customer Obsession Q1–Q6 and Ownership Q1–Q2 have been covered. Do not restart them or rewrite approved stories. Ownership Q2 intentionally used Story 3 again for a different question **within Ownership**; that was not a new incident.

## Retrieve before answering

Read current `main`, not a cached chat summary:

1. [Behavioral README](README.md) and this handoff: progress, user constraints and confirmed clarifications.
2. [Final Story Amazon Leadership](Final_Story_Amazon_Leadership.md): canonical story cores, numbering, evidence labels and readiness gaps.
3. [Customer Obsession complete module](modules/01_Customer_Obsession_Complete_Study_Module.md): current Customer Obsession Q1–Q6 variants and technical follow-ups.

Preserve the two canonical story files. This handoff records the user's latest clarifications, especially Story 3's original motivation; it does not replace the full stories. If older wording conflicts, flag it and apply the explicit constraints here without silently rewriting history. The [earlier action-focused guide](Amazon_SDEII_Action_Focused_STAR_Guide.md) has older numbering and some superseded scenarios.

**GitHub is reference documentation, not automatic RAG.** A future chat must actually retrieve these files through the GitHub connector or URLs. If access fails, identify the unavailable files and ask for their contents; never pretend they were read.

## Locked incident mapping and progress

| Incident | Primary leadership principle | Current state |
|---|---|---|
| Story 2 — QA-discovered order-input problem | Customer Obsession | Q1–Q6 studied; fit limits below remain binding |
| Story 3 — specification-first MCP delivery | Ownership | Q1 and Q2 covered; **next is Q3** |
| Story 1 — Engineering Agent | Insist on the Highest Standards | Keep this primary assignment |

**One unique real incident belongs to one primary LP. No cross-LP reuse.** Story 2 must not become an Ownership or other LP answer. The same incident may support six question variants within its primary LP, with different emphasis and unchanged facts. Its use of the Engineering Agent as a verification tool does not transfer ownership of that incident to Story 1.

Consult the current story guide for other mappings and evidence; do not presume all 18 stories are verified. Story 1 and Story 17 describe the same underlying Engineering Agent initiative in the guide: **flag this collision before actual interview use**, rather than count them as independent incidents. Secondary-LP fit tables are analytical observations, not permission to reuse incidents across LPs under this user's constraint.

## Story 2 — Customer Obsession: preserve these facts

- At Birlasoft, QA rate-limit/prompt-injection testing exposed invalid or incomplete order inputs. The engineer compared valid/invalid cases, traced the Java/Spring Boot backend, corrected validation handling and made a targeted nearby validation improvement, re-tested with QA, and pushed the feature-branch change.
- The user confirmed using their **own Engineering Agent for build/test and security checks**. Checks run does not mean checks passed. Exact reports, commands, scan findings, automated test cases, production rollout and measured customer effect are not established. Do not borrow Story 1's separate test counts or scan results.
- Responsibility boundary: the **LLM handles language, intent and tool proposals**. The **authorized deterministic backend owns authentication/authorization, validation, security, business rules, invariants and transactions**. The model is not the system of record. Explain this boundary without claiming every learning example was implemented.
- **Q2:** QA observations are not direct customer feedback. **Q3:** the saved-address stakeholder disagreement is illustrative, not historical evidence. The missing-delivery-address example is also illustrative; do not invent the actual problematic field.
- **Q6:** testing scope was reprioritized and related validation work added; no documented material schedule sacrifice, missed deadline or sacrificed deliverable exists. If the question requires one, flag the poor fit and request a different genuine Customer Obsession incident.
- The broader project's reported **42% reduction** in invalid/duplicate AI requests cannot be attributed to this single fix.

## Story 3 — Ownership: preserve the original motivation

At Birlasoft, the user personally delivered **inventory-availability and order-status MCP tool integrations**. They introduced **spec-driven / specification-first development because coding agents such as Codex or Claude Code generated code**: requirements, contracts and acceptance criteria needed to govern those implementations. The motivation was **controlling alignment of agent-generated code**, not generic documentation or simply helping future maintainers. Older Story 3 wording understates this motivation; retain this user-confirmed clarification when answering variants.

The user's personal actions were to write specifications covering inputs, outputs, business rules, errors, acceptance criteria and test expectations; guide AI coding with those specifications; personally review generated implementations against the spec; manage requirement changes through lead/client approval; follow testing; and deliver the integrations and handover. Preserve this sequence and personal accountability. Exact schema fields or a dramatic failure/redesign are not established.

The defensible outcome is delivery of the two capabilities and handover, with specifications as the reference for the user's work. **Do not claim team-wide rollout, automated spec-to-code compliance checks, quantitative improvement or unconfirmed deployment details.** Manual specification-guided development here is distinct from Story 1's proposed future automated specification-driven verification.

## What went wrong, and the rule that fixes it

| Previous chat issue | Required remedy |
|---|---|
| Repeated unnecessary rewrites and story drift | Retrieve first; lock each approved core. Change only newly user-confirmed facts with targeted edits. Preserve original history. |
| Spec-driven motivation deleted or obscured | Keep alignment of coding-agent-generated code central to Story 3's Situation, decision and Action. |
| Stories forced into mismatched questions | State the fit honestly; ask for a **different genuine incident** when evidence is missing. Do not invent one. |
| Hypotheticals presented as history; false material deadline sacrifice | Preserve illustrative labels and use “I would” for exercises. Do not invent stakeholders, deadlines, production effects or sacrifices. |
| Same incident reused across LPs | Enforce the locked primary mapping; variants are allowed only within that LP. |
| Beginner platitudes and Action too short | Explain personal engineering decisions, alternatives, trade-offs, authorization/invariant boundaries, failure modes, production safety and verification. |
| Invented impact or implementation | Separate confirmed work, proposed designs and unknowns. Use measurable or otherwise defensible impact; no invented metrics, tests, fields or outcomes. |

Use **“I” for personal actions** and “we” only for shared work/outcomes. Deliver natural STAR-L: brief Situation/Task, **Action the longest section (~60–90 seconds or a proportionate largest share)**, defensible Result, and **2–3 concise Learning sentences**. A normal full story is **3–4 minutes; six minutes is the ceiling**. Do not pad to meet a time target. Keep technical follow-ups separate. Senior SDE II depth must come from supported judgment, not invented complexity.

## Technology already studied — quick refresher only

DTO/structural validation versus service/business-state rules; 400 responses and exception mapping; MCP adapter structured errors and retryability; idempotency and concurrent attempts protected by DB unique constraints; database transactions; Kafka dual-write risk, transactional outbox and idempotent consumers; model/backend responsibility boundaries. These are learning themes, **not proof of historical implementation**. Do not teach them all again from scratch unless requested.

## Exactly how to continue

1. Confirm retrieval and briefly state: **“Customer Obsession Q1–Q6 and Ownership Q1–Q2 are covered. Resuming Ownership Q3.”**
2. Answer **Ownership Q3 — “Took a project end to end”** from locked Story 3. Emphasize the user's specification-first control of agent-generated code, personal implementation/review, approval path, testing and handover. Reuse the core within Ownership without reopening or rewriting it.
3. If a necessary historical detail is missing, ask one focused factual question; continue from established facts. If the question does not fit, flag it and ask for another genuine incident rather than manufacture a better result.
4. Mark Q3 covered only after it is actually studied. Update this checkpoint only for real progress or newly user-confirmed facts; do not extend entire modules or rework locked stories merely to migrate chats.

## Minimal copy-paste bootstrap for a future chat

```text
Continue my Amazon SDE II prep from Harish-K000/Study, current main. Use the GitHub connector or URLs to read these before answering:
https://github.com/Harish-K000/Study/blob/main/06-behavioral/README.md
https://github.com/Harish-K000/Study/blob/main/06-behavioral/AMAZON_SDE2_CHAT_HANDOFF.md
https://github.com/Harish-K000/Study/blob/main/06-behavioral/Final_Story_Amazon_Leadership.md
https://github.com/Harish-K000/Study/blob/main/06-behavioral/modules/01_Customer_Obsession_Complete_Study_Module.md
GitHub is reference docs, not automatic RAG; report access failures. Resume Ownership Q3: “Took a project end to end.” CO Q1–Q6 and Ownership Q1–Q2 are covered. Lock approved facts: Story 2=Customer Obsession, Story 3=Ownership, Story 1=Highest Standards; no cross-LP incident reuse. Story 3's specs controlled Codex/Claude-generated code alignment. No invented history, implementation or impact; flag poor fits and Story 1/17 collision. Use natural first-person senior SDE II STAR-L, Action longest (~60–90s), Learning 2–3 sentences, 3–4min normally/6min ceiling. Do not restart or rewrite finished stories.
```

## Repository reconciliation

Prepared after inspecting `main` at [`022c36e`](https://github.com/Harish-K000/Study/commit/022c36e452d5f499ac1c98bad21bbc285d9e26dd), including recent landed commits [`1276b0c`](https://github.com/Harish-K000/Study/commit/1276b0c581be1e96b70e9f22c9c2d3a2cc474f0f) (retrieval/STAR-L refinements) and `022c36e` (senior SDE II evidence/architecture). Their Customer Obsession refinements are retained. This handoff adds the continuation checkpoint and latest user constraints/Story 3 clarification; it does not rewrite the canonical story files.
