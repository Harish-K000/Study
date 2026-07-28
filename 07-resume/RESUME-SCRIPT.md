# Resume Script

[Master index](../README.md) · [Enterprise Java playbook](../FINAL-INTERVIEW-HANDBOOK.md) · [Answer book](../FINAL-INTERVIEW-ANSWER-BOOK.md) · Previous: [Behavioral](../06-behavioral/README.md) · Next: [Question map](../08-question-maps/QUESTION-MAP.md)

> Accuracy note: keep only statements you can defend with real implementation details. Replace bracketed items with your actual values and tools. The system description is broader than your personal ownership.

## 60–90 second introduction

I am a backend-focused software developer with more than three years of experience using Java, Spring Boot, REST APIs, microservices, SQL, automated testing, and cloud deployment practices. At Siemens, I work on an industrial asset-monitoring platform where the broader system collects machine telemetry and turns it into dashboards, alerts, and maintenance insights. My personal contribution is focused on Spring Boot APIs, validation, service integration, security, testing, CI/CD support, and production troubleshooting. Previously at Cognizant, I worked on an e-commerce cart service that exchanged JSON with pricing, promotion, inventory, and checkout services. I implemented and debugged rule-driven backend flows and used ELK and correlation IDs to trace production issues. I am interested in Air Canada because its customer-facing booking and travel systems need the same qualities I value in backend work: reliability, secure integrations, clear operational visibility, and safe delivery.

## Siemens project overview

At Siemens, I work on an industrial asset-monitoring platform. A simple way to understand it is as a health-monitoring system for machines: just as a smartwatch observes heart rate and warns about abnormal patterns, the platform receives measurements such as temperature, vibration, pressure, RPM, and runtime and turns them into status, alerts, and maintenance information. The overall platform includes ingestion, processing, analytics, dashboards, security, and cloud deployment. My own work is centered on [only list verified areas: Spring Boot services, REST APIs, validation, database integration, OAuth/JWT/RBAC, tests, CI/CD integration, and incident debugging].

## Siemens API story

One API flow allowed an authenticated dashboard user to request machine or alert information. The backend validated the JWT and request parameters, verified that the device existed, checked that the user’s tenant and plant permissions allowed access, queried the required data, mapped it to a response DTO, and returned JSON. The frontend was never trusted as the authority; existence, authorization, and business rules were enforced by the backend.

## Siemens Lambda story

For event-driven work, a sensor or processing event could be routed through EventBridge to Lambda. The Lambda function performed bounded background logic such as evaluating an alert condition or initiating notification work, so the synchronous ingestion API did not have to wait for every downstream step. Lambda was the event-driven runtime; if it used a container package, ECR stored the image. My role was [integrating/publishing the event/supporting the function—choose the truthful wording], not designing the whole AWS platform.

## Siemens CI/CD story

We used GitHub Actions or Jenkins to orchestrate CI/CD and AWS as the target environment. After a merge, Maven compiled the application, resolved dependencies, and ran tests. The pipeline performed quality and security checks, built a versioned Docker image, pushed it to Amazon ECR, deployed it to the selected AWS runtime, and ran health or smoke validation before promotion. This made releases repeatable and reduced manual error. If I quote the change from nearly two hours to under twenty minutes, I will identify the exact manual steps included in the baseline and the report or deployment records used to measure the result.

## Siemens production support

For an incident such as delayed alerts, missing sensor data, API timeouts, or memory pressure, I first determined customer and plant impact. I compared CloudWatch infrastructure metrics with application latency and error rates, checked recent deployments, and traced relevant requests or events through structured logs. I isolated whether the bottleneck was the application, database, queue/event flow, or downstream dependency. If immediate stabilization was required, I coordinated rollback or traffic isolation; after recovery, I documented the root cause and added the appropriate test, alert, or code correction.

## Siemens legacy modernization

