# Amazon SDE II — Locked Interview Answer Standard
**Updated: 2026-10-21.** This is the working ruleset for all future Leadership Principle STAR-L stories and Bar Raiser technical follow-ups. It consolidates the user's explicit corrections, the repository's canonical handoff, and external technical references. **Read this file before writing or revising any story.**

## 1. Governing priority and retrieval
1. **The user's latest direct correction prevails over older generated scripts.** Do not repeat a rejected framing even if a legacy module includes it.
2. Retrieve this ruleset, `06-behavioral/AMAZON_SDE2_CHAT_HANDOFF.md`, `06-behavioral/Final_Story_Amazon_Leadership.md`, and the relevant LP module from current GitHub `main`. The action-focused guide is older and may contain superseded scenarios.
3. Distinguish **observed historical fact / documented implementation / reported wider-project result / engineering inference / hypothetical illustration / proposed future improvement**. Do not silently transform one class into another.
4. Preserve accepted incident cores. When asked to continue, advance; don't restart or rewrite the previous answer unless explicitly requested. Only one question or one requested story per turn; no unrequested chapter, mock, quiz, or long curriculum.
5. Natural first-person spoken English. A candidate explaining their real engineering work, not a textbook, architecture lecture, or leadership slogan. Use `I` for personal contribution and `we` only for shared outcomes.

## 2. Required answer shape: STAR-L
- **S (brief):** Concrete system/customer context and observable problem. Name the affected capability.
- **T (brief):** What I personally needed to deliver or resolve, including the real scope of authority.
- **A (largest and strongest):** Specific observation -> reproduction/investigation -> evidence or diagnosis -> material decision **if one actually occurred** -> precise change made -> verification -> delivery/follow-through. Include the reason **and the concrete consequence** of a decision, not boilerplate motivation. A real alternative/trade-off only when the history or code supports it.
- **R (mandatory impact):** (a) what changed, (b) how it was verified, (c) who benefited and how, distinguishing **observed** impact from **plausible** customer value, (d) supported metrics with attribution limits. If only a feature-branch change and QA retest are confirmed, say that, not "production improved."
- **L (2–3 spoken sentences):** Specific lesson and practical future change, such as test coverage, contract documentation, decision log, change history, and AI-assistant-readable context. Say "I would" for unimplemented improvements.
- **Timing:** The user has approximately **six minutes for initial answer plus interviewer follow-ups**, not six minutes for an uninterrupted monologue. Choose a self-contained answer with enough technical depth and leave real room for follow-ups; roughly 2.5–3.5 minutes can be a planning baseline, **not** a rigid requirement. Do not announce a generic 90-second target or pad to fill time. Match the requested length.

## 3. Senior SDE II engineering-quality gate
**Before drafting, privately test these questions:**
- Can I name **what precisely failed or needed improvement**, not just "there was an issue"?
- What observation/test/log/spec showed this? What was expected versus observed?
- Which code, integration, contract, service, or tool did I **personally** change? Is that supported?
- If I include a "decision," was there a genuine engineering alternative with actual consequences, or just following established standards?
- What made this work more than routine implementation? Initiative, ownership through delivery, nonobvious diagnosis, cross-service impact, measurable improvement, or clear risk reduction — **demonstrated**, not asserted.
- Are test and release claims exact? **Ran checks** does not imply **passed**. QA retest does not imply production rollout.
- Can I say what improved, for whom, and with what proof? Do not write "successful" without a result.
- Can I defend each technical statement against the code and system boundaries? If not, remove it or label it a study example.
- Can a Bar Raiser ask "which field/class/query/test/log?" and get a truthful response? If the source does not establish an essential detail, **mark the gap; don't fill it with fabricated specifics**.
- Does the incident truly fit the question (real blocker, disagreement, deadline, mistake, customer feedback), not merely have a transferable moral?

### Explicitly prohibited framing
- **Never portray backend order validation, ordinary requirement analysis, proper QA, or consulting the business owner as a heroic architectural decision or exceptional trade-off.** These are baseline SDE II engineering responsibilities.
- **Do not manufacture a "prompt versus backend" decision** for the Birlasoft Customer Obsession order-validation incident. Backend validation is already the expected architecture. Although older GitHub notes contain this comparison, the user explicitly rejected it as irrelevant to the actual incident. Mention an AI/backend trust boundary **only in technical follow-ups when it directly answers a question**, not as the story's main Action.
- Do not invent a quick/hasty `if` implementation that the engineer nobly declined; standard impact analysis is not an extraordinary trade-off.
- No generic "I could have added an if statement," fake microservice-vs-monolith debates, imaginary customer complaint, blocked team, outage, rollback, deployment, adoption, measurable result, or unverified saved-address example.
- Do not imply that invalid orders were actually **persisted** or that AI caused them; neither is established.
- No vague "I improved reliability/security/maintainability" without explaining actual evidence and scope.
- No stacked technical buzzwords to make an unsupported story seem senior.
- Never invent a failure/solution only to make an LP question fit. Flag weak fit and choose a documented alternative when available.

