# Story 7 — Earn Trust: Making AI Operations Transparent

**Status:** Approved for rehearsal on 2026-11-11.  
**Primary LP:** Earn Trust  
**Target question:** Tell me about a time you had to earn the trust of other teams or stakeholders.

> Evidence boundary: The Birlasoft dashboard work and the reported **45% reduction in operational troubleshooting time** are grounded in the resume. The current notes do not preserve the exact before/after measurement window or calculation method, so do not invent those details in follow-ups.

## Situation

At Birlasoft, I work on an AI-enabled commerce platform where customer requests can pass through several layers: the AI interaction, MCP tool calling, Java/Spring Boot services, and synchronization flows around order and commerce data.

As those workflows grew, operational issues became harder to diagnose. From the outside, a problem could simply look like an “AI order failed,” but the real failure could be in the **order flow, the tool call, or a synchronization path**. That meant QA or support could easily become dependent on a backend engineer to interpret what had happened.

I saw that as a trust and operability problem. I did not want other teams to rely on verbal explanations from whichever engineer happened to know the system best.

## Task

I took responsibility for making those failures **more transparent and actionable**.

My goal was to give the people supporting the system a shared operational view that helped them identify the failure area quickly, while keeping the backend as the source of truth for the actual system state.

## Action

I first looked at the recurring failure patterns and separated them into meaningful operational categories instead of treating every issue as a generic application error.

The main categories I worked around were **AI-order failures, tool-call errors, and synchronization issues**. That distinction mattered because each one sends an engineer down a different investigation path.

I then built a dashboard using **React, TypeScript, and React Query** to surface those categories in one place.

A key design choice was to make the dashboard reflect **deterministic backend and integration evidence**, rather than asking the LLM to generate an explanation for what it thought had happened. The model could help interpret a customer request, but operational truth had to come from the application and integration layers.

I organized the UI so that an engineer could quickly tell which boundary had failed and then go deeper into the relevant service or workflow. I intentionally avoided turning the dashboard into a dump of every internal log entry, because that would move the complexity instead of reducing it. The first view needed to answer: **what failed, where should I investigate next, and what evidence supports that direction?**

Because I was also working across the Java backend and AI tool-calling side, I could check whether what the dashboard showed was consistent with the underlying application behavior. I used representative failure cases from those different categories to verify that the dashboard pointed to the correct operational area.

The biggest change was in how I communicated incidents with other teams. Instead of saying, “I checked the backend and this is probably the issue,” I could point to the same operational evidence they could see. That made the discussion more objective and reduced unnecessary back-and-forth.

The trade-off was that building a curated operational view required additional frontend and integration work, and it would never replace deep service-level debugging. I accepted that because the purpose was not to replace engineering investigation; it was to make the **first diagnosis faster and shared**.

## Result

The dashboard work reduced **operational troubleshooting time by 45%** for AI-order failures, tool-call errors, and synchronization issues.

The more important behavioral result was that teams no longer had to depend as heavily on one engineer's explanation to understand where a failure had occurred. We had a common starting point backed by system evidence, which made incident discussions more factual and actionable.

## Learning

I learned that **trust grows when people can independently verify what the system is doing instead of relying on verbal reassurance**. I also learned that useful observability is not about exposing every detail; it is about exposing the evidence that helps someone make the next correct decision.

## Memory spine

**AI workflow failures hard to interpret → separate failure boundaries → build React/TypeScript dashboard → backend evidence, not model guesses → common operational view → representative validation → shared incident language → 45% lower troubleshooting time**

## Likely interviewer notes

- Recent **Birlasoft AI-commerce** incident
- Cross-layer operational complexity
- **AI-order / tool-call / synchronization** failure categories
- Built shared operational visibility
- **Evidence over verbal reassurance**
- React + TypeScript + React Query
- **45% lower troubleshooting time**
- Reduced dependency on individual engineers

## Key trade-off

**Raw logs only:** maximum detail, but high cognitive load and strong dependence on system experts.

**Curated operational dashboard:** additional implementation and maintenance, but faster triage and a shared evidence base.

I chose the second approach for first-line diagnosis while keeping deeper logs and service-level debugging available when needed.

## High-risk Bar Raiser follow-ups

### Why is this Earn Trust and not just a dashboard project?

The dashboard was the mechanism. The trust problem was that other teams had to depend on individual engineers to interpret failures. I made the system state more transparent so people could verify the same evidence themselves and have a more objective conversation about ownership and next steps.

### Why not just give everyone raw logs?

Raw logs are useful for deep debugging, but they require knowledge of several services and can overwhelm someone doing initial triage. I used the dashboard to identify the failure boundary quickly, then deeper logs remained available for the engineer investigating that specific area.

### Why did you separate order failures, tool-call failures, and synchronization issues?

Because they are different operational boundaries. A failed tool invocation, a backend order problem, and stale or failed synchronization can all appear as one broken AI workflow to the user, but they require different owners and debugging paths.

### Why not let the LLM summarize the failure?

I wanted the operational view to be based on deterministic system evidence. An LLM can help explain information, but it should not become the source of truth for whether a tool call executed, an order operation failed, or synchronization completed.

### How do you defend the 45%?

The **45% reduction in operational troubleshooting time** is the reported result from this dashboard work and is present on my resume. The exact baseline window and calculation method are not preserved in my current preparation notes, so I should recover that from the original project evidence before giving a more granular numerical explanation rather than invent one.

## Sentence to remember

**“I did not want other teams to trust my explanation because I knew the backend; I wanted them to have the same evidence I had.”**
