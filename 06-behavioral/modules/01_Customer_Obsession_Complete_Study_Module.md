# Amazon SDE II — Customer Obsession: Complete Study Module

**Leadership Principle 1 of 16 · Story 2 (Birlasoft) · Study before mock interviews · 19 October 2026**

> **How to use this guide:** First understand the engineering problem and trade-offs; then practice the six natural STAR answers; then learn the 27 follow-ups and system-design extensions. Do not memorize exact wording. The STAR headings are training cues: in the interview, speak as one connected story without saying “Situation, Task, Action, Result.”
>
> **Evidence boundary:** The central work is grounded in the user's described Birlasoft experience: while supporting QA rate-limit and prompt-injection testing, the engineer noticed invalid/missing order inputs, investigated/corrected backend validation handling, pushed a feature-branch change, and checked valid and invalid cases with QA. The precise field, Java change, exhaustive automated tests, release impact, and personal causal impact on the broader **42%** reported project outcome are not established. Technical code, failure examples, and designs below are **learning examples**, not claims about the deployed Birlasoft implementation.

**Source story:** [Final Story Amazon Leadership](https://github.com/Harish-K000/Study/blob/main/06-behavioral/Final_Story_Amazon_Leadership.md) — Story 2. **Interview syllabus:** the user's six Customer Obsession questions and 27 follow-ups from the consolidated Amazon SDE II question bank.

---

## Part A — What this principle tests

Amazon Customer Obsession means working backward from the customer's needs and protecting customer trust, not merely doing a task that happens to involve customers. For this story, the customers are people using the commerce order flow. The customer risk is **incorrect handling of order inputs**, not a proven production incident. The distinctive contribution is noticing a problem while working on a different QA objective and addressing it at the trusted backend boundary.

### LP fit

| Leadership Principle | Fit | Defensible reason |
|---|---|---|
| **Customer Obsession** | **Primary** | Proactively pursued a potential order-correctness risk discovered outside the assigned QA focus. |
| Ownership | Secondary | Followed an unassigned issue into the backend and through QA verification. |
| Insist on the Highest Standards | Secondary | Corrected input handling while checking that valid requests still worked. |
| Dive Deep | Secondary | Investigated the request/validation path instead of attributing the inputs to the AI model without evidence. |

**One primary LP; three useful secondary LPs.** This is not a genuine customer-disagreement story, a proven deadline-sacrifice story, or evidence of a direct customer complaint.

### Interviewer's recurring six-part drill

1. What exactly went wrong?
2. How did you locate the responsible boundary?
3. Why correct server-side behavior instead of only adjusting AI prompt instructions?
4. What legitimate traffic might stricter validation accidentally reject?
5. How did you check the fix without creating a regression?
6. What result can you actually attribute to your contribution?

---

## Part B — Technology learning map: what, why, and trade-offs

Study in this order. In a follow-up, explain the **reasoning first** and name the framework annotation or component only when it adds clarity.

| Order | Topic | Why you need it for Story 2 | Core trade-off |
|---|---|---|---|
| 1 | Spring Boot input validation | Explains what it means to correct invalid requests at the API boundary. | Reject early vs. incorrectly restricting valid requests. |
| 2 | REST contracts / error handling | Shows how a caller can recover from a rejected order. | Helpful diagnostics vs. disclosure of internal or customer data. |
| 3 | AI tool calling / MCP trust boundaries | Explains why model instructions do not enforce business correctness. | Flexible model interactions vs. deterministic backend rules. |
| 4 | Unit / integration / regression tests | Shows how you would substantiate both corrected negative cases and preserved success paths. | Fast isolated tests vs. realistic integration confidence. |
| 5 | Transactions and database invariants | Protects the order from partial side effects when validation or processing fails. | Transactional guarantees vs. contention and complexity. |
| 6 | Idempotency and retries | Protects one customer action from becoming multiple orders after an ambiguous timeout. | Durable duplicate prevention vs. key/state management. |
| 7 | Authentication / authorization | Ensures the caller is allowed to create or view the relevant order. | Access control at multiple layers vs. added implementation and test obligations. |
| 8 | Observability and customer measurements | Distinguishes a test correction from demonstrated improvement for customers. | Meaningful telemetry vs. noisy metrics and sensitive logging. |

### Topic 1. Spring Boot validation — learn this first

**Problem:** A structured JSON request can be syntactically valid but still contain invalid business data. We want to reject incorrect inputs predictably **before irreversible side effects**.

**Levels of validation:**

| Level | Example | Usual owner |
|---|---|---|
| JSON decoding/type | `quantity` is a string when an integer is required | HTTP/Jackson parser |
| Structural/field constraints | Missing product ID, blank value, nonpositive quantity | DTO + Jakarta Bean Validation |
| Cross-field consistency | Pickup vs. delivery fields have different requirements | DTO custom validator or domain/service |
| Business-state constraints | Inventory insufficient, product not purchasable, invalid order transition | Service/domain + authoritative data |
| Identity / authorization | Caller cannot create for another customer or access an order | Spring Security + domain-level ownership policy |
| Persistence invariants | Duplicate idempotency key or broken referential integrity | Database constraints + transactions |

The basic Jakarta validation annotations you should recognize are `@NotNull`, `@NotBlank`, `@NotEmpty`, `@Positive`, `@PositiveOrZero`, `@Size`, `@Min`, `@Max`, and nested `@Valid`. `@NotNull` means not null but permits an empty string; `@NotBlank` rejects whitespace-only strings. `@Valid` triggers validation of constraints defined on a nested input object; it does not automatically check stock levels in another service or authorize access.

**Illustrative Java 21 / Spring Boot 3+ controller (not historical production code):**

```java
import jakarta.validation.Valid;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;
import jakarta.validation.constraints.Positive;
import org.springframework.http.ResponseEntity;
import org.springframework.security.core.Authentication;
import org.springframework.web.bind.annotation.*;

record CreateOrderRequest(
    @NotBlank String productId,
    @NotNull @Positive Integer quantity
) {}

record OrderReceipt(String orderId, String status) {}

interface OrderService {
    OrderReceipt placeOrder(String authenticatedCustomerId,
                            CreateOrderRequest request,
                            String idempotencyKey);
}

@RestController
@RequestMapping("/orders")
class OrderController {
    private final OrderService service;

    OrderController(OrderService service) { this.service = service; }

    @PostMapping
    ResponseEntity<OrderReceipt> create(
            @Valid @RequestBody CreateOrderRequest request,
            @RequestHeader("Idempotency-Key") String idempotencyKey,
            Authentication authentication) {
        // Identity comes from the security context, never an AI-supplied customerId.
        var receipt = service.placeOrder(
                authentication.getName(), request, idempotencyKey);
        return ResponseEntity.ok(receipt);
    }
}
```

This example assumes Spring Security authentication is already enforced for the route. The service still needs authoritative inventory checks, authorization rules, durable idempotency handling, and transactional coordination; none is magically supplied by the annotation.

**SDE II trade-off explanation, naturally spoken:** “I'd reject missing or malformed fields at the request boundary because that's cheap and predictable. But I wouldn't try to encode everything in DTO annotations. Stock availability and account permissions depend on current system state, so I'd keep those in the business layer and protect the database invariants separately. The risk with validation is making rules stricter than the actual customer contract, which is why I'd test valid requests as carefully as invalid ones.”

**Spring MVC detail:** Object validation commonly raises `MethodArgumentNotValidException`; method-parameter constraints may raise `HandlerMethodValidationException` in supported versions. With `@ControllerAdvice`/`ResponseEntityExceptionHandler` you can map errors to safe, consistent responses. Know that different framework versions and controller signatures affect this. [Spring MVC validation reference](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-validation.html).

### Topic 2. REST error contracts — help the caller recover

**Why:** An invalid order should not become a vague `500 Internal Server Error`. A meaningful, stable response tells the caller whether to correct the input, authenticate, stop, or retry.

| Status | Typical use | Important distinction |
|---|---|---|
| `400` | Invalid/malformed request contract | Not every business refusal must be 400. |
| `401` | Caller not authenticated/invalid credentials | Authentication, not ownership. |
| `403` | Authenticated caller lacks permission | Role/object access. |
| `409` | Conflict with current state, e.g., incompatible idempotency-key reuse | A client might need to change the operation, not blindly retry. |
| `422` | Semantic invalidity if your API contract distinguishes it | Do not change existing conventions without reason. |
| `429` | Rate limit | Respect backoff / `Retry-After` where provided. |
| `5xx` | Server/dependency failure | May be transient; don't treat as user input error. |

**Illustrative response** (not the original order field from Story 2):

```json
{
  "code": "INVALID_ORDER_INPUT",
  "field": "quantity",
  "message": "Quantity must be greater than zero."
}
```

An alternative standardized format is RFC 9457 `ProblemDetail`, supported by Spring MVC. Whichever you choose, keep machine-readable error codes stable, avoid exposing PII or internal stack traces, and make the user's next action clear. [Spring MVC error responses](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-rest-exceptions.html).

**Trade-off:** More detail improves recovery but may leak private state. For example, a cross-account order lookup should not reveal sensitive details just because the identifier exists.

**Spoken answer:** “I want the client to know whether the request is wrong or the system is temporarily unavailable. If the quantity is invalid, the right outcome is a consistent client error that explains what to correct, without leaking internal implementation. Treating that as a server error can encourage retries that won't help the customer.”

### Topic 3. AI tool calling / MCP — separate suggestion from authority

**Conceptual request path, not a verified Birlasoft deployment diagram:**

```text
Customer message
  -> AI assistant decides which APPROVED tool to invoke
  -> MCP / tool adapter maps structured tool arguments
  -> Authenticated Spring Boot backend
  -> Authorization + request contract validation
  -> Current business-state checks
  -> Transaction / persistence / side effects
  -> Minimal safe result returned to the model
```

**Why:** Structured tool arguments tell the system what the model *wants* to do; they cannot prove that the action is authorized, valid, idempotent, or safe.

**Options and trade-offs:**

- **Prompt-only validation:** fast to change; can improve input quality, but non-authoritative, prompt-injectable, and not guaranteed across callers.
- **Tool schema constraints:** useful early filter and contract description; does not prove ownership, inventory state, or enterprise business rules.
- **Backend validation and authorization:** consistent authoritative enforcement; adds explicit rule implementation, error handling, and tests.
- **Frontend/client-side validation:** best for immediate UX; clients can bypass it, so it complements rather than replaces backend checks.

**Spoken answer:** “I use the model to understand the customer's request and choose an approved capability. But I don't treat the model's output as permission to perform a transaction. The server still validates inputs, checks ownership and business state, and controls the side effect. Prompts guide behavior; code enforces behavior.”

### Topic 4. Testing — show both sides of the contract

**What to learn:** JUnit 5 and Mockito for unit tests; MockMvc for MVC request/response behavior; representative integration tests against a real database when persistence rules matter; API contract tests when callers depend on error shapes.

**Illustrative matrix — a proposed design for study, not confirmed Story 2 tests:**

| Test | Expected behavior | Why it matters |
|---|---|---|
| Missing required value | 400 / agreed validation response; no order inserted | Negative-path correctness. |
| Blank product ID | Rejected | `@NotBlank` rather than only null. |
| Zero/negative quantity | Rejected if domain requires positive quantities | Rule clarity. |
| Normal valid request | Order creation continues to work | Prevent false rejection. |
| Valid input, unavailable inventory | Clear domain failure, no partial order | Current-state validation. |
| Same request retried with same key | Previous logical result, no second order | Retry safety. |
| Two concurrent attempts, same key | One logical operation | Atomic duplicate prevention. |
| Unauthorized account | No protected operation or data | Security boundary. |

**Unit vs integration trade-off:** Unit tests are fast, easy to isolate and diagnose; integration tests expose wiring, transaction, serialization and database behaviors. A balanced suite uses both rather than relying on only one.

**Spoken answer:** “I wouldn't stop after proving the invalid case failed. I'd also verify that a valid order still succeeds. Then I'd test the API's error contract and any persistence effect, because the customer outcome matters more than simply throwing an exception.”

### Topic 5. Transactions and integrity — don't partially commit an invalid order

An order operation may update multiple records (order header, items, inventory reservation). **Atomicity** means a transaction commits or rolls back its database changes as a unit. Database constraints defend important invariants even under concurrency.

**Trade-off:** A long transaction holding locks while calling external services creates contention and uncertain failure semantics. Prefer a bounded local database transaction; for cross-service workflows, coordinate with explicit states/compensation rather than claiming one local transaction spans everything. Validation should happen as early as practical, but current-state invariants may need rechecking with appropriate locks/version checks inside the commit boundary.

**Spoken answer:** “I'd validate the request before creating side effects, but I'd also protect the important invariants where the data changes. Inventory can change after an early check, so a correct design needs concurrency control at the point of reservation or update—not just a one-time validation before the transaction.”

### Topic 6. Idempotency, retries and ambiguous outcomes

A timeout means **the caller did not see a response**, not necessarily that the order failed. Safe retries require identifying **one business request** across multiple attempts. A new, deliberate purchase must still be distinct even if the cart contents match.

**Possible durable design:** client-generated idempotency key scoped to caller/operation, a uniqueness guarantee, stable request payload semantics, transactionally coordinated result/order reference, and a defined retention period. Under concurrent requests, check-then-insert without an atomic constraint is unsafe. Same key + different payload should be treated as a conflict according to the contract. [AWS Builders' Library — safe retries](https://aws.amazon.com/builders-library/making-retries-safe-with-idempotent-APIs/).

**Trade-off:** Durable deduplication requires extra storage, retention policies and transactional thought; without it, retry convenience can create duplicate charges/orders.

**Spoken answer:** “I separate request validation from idempotency. Validation asks whether this order makes sense; idempotency asks whether we've already applied this same customer action. I wouldn't retry an invalid request, but if a valid request times out, I need a stable business-request ID so another attempt doesn't create another order.”

### Topic 7. Authentication, authorization, record ownership

Authentication tells the system **who** the caller is. Operation permission says whether they can **invoke** a capability. Object-level authorization says whether they can **act on this specific resource**. A customer ID passed by the AI should never replace the trusted authenticated principal. Use least-privilege scopes/roles, tenant/account ownership checks, and minimal tool responses. See [OWASP BOLA](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/).

**Trade-off:** Extra checks per exposed capability require work, but the ease of accessing a service through chat must not widen access to another customer's data.

**Spoken answer:** “A user being allowed to call an order-status tool doesn't mean they can read any order ID they know. I'd derive identity from the authenticated security context and authorize access to that particular order before returning any protected data.”

### Topic 8. Observability and measured customer results

Useful measures include: validation-error volume *by category*, legitimate order success/completion, user correction/retry success, p95 latency, error-rate trends, and unintended side effects. Never put passwords, tokens, or order PII into routine logs.

**Trade-off:** A high validation-error rate might mean the backend is stricter—or that the UI is confusing. Evaluate customer recovery and successful order completion together, not just how many requests were rejected.

**Spoken answer:** “For this particular fix, I can support the QA verification. The project's wider 42% number isn't a measurement of my change alone. If I were measuring customer impact, I'd look at whether customers successfully corrected invalid requests and completed orders, not just whether we returned more 400 responses.”

---
## Part C — Six behavioral questions: natural first-person STAR scripts

**How to speak:** Treat the four paragraphs as a memory aid: context → what I needed to solve → what I personally did and why → what happened and what I learned. Speak without labels. The variants intentionally reuse the *same incident* but change emphasis to match the question.

### Q1 ★ Tell me about a time you went above and beyond for a customer.

**S — Situation**

“At Birlasoft, I was working on the backend of an AI-enabled commerce application. During one testing cycle, I was helping QA with rate-limiting and prompt-injection scenarios. While going through those cases, I noticed that some order requests with missing or invalid information weren't being handled as expected.”

**T — Task**

“That wasn't what we were originally testing. But because this was an order operation, I didn't want to leave a correctness issue unresolved just because it fell outside the current checklist. I wanted to understand the backend behavior and correct it without affecting legitimate requests.”

**A — Action**

“I compared the problematic requests with valid ones, then followed the relevant request path into the Java backend. I focused on the validation boundary rather than assuming the AI itself was the cause. My reasoning was that the backend should enforce its order contract whether a request comes from an AI assistant or another client. I corrected the affected handling and worked with QA to check the invalid cases and make sure valid orders still behaved correctly.”

**R — Result and learning**

“We verified the corrected behavior with QA, and I pushed the change to my feature branch. What stayed with me was that customer protection isn't always a separate feature request. Sometimes it's noticing something wrong while doing another task and following through because the core operation matters.”

**What makes this Customer Obsession:** You identified a potential customer-impacting risk without waiting for a complaint or separate assignment.

### Q2 Tell me about a technical decision you made based on customer feedback.

**Fit: partial. QA feedback is not a confirmed direct customer complaint.**

**S** “One close example came up during QA testing of our commerce backend at Birlasoft. We were testing rate limiting and prompt-related risks, and the tests also surfaced unexpected handling of incomplete or invalid order data.”

**T** “I needed to understand whether the issue was just a bad AI-generated input or something we needed to handle consistently in the backend.”

**A** “I looked at the relevant request-validation path and corrected the backend behavior. The important consideration for me was that instructions to an AI model can improve the request it generates, but they shouldn't be the only line of defense before an order is processed. I also checked the correction with QA against both invalid and legitimate requests.”

**R** “The behavior was corrected and verified in testing. To be precise, the feedback in this example came from QA observations, not a customer directly contacting us. The customer benefit we were protecting was reliable order handling.”

**If they insist on direct customer feedback:** This example does not meet that exact requirement; do not relabel QA as a customer.

### Q3 Tell me about a time you disagreed with a customer but still delivered a successful solution.

**Not answered by Story 2. No customer disagreement is established.** Do not pretend that choosing backend validation over prompt instructions was a disagreement with a customer. The following is a **practice-only STAR decision framework**, not a first-person historical incident:

- **Situation:** A stakeholder wants a checkout operation to accept incomplete information for convenience.
- **Task:** Clarify whether they need an editable draft or a final submitted order.
- **Action:** Explain the risk of an invalid committed order; propose draft-save flexibility with strict validation at final submission; compare error recovery and customer friction.
- **Result:** Test whether customers can save progress without invalid orders being committed. No actual delivered outcome is claimed here.

**Interview handling:** If asked this exact question, select a different **real disagreement** story rather than invent a dispute or forcing Story 2 into one.

### Q4 Tell me about a time you balanced customer needs against technical limitations.

**S** “In our commerce backend at Birlasoft, I noticed an order-validation problem while helping QA with another set of tests. Some requests with missing or invalid information weren't handled the way they should have been.”

**T** “I wanted the system to protect order correctness, but I also needed to avoid making the validation so restrictive that normal customers couldn't place legitimate orders.”

**A** “I traced the affected validation handling and focused the correction in the backend. I knew we couldn't depend entirely on the AI prompt because other callers can reach the same service, and prompts aren't enforcement. But I also paid attention to the existing order contract: a field shouldn't suddenly become mandatory just because we were fixing a negative test. I worked with QA to check both the problematic requests and the valid path.”

**R** “The issue was corrected and the relevant behavior was verified with QA. My takeaway was that protecting customers means rejecting genuinely invalid requests without creating unnecessary friction for valid ones.”

**Actual trade-off:** Correctness/safety against false rejection and compatibility. Do not imply you redesigned checkout UX.

### Q5 ★ How did you identify a customer problem before the customer reported it?

**S** “I found one while testing a commerce backend at Birlasoft. I was supporting QA on rate-limit and prompt-injection cases, so order-field validation wasn't our main objective. While reviewing the requests, I noticed missing or invalid order information wasn't being handled correctly.”

**T** “At that point, it hadn't come to me as a customer complaint. I wanted to understand whether the backend would handle those requests safely and make sure we weren't leaving that issue for a real order flow.”

**A** “I looked at the actual inputs reaching the service, followed the validation handling in the Java backend, and corrected the affected code path. I focused on the backend because it's the component responsible for enforcing the transaction contract. After the change, I worked with QA to retest the invalid scenarios as well as ordinary valid requests.”

**R** “We verified the correction during testing and I pushed it to my feature branch. For me, the lesson was to treat an unexpected testing observation as a possible customer concern rather than waiting for it to appear as a support ticket.”

**Actual detection path:** QA, not proven end-customer impact.

### Q6 Tell me about a time you sacrificed a short-term technical goal to improve customer experience.

**Fit: partial. A schedule impact, delayed deliverable or actual sacrifice is not established.** This STAR truthfully describes a change of focus, but it does not satisfy a demand for a **material** sacrifice.

**S** “I was helping QA with rate-limiting and prompt-injection testing for our commerce services when I noticed an order-input validation issue.”

**T** “That wasn't the reason I was working with QA. I needed to decide whether to leave it for a later task or spend time understanding the behavior because it affected a core order operation.”

**A** “I spent time tracing the invalid requests through the Java backend, corrected the validation handling and coordinated the retesting with QA. I focused on a small correction and verified valid order behavior rather than using it as a reason to redesign the whole service.”

**R** “We verified the affected scenarios and I pushed the correction. The short-term change here was my attention moving beyond the original testing focus. I wouldn't claim that I missed a deadline or deferred another commitment, because that isn't part of this example.”

**If the interviewer asks what was actually sacrificed:** Answer the narrower truth; use another real incident if the question requires a significant measurable trade-off.

---

## Part D — All 27 Bar Raiser follow-ups, answered naturally

**Answering approach:** For a follow-up, a full two-minute STAR would sound unnatural. Instead, use a **mini-STAR in normal speech**: remind them of the context, explain your individual reasoning/action, and finish with the supported outcome or lesson. Most answers below are ~15–40 seconds when spoken. Technical “how would you” answers are clearly design reasoning rather than invented past implementation.

### A. Customer, ownership and scope

**1. Why was this a customer problem?**  
“In our commerce application, order information affects whether the system can process a legitimate purchase correctly. I found that some invalid requests weren't handled as expected. So although I saw it during QA, I treated it as a customer-correctness concern, investigated the backend handling and worked with QA to verify the correction.”

**2. Was there an actual customer complaint?**  
“No, the issue I can describe was observed during QA testing. I didn't receive it as a specific customer complaint. What concerned me was that the behavior was in the order path, so it made sense to correct it before it became a customer-visible problem.”

**3. Why did you investigate it when it wasn't assigned?**  
“I was already looking at the API behavior with QA, and the invalid order handling was something I could trace into the backend. Instead of dismissing it because it wasn't the purpose of that test cycle, I followed it through to the relevant code path and made a focused correction.”

**4. What did you personally own?**  
“My part was investigating the affected Java backend validation handling, making the correction and checking the relevant cases with QA. I pushed the change to my feature branch. I don't need to claim ownership of the whole AI commerce platform to explain that contribution.”

**5. What was the exact invalid field?**  
“The exact field isn't preserved in my current study notes. What I can explain is the type of problem I investigated—missing or invalid order input at the backend boundary—and the correction and verification work I did. I wouldn't invent a field or annotation just to make the story more specific.”

**6. How did you prioritize the work?**  
“The issue was on the order path, which is a core customer operation. I gave it attention because incorrect request handling can create a bad customer outcome. I made a focused change within that scope rather than starting a large redesign. I don't have evidence that it displaced a dated deliverable.”

**7. Who was the customer?**  
“The end customers were people using the commerce order flow. QA helped us find the unexpected behavior before I could point to any direct customer report. My decision was based on protecting a customer-facing transaction boundary.”

### B. Why this technical approach?

**8. Why backend validation instead of prompt engineering?**  
“In that order flow, the AI could help produce a request, but the Spring Boot service was responsible for accepting or rejecting it. A prompt might lower the number of bad inputs, but it can't guarantee correctness for every caller. That's why I focused the correction at the server boundary and verified the behavior there.”

**9. Why not validate only in the frontend?**  
“Frontend checks are useful because they give the customer feedback immediately. But the order API can also be called from other clients or integrations. So I'd keep frontend validation for user experience and backend validation as the authoritative contract.”

**10. Why not accept incomplete orders and repair them later?**  
“If we're talking about a final order, accepting required data as missing can create messy downstream state. I'd rather reject it or handle it explicitly as a draft. The distinction matters: saving an incomplete draft is a valid product choice, but silently committing an invalid order is not.”

**11. Could stricter validation make the experience worse?**  
“Yes. If we accidentally make an optional field mandatory or interpret a rule incorrectly, we can reject legitimate customers. That's why, after addressing the invalid path, I also checked valid requests with QA. Good validation protects the existing contract; it shouldn't invent new restrictions.”

**12. Did the LLM cause the invalid requests?**  
“I didn't establish that. What I observed was missing or invalid data reaching the backend. I followed the behavior at the point we could enforce correctness rather than making an unsupported assumption about where the bad data originated.”

**13. Why not redesign the entire workflow?**  
“The problem I was looking at was specific to input handling. A large redesign would introduce unrelated risks and slow the correction down. I focused on the relevant backend path and checked that the normal order flow still worked.”

**14. What alternatives did you actually consider?**  
“The key engineering distinction I can defend is why backend validation must be authoritative instead of relying only on prompt or caller-side behavior. I wouldn't present a long formal alternatives review that wasn't part of the recorded incident. Technically, frontend validation, tool schemas and prompts can all help, but they're complementary controls.”

### C. Validation, API semantics and error safety

**15. How would you validate an order request in Spring Boot?**  
“I'd first define the order request DTO and put simple constraints such as required product ID and positive quantity there. In a Spring MVC controller I'd trigger those with `@Valid`. Then I'd put rules involving live inventory, account permissions or order state in the service layer, because annotations alone can't check authoritative system state. That's a design explanation, not a claim about the exact code in this incident.”

**16. What's the difference between field validation and business validation?**  
“Field validation checks things like whether a quantity was supplied and whether it's positive. Business validation asks whether the operation is actually allowed—for example, whether inventory is available or an order can move to the requested state. Both matter, but they depend on different information and belong in different layers.”

**17. What should an invalid-input API response look like?**  
“I'd want a predictable client error with a stable code and enough safe detail to tell the caller what to correct. For malformed or missing required input, that often means a 400 response. If the API distinguishes semantic failures, it may use 422. The important thing is consistency with the contract, not choosing a status in isolation.”

**18. What if the request is valid but inventory is unavailable?**  
“Then the payload passed input validation, but the business operation still cannot proceed. I'd return the agreed domain error and avoid committing an incorrect order or reservation. That's why I wouldn't treat passing `@Valid` as proof an order should be accepted.”

**19. What if invalid input reaches the database?**  
“I'd try to reject it before side effects. But I'd also rely on database constraints and appropriate transaction boundaries for critical invariants, because application-level validation alone can miss concurrency cases or be bypassed by another code path. I'd test that a rejected request leaves no partial order.”

**20. How do you avoid leaking data in validation errors?**  
“I'd return the safe error category and corrective information, not raw database exceptions, tokens, customer records or stack traces. I'd also avoid letting the AI tool response include unnecessary order data. A useful error helps the customer without exposing the internals.”

### D. Testing, result measurement and reflection

**21. How did you verify the actual fix?**  
“I worked with QA to check the requests that had been handled incorrectly, and also checked valid requests so the correction didn't break normal processing. That is the verification I can support from this incident.”

**22. Did you write automated regression tests?**  
“I can speak to QA verification, but I don't have a confirmed automated test identifier for this particular correction. As a next improvement, I'd automate the same negative and positive cases so a later change couldn't silently reintroduce the behavior.”

**23. How would you strengthen the tests now?**  
“I'd make the order-input contract explicit, then create parameterized tests for missing, blank, malformed and valid values. I'd check the response code and body, and separately verify that rejected requests have no transaction side effect. For rules involving persistence or concurrency, I'd add integration tests rather than relying only on mocks.”

**24. How did you measure customer improvement?**  
“For this correction, the evidence I have is that the affected behavior was retested with QA. I don't have a production customer-impact measurement tied to this single change. I'd distinguish that from the broader project reporting.”

**25. Did you achieve the reported 42% reduction?**  
“That 42% was a broader initiative result involving invalid and duplicate AI requests. My individual contribution here was the backend validation correction, so I wouldn't attribute the entire project metric to this one change.”

**26. How would you measure it in production?**  
“I'd look at validation failures grouped by error type, but I'd also check whether customers can recover and complete a valid order afterward. If errors go up while successful orders fall, we may have made the contract too strict. I'd compare a baseline and after-state and avoid logging sensitive order data.”

**27. What would you do differently?**  
“I'd make the order-input contract and negative cases clearer earlier. Then we'd have shared examples of what should be rejected, what should be accepted, and what error the caller should receive. That would make it easier to detect this kind of issue before a broader QA cycle.”

---
## Part E — System-design follow-ups: spoken technical answers

These are **architecture exercises**, not additional claims about what was implemented in Story 2. Here, structure the answer as **problem → goal → design decision → failure handling → how I'd verify**. This gives a natural STAR-like shape for a technical interview.

### The 90-second design prompt

**Interviewer:** “How would you design an order-creation API that safely handles invalid inputs?”

**Answer:**

“I'd first clarify what creating an order actually means for this system. For example, are we saving an editable draft or confirming a purchase? That tells me which fields are genuinely required and which failures should be shown to the customer.

“At the Spring Boot API boundary I'd validate the request structure and straightforward field constraints. Separately I'd authenticate the caller, check permissions and validate the current business state, especially inventory and whether the operation is allowed. I wouldn't use an AI prompt as the only validation layer, even if the request came through an agent.

“Before any irreversible side effect, I'd use the right database transaction and constraints to protect the order state. I'd also design idempotency around a stable customer-request key because a timeout doesn't tell the client whether an order was created. If a dependent service failed, I'd have an explicit failure state or recovery path instead of leaving the user guessing.

“Finally, I'd test invalid, valid and concurrent requests, plus lost responses and retries. And I'd monitor whether customers recover from validation errors and successfully complete orders—not just the number of 400s we're returning.”

### Seven deeper probes

**1. What if two requests try to create the same order at the same time?**

“I'd decide first whether they represent one customer action or two legitimate purchases. If it's the same action, they should share a stable idempotency key. The backend needs an atomic uniqueness guarantee, not an in-memory check followed by an insert, because both requests can pass that check concurrently. I'd verify that one logical order results even under overlapping attempts.”

**Trade-off:** Database-backed deduplication has cost and key-retention requirements; it's preferable to accidental duplicate business side effects.

**2. What if inventory changes between checking it and saving the order?**

“An early inventory lookup can be stale as soon as it returns. I'd protect the inventory reservation or decrement with a concurrency strategy—such as an atomic conditional update, optimistic versioning, or carefully scoped locking—depending on contention and consistency requirements. I'd test simultaneous purchases for the last unit, not just sequential happy paths.”

**Trade-off:** Stronger serialization reduces oversell risk but can add contention and latency.

**3. What if the database creates the order but the HTTP response is lost?**

“I wouldn't treat a timeout as proof the order failed. The caller should retry with the same request identifier, and the backend should return the existing logical outcome rather than create another order. That requires the idempotency record and order result to be coordinated durably, with clear behavior when the same key arrives with different payload data.”

**Trade-off:** Durable idempotency simplifies caller recovery but increases implementation/state-management complexity.

**4. What if the AI tool sends a different customer ID?**

“I'd treat the AI-supplied ID as untrusted input. The real caller identity must come from the authenticated application security context, and the backend must authorize the specific business operation or record. Being allowed to invoke the tool isn't permission to access another customer's order. I'd also limit the returned fields to what the tool actually needs.”

**Trade-off:** Extra checks can add work, but they preserve the original customer's security boundary when a new AI channel is added.

**5. What if a downstream payment or inventory service is unavailable?**

“I'd decide which steps can safely complete and which require a pending or failed state. I wouldn't hold a database transaction open indefinitely across network calls. For retriable transient failures I'd use bounded retries and backoff; for permanent errors I'd stop and return a clear outcome. For multi-step business changes, I'd use explicit workflow state and recovery or compensation appropriate to the domain.”

**Trade-off:** Asynchronous recovery can improve reliability but makes state transitions and user messaging more complex.

**6. How would you change validation rules without breaking existing clients?**

“I'd start with the current request and response contract, identify existing callers and use cases, and classify the rule as additive or breaking. I'd avoid silently turning optional fields into required ones. I'd test old and new examples, communicate breaking changes, and use versioning or a staged rollout when necessary.”

**Trade-off:** Compatibility can constrain a cleaner design; migration should be justified by actual customer benefit.

**7. How would you know if invalid-order behavior became a problem again?**

“I'd monitor error codes by safe category, successful order completion, and the percentage of callers that correct an error and succeed. I'd also look at sudden changes in error rates and p95 latency after releases. More validation errors alone isn't automatically improvement; it might mean we're rejecting valid customers.”

**Trade-off:** Useful instrumentation takes engineering effort and careful privacy handling; it provides evidence beyond a test-only pass.

---

## Part F — How to think through any unfamiliar follow-up

If you forget a prepared line, respond using this sequence:

1. **Name the customer concern in plain English.** “I didn't want a bad order request to produce an incorrect transaction.”
2. **State the decision and why.** “I focused on the backend because it has to enforce the contract for every caller.”
3. **Describe a real alternative and drawback.** “Prompt validation improves inputs but can't guarantee correctness or protect another client.”
4. **Describe what you checked.** “I verified affected invalid and legitimate requests with QA.”
5. **State the supported outcome.** “The backend behavior was corrected and verified in testing.”
6. **Name the limit without overexplaining.** “The wider 42% project improvement isn't attributable to this change alone.”

**The main sentence to remember:**

> “I noticed unexpected order data while testing something else, traced it into the Java backend, and corrected the validation boundary because that's where order correctness must be enforced.”

**Four technical sentences to remember:**

> “Prompts guide behavior; backend code enforces business rules.”
>
> “`@Valid` checks declared input constraints; it doesn't prove inventory is available or a customer is authorized.”
>
> “A timeout describes what the caller observed, not whether the order was committed.”
>
> “I have to test that invalid requests fail **and** valid requests continue working.”

### Don't accidentally turn study knowledge into claimed history

| Fine to describe from Story 2 | Learn as engineering examples, not personal claims |
|---|---|
| Found order-input handling issue while supporting QA | Exact missing field, Java annotation or line of code |
| Traced affected backend validation and changed handling | That a particular `@ControllerAdvice` or custom exception was implemented |
| Retested affected cases and valid requests with QA | Specific JUnit, MockMvc, WireMock or Testcontainers test results for this incident |
| Pushed correction to a feature branch | Confirmed production customer complaint, production deployment or prevented outage |
| Knew the broader commerce project reported 42% fewer invalid/duplicate requests | Attribution of the 42% result to this one validation correction |

---

## Part G — Study checklist (finish all before a mock interview)

- [ ] Explain the Story 2 incident in 90–120 seconds without reading.
- [ ] Explain why Customer Obsession is primary and Ownership / Highest Standards / Dive Deep are secondary.
- [ ] Understand `@NotBlank`, `@NotNull`, `@Positive`, `@Valid`, DTO validation, and business-state validation.
- [ ] Understand what Spring MVC does with validation exceptions and why callers need stable error contracts.
- [ ] Explain prompt-only vs frontend vs tool schema vs backend enforcement, with benefits and limits.
- [ ] Explain positive, negative and regression tests, including why valid requests matter.
- [ ] Explain local database transactions, atomic updates and the inventory race condition.
- [ ] Explain stable idempotency keys, ambiguous timeouts and concurrent retries.
- [ ] Explain authentication vs role permission vs object-level authorization.
- [ ] Explain how to measure recovery, successful order completion and the limits of the 42% number.
- [ ] Read and practice all six behavioral question variants without inventing a customer disagreement or schedule sacrifice.
- [ ] Read and answer all 27 follow-ups aloud in 15–40 seconds each.
- [ ] Explain the 90-second safe-order-API design and seven deep technical probes.
- [ ] Only after all items above: begin the interviewer-style mock round.

### Self-test: questions you should be able to answer before moving on

1. What does `@Valid` actually do, and why isn't it enough for inventory availability?
2. Why might `@NotNull` accept an empty string that `@NotBlank` would reject?
3. Why do we need a stable client error code, not just a generic 500?
4. If the AI produces a perfectly shaped order DTO, is the request safe to execute? Why not?
5. How could a new validation rule unintentionally harm customers?
6. What evidence would you collect to show that no invalid order was created?
7. Why does a response timeout not imply that a transaction failed?
8. Why is a database unique constraint important under concurrent retries?
9. What is the difference between permission to call `getOrderStatus` and permission to view order `O123`?
10. What does the 42% project metric establish—and what does it not establish about Story 2?

---

## Reference material

- [Your final 18-story STAR / follow-up guide](https://github.com/Harish-K000/Study/blob/main/06-behavioral/Final_Story_Amazon_Leadership.md)
- [Amazon Leadership Principles](https://www.amazon.jobs/content/en/our-workplace/leadership-principles)
- [Amazon SDE II interview preparation](https://amazon.jobs/content/en/how-we-hire/sde-ii-interview-prep)
- [Spring Framework — MVC validation](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-validation.html)
- [Spring Framework — MVC API error responses](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-rest-exceptions.html)
- [AWS Builders' Library — Safe retries with idempotent APIs](https://aws.amazon.com/builders-library/making-retries-safe-with-idempotent-APIs/)
- [OWASP — Broken Object Level Authorization](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/)

**End of Customer Obsession module. The next step is studying these topics, not taking a mock interview yet.**