## 4. Results / impact evidence ladder
1. **Verified direct outcome:** code change, precise behavior demonstrated by tests, deployed feature, saved run result, accepted handoff.
2. **Verified engineering/customer effect:** a demonstrated passing regression case, prevented incorrect state in a test, repeatable faster task, measured error/latency/defect improvement. Specify environment.
3. **Reported team/project metric:** quote only with its scope, measurement caveat, and no unsupported personal attribution.
4. **Expected business benefit:** explain as a potential risk reduction, not an observed production impact.
5. **Unknown:** say what measurement would establish it; do not fabricate baseline/after numbers.

For each story, collect where possible: observed symptom; reproduction input; expected/actual; original root cause; exact changed boundary; regression tests and environment; branch/PR/release; customer or consumer affected; before/after metric and data source; constraints/alternative actually considered; next-step documentation. Never ask the user to reconstruct details repeatedly when sources already answer.

## 5. Locked source-grounded incident notes and collisions
- **Customer Obsession A — Story 2, Birlasoft QA-discovered invalid order-input handling.** Known: QA working on rate limits/prompt injection, unexpected missing/invalid order data handling noticed, valid/invalid requests compared, Spring Boot path investigated, affected validation handling corrected plus targeted nearby improvement, QA re-test, own Engineering Agent build/test/security checks **run**, feature branch push. **Unknown:** exact field, observed response, changed class/annotation, scanner pass/fail, production rollout, attributable customer KPI. Project-wide 42% decrease in invalid/duplicate AI requests is **not attributable** to this individual correction. Main interview value: identified customer-correctness risk outside initial QA objective, investigated, corrected, verified. **Do not center prompt/backend architecture.**
- **Customer Obsession B — AI commerce permissions.** Documented foundation: OAuth2/JWT/RBAC, scoped AI tool access and order-status integration. **Not confirmed:** exact cross-account object-level authorization checks, negative test results, blocked malicious request, prevented breach. Describe actual scoped access; reserve more detailed object-level scenarios for explicitly labeled architecture follow-ups.
- **Ownership A — Story 3, inventory-availability and order-status MCP integrations.** **Critical original motivation:** use written specs to keep **Codex/Claude-generated code aligned with requirements and acceptance criteria**. Personally specified request/response contracts, business rules, errors and tests; guided/reviewed agent-generated implementation; coordinated business changes through lead/client approval; tested, delivered and handed over. Don't dilute the motivation into generic documentation. Do not claim automatic spec-compliance enforcement, team-wide rollout or measured productivity increase. Next learning: preserve design decisions, change reasons and tests so future engineers or AI assistants can debug with context.
- **Ownership B / Insist on the Highest Standards — Engineering Agent.** The same real project appears under two LP framings; **it is one incident, not two**. Independently bounded Git/tool access, builds/tests, fixed security scanners, structured evidence, GitHub Actions PR flow and audit. Example run: **63 tests passed**, nine dependency advisory matches including two critical-rated; review **BLOCKED** for investigation, **not** proof that production merge was actually stopped or vulnerabilities exploitable. If used in Ownership, don't reuse the same incident as a separate Highest Standards event within the same loop without acknowledging the collision.
- **Failure/Learning — Engineering Agent local model diagnostic.** **Same project again, alternative story angle.** Four fixtures; three labeled concerns; local `qwen2.5:1.5b` found **0/3**, zero proposed findings, zero discarded by evidence filter; 4/4 structured responses. A negative diagnostic finding, **not a successful model fix**. No production-accuracy claim. Distinct Failure incident still needed if the Engineering Agent has already been used for another LP in the same loop.
- **Bias for Action — rollback/outage in Story 9 is FICTIONAL.** Not workplace history. Use only as a clearly labeled practice architecture scenario. Need a real incident showing a consequential, reversible decision taken with incomplete information and a defensible result.
- **Story 14 excluded** from practice at user's direction.
- Default goal is **two distinct genuine story incidents per LP** where possible. Reusing one incident for multiple questions within an LP is acceptable when genuinely responsive; avoid repeating the same incident across LPs in one loop. Surface unavoidable collisions explicitly. A repository's fictional scenario is not upgraded into real history because it sounds technically realistic.

## 6. Technical sources: how they may and may NOT be used
Use official references to make the **separate technical-follow-up explanation** correct and to inspect real design alternatives. They are **not evidence that the candidate historically made that decision or implemented every pattern**. Do not introduce a framework concept into an Action section unless the actual work supports it.

