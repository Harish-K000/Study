# Story 7 — Earn Trust: Making the AI System Easier to Operate After Handover

**Status:** Approved core; handover framing tightened on 2026-11-12.  
**Primary LP:** Earn Trust  
**Target question:** Tell me about a time you had to earn the trust of another team or make it easier for others to rely on your work.

> Evidence boundary: The Birlasoft dashboard work and the reported **45% reduction in operational troubleshooting time** are grounded in the resume. The project was described as a small R&D-style team handing the solution over with KT to another team that was newer to the AI/tool-calling architecture. The exact historical logging product and the exact before/after measurement window are not preserved in the current notes. Use **existing centralized application logs / operational data** unless the specific logging platform is confirmed.

## Situation

At Birlasoft, I was part of a relatively small R&D-style team building an AI-enabled commerce solution using Java/Spring Boot, Spring AI, MCP and tool calling.

The goal was not just to finish the implementation. We also had to **hand the solution over to another team and provide KT** so they could maintain and troubleshoot it after our work was complete.

That receiving team was newer to the AI and tool-calling side of the architecture. We already had normal engineering mechanisms such as documentation, Jira and application logs, but I saw a practical handover problem: if every failure required someone to know which AI service, MCP tool, backend API or synchronization path to inspect first, the receiving team would still depend heavily on the original developers.

## Task

I wanted to reduce that dependency.

My goal was to give the receiving team **one project-level operational starting point** for common AI-commerce failures, while keeping the existing logs as the place for detailed root-cause investigation.

## Action

I first separated the responsibilities of the tools we already had.

**Jira** was useful for defect ownership and tracking. **Application logs** were useful for deep debugging once someone knew what request or component to investigate. Neither by itself gave a new engineer a simple first view of the AI workflow.

So I worked on a lightweight operational dashboard using **React, TypeScript and React Query**.

I organized the view around the failure areas that mattered to the project: **AI-order failures, tool-call errors and synchronization issues**. The purpose was not to copy every log line into the UI. It was to surface enough context for first-level triage — what operation failed, where in the workflow it failed, when it happened, and the available request/correlation identifier or error context.

The underlying failure evidence came from the application's existing operational information and centralized logs through the backend. The dashboard remained a controlled application view rather than becoming a replacement logging platform.

That created a clear troubleshooting path for the receiving team:

**start with the dashboard → identify the failing workflow/component → use the request or correlation identifier to drill into the detailed application logs → create or update Jira for ownership and resolution when necessary.**

I also made this operational flow part of the **KT and handover**, rather than handing over only code and static documentation. The point was to teach the new team how to reason about the system when it was unhealthy, not only how the happy path worked.

I considered the simpler option of relying only on documentation, Jira and raw logs. That had almost no additional implementation cost, but it preserved more tribal knowledge because a new engineer still had to know where to begin. At the other extreme, introducing a much larger observability platform specifically for this R&D scope would have added more infrastructure and operational overhead than we needed.

The lightweight project-specific dashboard was the middle ground: some additional frontend/backend integration work, but a much simpler first-level operating model for the team receiving the system.

## Result

The dashboard work reduced **operational troubleshooting time by 45%** across the AI-order, tool-call and synchronization issues covered by the work.

The broader result I cared about was the handover quality. The receiving team had a common place to begin an investigation and a defined path from project-level visibility to detailed logs, rather than having to search old Jira tickets or depend immediately on the original R&D developers for context.

## Learning

I learned that a strong handover includes **operational context, not only code and documentation**. I also learned that observability should reduce the amount of system knowledge someone needs before they can take the first correct troubleshooting step.

## Memory spine

**Small R&D AI team → handover to team newer to AI → logs/Jira/KT alone still require tribal knowledge → lightweight React operational view → show failure boundary + request/correlation context → drill into existing logs → include workflow in KT → 45% lower troubleshooting time**

## Likely interviewer notes

- Small R&D team with **handover responsibility**
- Receiving team newer to AI/MCP/tool-calling architecture
- Identified **operability / tribal-knowledge** risk
- React + TypeScript + React Query dashboard
- Existing logs retained for deep diagnosis
- Dashboard used for **first-level triage**
- KT included troubleshooting workflow, not just code walkthrough
- **45% lower troubleshooting time**

## Key trade-off

**Only docs + Jira + raw logs:** lowest implementation cost, but a new team needs more architectural knowledge before it can even start investigating.

**Large observability/APM expansion:** richer capabilities, but greater infrastructure and operational overhead than this R&D handover required.

**Lightweight project dashboard over existing operational data:** some implementation/maintenance cost, but enough domain-specific visibility to reduce dependence on the original team.

## High-risk Bar Raiser follow-ups

### Why was the dashboard necessary if you already had logs?

Logs were still the right place for detailed debugging. The dashboard solved the earlier step: helping someone unfamiliar with the architecture identify which AI/tool/backend/synchronization boundary to investigate before searching the logs.

### Did the dashboard replace Jira?

No. Jira remained the system for tracking defects, ownership and resolution. The dashboard was the operational starting point; once an issue needed engineering work, Jira still handled the normal workflow.

### Where did the dashboard data come from?

The historical notes establish existing application/operational data and logs but do **not** preserve the exact logging product. The safe historical answer is that the backend exposed the relevant failure information from the project's existing operational/logging data; do not name CloudWatch as the actual source unless it is confirmed.

### Why not expose raw logs directly in React?

The dashboard needed a stable, controlled view of the fields relevant to first-level triage, not unrestricted log access or every implementation detail. Detailed logs remained available through the normal logging/access path when deeper investigation was required.

### Why is this Earn Trust?

I was preparing another team to own a system they had not built and whose AI/tool-calling architecture was newer to them. I wanted them to be able to verify the system state and begin troubleshooting from shared evidence instead of having to trust the original developers' verbal explanations.

### How do you defend the 45%?

The **45% reduction in operational troubleshooting time** is the reported result for this dashboard work and is present on my resume. The exact baseline window and calculation method are not preserved in the current preparation notes, so recover that evidence before giving a more granular numerical breakdown.

## Sentence to remember

**“I did not want to hand over only working code; I wanted to hand over enough operational context that the next team could support the system without depending on the people who built it.”**
