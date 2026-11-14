# Story 8 — Bias for Action: Narrowing a Distributed Cart Failure Before Waiting for Full-System Diagnosis

**Status:** Finalized safe rehearsal version on 2026-11-13.  
**Primary LP:** Bias for Action  
**Target question:** Tell me about a time you had to act quickly without having all the information.

> Evidence boundary: The Cognizant cart workflow, distributed JSON flow, ELK/correlation-ID investigation, payload/contract comparison, lower-environment reproduction, focused diagnostics, and cross-team evidence sharing are grounded in the current preparation notes. The exact production configuration/root cause, incident duration, hotfix/rollback, and quantitative incident impact are **not** established, so this story intentionally does not claim them.

## Situation

At Cognizant, I supported a customer-facing cart workflow where a request could move through several services, including pricing, promotion, inventory and checkout-related components.

In one incident, a **bundle-related cart flow was executing unexpectedly**. From the cart service alone, it was not immediately clear whether the problem came from our own conditional logic or from a value arriving in an upstream JSON response.

Because several services were involved, waiting for every team to investigate its entire stack first would have slowed the diagnosis and created a lot of back-and-forth.

## Task

My responsibility was to **move the investigation forward quickly without making a speculative production change**.

I needed to narrow the failure to a specific request, identify the decision point in our service, and produce enough evidence to determine whether the next action belonged to our code or an upstream dependency.

## Action

I started with the **correlation ID** for the affected request and used ELK to reconstruct the request path instead of reading logs service by service without a clear anchor.

I compared the payload received by our cart service with the relevant API contract and then traced the values through our business logic. That narrowed the unexpected behavior to a **field value that selected a nested conditional path** in the cart workflow.

At that point I still did not assume our service was wrong. The same symptom could come from either side of the contract: our code could be interpreting a valid value incorrectly, or the upstream service could be sending a value that did not match the agreed behavior.

Rather than wait for a full multi-team investigation, I took a reversible step: I **reproduced the same request in a lower environment and added focused diagnostics around the decision point**. That let me compare the input, the branch selected by our code, and the expected contract behavior without changing the production business logic based on an incomplete hypothesis.

I then verified our own handling first. If the received value was valid and our logic was wrong, the evidence told us exactly where our correction belonged. If the payload itself did not match the contract, I packaged the request/response details, timestamp, correlation ID and expected behavior so the upstream team could act on a reproducible case instead of a vague statement that their service was wrong.

The alternative was to stop at “something upstream looks wrong” and wait for the other team to investigate. That would have reduced my immediate effort, but it would also have made the handoff slower and less credible.

The trade-off in my approach was spending some additional time reproducing and instrumenting a path that might ultimately be owned by another service. I accepted that because the investigation was low-risk and reversible, while a speculative code change in a customer-facing cart flow was not.

## Result

I narrowed the issue from a broad distributed-workflow problem to a **specific request, payload value and decision point** and gave the teams a common evidence set to work from.

That reduced unnecessary back-and-forth and helped identify the correct owner and resolution path without making an unverified change to the customer-facing flow.

I do not attach a numerical incident-time or customer-impact result to this story because those measurements are not preserved in the current evidence.

## Learning

I learned that **Bias for Action does not mean changing production code before understanding the risk**. I can move quickly by taking reversible investigative actions that reduce uncertainty and create evidence for the next decision.

## Memory spine

**Unexpected bundle flow → distributed ownership unclear → correlation ID in ELK → compare payload to contract → trace branch decision → reproduce lower environment → add focused diagnostics → verify our side first → evidence-backed owner/resolution**

## Likely interviewer notes

- Customer-facing distributed cart workflow
- Acted under **incomplete ownership/root-cause information**
- Used **correlation ID + ELK**
- Narrowed issue to a concrete payload/decision point
- Chose **reversible investigation over speculative production change**
- Verified own service before escalating
- Cross-team evidence instead of blame
- No invented incident metric

## Why this fits Bias for Action

The fast action was not a blind hotfix. I used the information already available to reduce uncertainty immediately: trace one real request, reproduce it safely, instrument the decision point, and establish ownership.

That is a calculated action because those steps were **low-risk and reversible**, while changing business logic before understanding the contract would have carried greater customer risk.

## Key trade-off

**Wait for every service team to complete its own investigation:** lower immediate effort for me, but slower convergence and more ambiguous ownership.

**Make a speculative code change:** potentially fast, but high risk because the root cause was not yet established.

**Trace, reproduce and instrument the affected path:** some extra investigation effort, but reversible, evidence-driven and fast enough to move the incident toward resolution.

I chose the third option.

## High-risk Bar Raiser follow-ups

### What exactly did you do quickly?

I anchored the investigation on the affected request's correlation ID, traced the payload through ELK and our cart logic, reproduced the behavior in a lower environment, and added focused diagnostics around the branch that was being selected. That gave us evidence without requiring a speculative production change.

### Why is that Bias for Action rather than Dive Deep?

Dive Deep is certainly a secondary signal because I traced the data path carefully. The primary Bias for Action signal is that I did not wait for a full cross-team diagnosis before making progress; I chose reversible investigation steps that immediately reduced uncertainty and moved ownership toward the right team.

### Why didn't you just fix the conditional?

Because at that point I did not yet know whether our conditional was wrong or whether the upstream value violated the contract. Changing business logic before resolving that distinction could have fixed one example while breaking valid cart behavior.

### Why didn't you immediately blame the upstream service?

I verified our own behavior first. Once I had the received payload, the contract expectation and the branch our code selected, I could communicate an evidence-backed mismatch rather than an assumption.

### What was the exact field or configuration?

That exact historical detail is not preserved in the current notes. I would not invent it in an interview; I would describe the confirmed investigation sequence and only name the field if I recover it from the original incident evidence.

### Did you deploy a hotfix or rollback?

The current evidence does not establish a specific hotfix or rollback for this incident, so I do not claim one. The result I can defend is the narrowed failure, reproducible evidence and correct ownership/resolution path.

## Sentence to remember

**“I moved quickly by reducing uncertainty with reversible investigation steps; I did not confuse Bias for Action with making a speculative production change.”**