- **Google AIP-193: errors** — https://google.aip.dev/193 — consistent, actionable error semantics. Google AIPs describe Google's API conventions; do not pretend they are literal required Spring MVC implementations.
- **Google AIP-203: field behavior** — https://google.aip.dev/203 — document required versus optional/immutable field behavior, distinguish contract specification from business-state validation. Not a substitute for historical invalid-field evidence.
- **Google AIP-155: request identification / idempotency** — https://google.aip.dev/155 — stable request identifiers and retry safety. Discuss only when the question involves duplicated side-effectful requests/retries.
- **Microsoft Azure Architecture Center: API design** — https://learn.microsoft.com/en-us/azure/architecture/microservices/design/api-design — service contracts, compatibility, versioning, separation of API from internal model, retries/idempotency.
- **Microsoft Azure Architecture Center: Web API implementation** — https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-implementation — expected routes, auth, exception handling, meaningful responses, tests, monitoring.
- **NVIDIA Secure Agent Workspace: Agent blueprint patterns** — https://docs.nvidia.com/enterprise-reference-architectures/secure-agent-workspace-reference-design/latest/agent-blueprint-patterns.html — explicitly scoped tools, capability permissions, review gates, logging and lifecycle ownership. This informs discussion of agent tool security; **it does not establish the Birlasoft implementation**.
- **Repository implementation evidence:** https://github.com/Harish-K000/Spring-Boot/blob/main/services/engineering-agent/README.md — concrete Engineering Agent tools, security restrictions, tests/evaluations. Prefer this evidence for actual Agent details.

**Apply this mapping to technical follow-ups, not as performative citations inside the spoken STAR-L.** Examples: a validation-related probe -> distinguish DTO constraints from domain invariants and exact error behavior; MCP scope probe -> distinguish tool invocation permission from object access; idempotency question -> request identity and atomic duplicate protection; reliability probe -> what was actually tested vs only proposed.

## 7. Follow-up protocol (one input at a time)
1. Give the requested **complete answer in STAR-L** when asked for a story, or a direct technical answer when asked for one follow-up.
2. On story requests, optionally add **2–4 highest-risk Bar Raiser probes with short spoken answers**, not dozens, unless explicitly asked for comprehensive follow-ups.
3. Emphasize the likely probe: "what specifically was wrong?" -> "how did you localize it?" -> "what exact implementation change?" -> "what test demonstrates the correction?" -> "what outcome and attribution?"
4. If source gap blocks a truthful technical answer, say **"not established in the repo"**, then separately explain **what a proper implementation would look like**. Never blur the boundary.
5. Technical answers should explain why things work and relevant failure modes using Google/Microsoft/NVIDIA references when relevant, while keeping historical claims grounded.
6. Avoid repeated apologies, promises and wholesale rewrites. Incorporate feedback into **this ruleset**, then apply it consistently. Do not spend the user's preparation time repairing the same mistakes.

## 8. Pre-send checklist (MUST PASS)
Before producing a new answer, check:
[ ] One actual incident, best-fit question; unique LP mapping checked.
[ ] No story inventions, assumption presented as observation, or metric misattribution.
[ ] No manufactured choice between following standards and violating standards.
[ ] Action demonstrates investigation, individual contribution, relevant genuine judgment and verification.
[ ] Result conveys delivered outcome, actual evidence and bounded customer/engineering impact.
[ ] Learning practical, brief, clear on future versus implemented.
[ ] Technical follow-ups source-accurate and **separate** from the story.
[ ] No unnecessary time targets, filler, scripts rewritten unasked, or multiple questions bundled.

## 9. Current checkpoint
- Customer Obsession Q1 (order validation) is **under revision, not user-approved**. The user's latest explicit correction rejects the invented "I decided backend validation over prompt" narrative; use the actual debugging/validation work only.
- User wants rules **persisted and used**, not re-litigated or rewritten with each turn.
- Five-LP study pack and earlier story variants exist, but **do not treat draft scripts as authoritative evidence**.
- Before resuming the next question, retrieve the relevant canonical story and this playbook.

## New-chat bootstrap
"Continue Amazon SDE II behavioral preparation. **First fetch** `Harish-K000/Study/06-behavioral/INTERVIEW_ANSWER_STANDARD.md`, `AMAZON_SDE2_CHAT_HANDOFF.md`, and `Final_Story_Amazon_Leadership.md` from current main. Treat my latest corrections as binding. No fabricated trade-offs, fake incidents, invented production impact or generic backend-validation-versus-prompt monologues. Detailed personal Action; evidence-backed Outcome & Impact; short Learning; credible technical Bar Raiser answers using Google AIPs, Microsoft Azure API design, NVIDIA agent security references only as study support. One question at a time. Do not restart completed stories." 