Legacy modernization means improving an older, tightly coupled application so it is easier to change, test, deploy, and scale. It is like separating an old building’s single electrical circuit into independent circuits: work on one section no longer requires shutting down everything. In the project, the team identified modules with clear business responsibilities, separated their logic and interfaces, added independent tests, packaged them for isolated deployment, and updated the delivery pipeline. My personal contribution was [specific module, APIs, tests, containerization, or pipeline work].

## Cognizant project overview

At Cognizant, I worked on the cart area of a large e-commerce platform. When a customer added a product, a JSON model moved across services such as cart, pricing, promotions, inventory, and checkout. The JSON was like an order sheet moving through departments: each service read relevant fields, applied its responsibility, and added or returned information. In the cart service, bundle eligibility and promotions depended on upstream fields, so the business logic contained multiple conditional paths and defensive handling for optional values.

## Cognizant debugging story

We had a case where a bundle-related method executed unexpectedly. Because our service reacted to fields from upstream responses, the visible problem did not prove where the defect originated. I searched ELK using the transaction or correlation ID, reconstructed the payload received by our service, compared it with the expected contract, and identified the exact field and condition that selected the branch. I reproduced it in a lower environment and added focused logging or tests around the decision point. If our logic was wrong, I corrected and tested it; if the payload violated the contract, I raised an evidence-backed ticket with the request, response, timestamp, expected value, and correlation ID. This prevented blame and helped the correct team resolve the problem quickly.

## Cognizant microservices answer

Our cart service owned cart-related rules but collaborated with pricing, promotion, inventory, and checkout services through HTTP and JSON. This separation allowed teams to deploy and scale business capabilities independently, but it also introduced network failures, contract dependencies, and distributed debugging. We used timeouts, appropriate error handling, logs, and correlation IDs to keep failures visible and traceable.

## Cognizant asynchronous processing

Where pricing, promotion, or availability calls were independent, they could be executed concurrently and combined before the final cart response. The goal was to reduce waiting time without changing business dependencies. We controlled thread usage and considered timeouts, partial failures, and fallbacks; we did not create unlimited threads or make dependent steps asynchronous merely for speed.

## Cognizant blue-green and canary deployment

For production-critical changes, the team used controlled rollout strategies. Blue-green deployment kept the stable and new versions available and switched traffic after validation, giving a fast rollback path. Canary deployment exposed the new version to a small percentage of traffic and expanded only when error rate, latency, resource health, and critical cart/checkout transactions remained stable. I describe this as a team deployment practice unless I personally configured the traffic rules.

## Metric defense template

For every resume number, prepare:

- Baseline: what was measured before?
- Change: what did you personally modify?
- Measurement: which dashboard, report, test, or pipeline record?
- Comparison: same load, environment, and period?
- Result: exact or approximate?
- Attribution: what other team changes influenced it?

Use:

> We measured [metric] using [source] under [conditions]. The baseline was [value]. I changed [specific work]. After validating under comparable conditions, the metric became [value], which was approximately [percentage] improvement.

Do not claim “millions of events,” availability percentages, test-coverage changes, or downtime reductions unless you can defend their source.

## Honest skill-gap answers

### Kafka

My strongest production experience is asynchronous Java workflows and service integration. I understand Kafka topics, partitions, consumer groups, offsets, replay, and idempotent consumers, but I would not describe myself as a production Kafka administrator.

### AWS

I have hands-on application-level exposure to AWS services and integrations, while my deepest experience is Java backend development. I can explain how our services used EC2/ECR/Lambda/EventBridge/RDS or the verified subset, but I distinguish application integration from infrastructure ownership.

### Kubernetes

I understand Pods, Deployments, Services, configuration, probes, scaling, and controlled releases. My experience is application deployment and troubleshooting rather than cluster-platform administration.

### Airline domain

I have not worked directly in the airline domain, but e-commerce cart, checkout, payment, integration, and reliability concerns transfer well to search, booking, payment, and notification workflows.
