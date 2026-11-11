# Story 6 — Dive Deep: CRM API Performance Investigation

**Status:** Approved for rehearsal on 2026-11-11.  
**Primary LP:** Dive Deep  
**Target question:** Tell me about a time you had to dive deep to identify the root cause of a difficult technical problem.

> Evidence boundary: Cognizant CRM performance work and the reported **41% response-time improvement** are grounded in the resume. The exact API, SQL shape, index, and root-cause sequence below are a rehearsal reconstruction unless later tied back to specific historical evidence.

## Situation

At Cognizant, I was working on a **Java and Spring Boot CRM platform** used for workflows such as lead creation, updates, deduplication, and conversion.

As the amount of CRM data increased, one of the frequently used **lead-search APIs** started showing noticeably higher response times. The API was still returning correct results, so initially it looked like a general performance problem rather than a functional defect.

Because that API was part of a workflow sales users accessed frequently, I wanted to understand the **actual bottleneck** instead of immediately adding infrastructure or caching around it.

## Task

My responsibility was to **identify where the latency was coming from**, improve the API without changing its business behavior, and make sure that any optimization would still preserve filtering, sorting, pagination, and data correctness.

## Action

I started by establishing a **baseline** for the slow request rather than changing code immediately.

I reproduced the same search using representative filters and broke the request time into the major stages: the controller and service processing, the database interaction, and the response mapping.

That comparison showed that most of the time was being spent in the **database query path**, not in Spring request processing or a downstream service.

I then looked at the SQL generated for that operation and examined how the database was executing it.

The query was filtering leads using several commonly used fields and then sorting the results by the most recently updated records. As the table had grown, the database was examining and sorting considerably more rows than the API actually needed before returning one page of results.

That was the key finding: the problem was not simply “the database is slow.” The **access pattern of the query no longer matched the way the data was being retrieved**.

At that point I considered three approaches.

The first was **caching the API response**. That could improve repeated reads quickly, but lead information changes frequently, so I would then have to manage invalidation and accept some amount of stale data. More importantly, caching would hide the inefficient query rather than remove it.

The second option was increasing database capacity. That could give us more headroom, but it would increase resource consumption while leaving the inefficient access pattern unchanged.

The third option was to **optimize the query and indexing first**, which was the direction I chose.

I reviewed which fields were actually required by the API instead of loading more entity data than the response needed. I reduced unnecessary data retrieval and made sure pagination was applied at the database level rather than processing a much larger result set in the application.

I then looked at the fields that were repeatedly used together for filtering and sorting and added an **index aligned with that access pattern** rather than adding indexes blindly to individual columns.

I was also careful with that decision because indexes are not free. They improve particular read patterns, but they consume storage and add work to inserts and updates. Since this was a read-heavy API and the access pattern was common, the read benefit justified that additional write cost.

After changing the query path, I reviewed the Spring service as well to make sure we were not performing unnecessary repeated database lookups while mapping the result.

I deliberately treated caching as a **secondary optimization**, not the first fix. For relatively stable reference information, caching could still be useful, but I did not want the correctness of frequently changing CRM data to depend on a stale cached search result.

For verification, I checked more than latency.

I reran the functional scenarios for **filtering, sorting, pagination, null values, and normal lead retrieval** because an optimized query that returns different business results is not an optimization.

Then I reran the performance test using the **same type of request mix** so that the before-and-after comparison was meaningful rather than comparing unrelated workloads.

## Result

Across the critical CRM workflows included in this performance work, we achieved a **41% reduction in average response time**.

The important part for me was that the improvement came from identifying and removing work in the **actual bottleneck** rather than masking the symptom with additional infrastructure.

The API continued to preserve its expected filtering, sorting, and pagination behavior during regression validation, while the optimized database access path reduced the time required to serve those requests.

## Learning

I learned to **measure where the time is actually being spent before choosing an optimization**. I also learned that caching and additional capacity are useful tools, but I should first remove unnecessary work when the underlying access pattern is the real bottleneck.

## Memory spine

**Slow CRM API → establish baseline → isolate DB time → inspect SQL/access path → query + index problem → reject cache-first approach → optimize query/index → regression test → same workload → 41% improvement**

## Likely interviewer notes

**Production-facing CRM API → measured bottleneck → database investigation → query/index decision → cache trade-off → protected correctness → 41% response-time reduction**

## Sentence to remember

**“I didn't start by adding cache or capacity; I first proved where the latency was coming from and removed work at the bottleneck.”**
