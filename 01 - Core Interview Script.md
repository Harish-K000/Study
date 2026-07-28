# Air Canada Interview — Final Spoken Answer Script

**Candidate:** Harish Kannan  
**Use this file for revision.** Every answer is written as something to say aloud to an interviewer. The other folders are background reference only.

> Do not memorize word for word. Remember the flow: direct answer, why it matters, your example, then stop and let the interviewer ask the next question.

---

# 1. Opening and Experience

## Tell me about yourself

I’m a backend-focused software developer with more than three years of hands-on development experience, primarily with Java, Spring Boot, REST APIs, microservices, SQL, testing, and cloud deployment practices. I also completed a Master of Applied Computing at Wilfrid Laurier University. In my current role at Siemens, I work on an industrial asset-monitoring platform. A simple way to understand it is as a health-monitoring system for machines: the platform receives signals such as temperature, vibration, pressure, and runtime and turns them into dashboards, alerts, and maintenance information. My work is mainly around backend APIs, validation, service integration, security, testing, CI/CD support, and production troubleshooting.

Previously, at Cognizant, I worked on Verizon’s e-commerce platform, mainly in the cart area. The cart service exchanged JSON with pricing, promotions, inventory, checkout, and other services. I implemented and debugged business rules and used ELK with correlation IDs to trace issues across the complete service flow. My strongest area is Java backend engineering, and I’m interested in Air Canada because its booking and travel systems require the same reliability, secure integrations, and production ownership that I have worked with in customer-facing systems.

**If they ask what your strongest area is:** My strongest area is backend development with Java and Spring Boot. I also understand the frontend request flow, deployment process, and production monitoring, which helps me work effectively with frontend, QA, and DevOps teams.

## Tell me about your work at Cognizant

At Cognizant, I worked as a backend developer on Verizon’s e-commerce platform using Java, Spring Boot, REST APIs, SQL, and microservices. My area was mainly the cart workflow, but the cart did not work alone. When a customer added a product, a JSON response travelled through services such as pricing, promotions, inventory, and checkout.

I explain that JSON like an order sheet moving through different departments. Each department reads the fields it needs, applies its own rules, and passes the updated information forward. In our cart service, values such as product type, bundle eligibility, promotion status, and optional fields decided which business path or method would run. My responsibility was to implement and maintain those rules, validate the data we received, handle missing values safely, write tests, and investigate production issues.

I also worked closely with QA, frontend, DevOps, and upstream service teams. Because it was a customer-facing transaction flow, performance, correct error handling, and the ability to trace one request across services were very important.

## What exactly did you personally implement at Cognizant?

I personally worked on Spring Boot service-layer logic, REST integrations, request validation, conditional cart and bundle rules, exception handling, database operations, unit tests with JUnit and Mockito, and production defect fixes. I also investigated why a particular method executed based on values received from another service.

For example, if a bundle flag or promotion value came back in the JSON, our service selected a specific business path. When the result was unexpected, I traced the payload, found the exact condition that triggered the method, reproduced it in a lower environment, and then either corrected our logic or raised an evidence-based ticket to the service that owned the unexpected value. I do not claim that I designed the entire Verizon platform; this was my contribution within the cart service and the wider team.

## Tell me about your Siemens project

At Siemens, I work on an industrial asset-monitoring platform used to monitor equipment such as motors, pumps, compressors, and conveyors. I usually compare it to a smartwatch for machines. A smartwatch collects signals and warns a person when something looks abnormal; this platform receives readings such as temperature, vibration, pressure, RPM, and runtime and turns them into machine status, alerts, trends, and maintenance information.

The overall platform includes ingestion, processing, storage, analytics, security, dashboards, and AWS-based deployment. My personal work is focused on the backend and application side: Spring Boot APIs, request validation, database and service integration, authentication and role-based access, automated testing, CI/CD support, and production troubleshooting. I always separate the full platform architecture from the parts I personally implemented.

## Explain one Siemens API you worked on

One API flow allowed an authenticated dashboard user to request information for a machine or its alerts. When the request reached the backend, we first validated the token and request parameters. Then we checked whether the device existed and whether the user’s tenant, plant, and role allowed access to that device. After that, the service retrieved the required data, applied the business rules, mapped it to a response object, and returned JSON to the dashboard.

The important point is that the backend did not trust the frontend. The frontend could request a device ID, but the backend remained responsible for validating existence, authorization, and the business rules before returning data.

## Tell me about legacy modernization at Siemens

Legacy modernization means gradually improving an older application that is tightly coupled and difficult to change, test, deploy, or scale. I compare it to an old building where lighting, heating, and security are all controlled by one electrical circuit. A small change in one area can affect the whole building. Modernization separates those responsibilities so each area can be changed and maintained more safely.

In the application, the team identified modules with clear business responsibilities, separated their logic, exposed controlled APIs between them, added independent tests, and packaged them so they could be released with less impact on unrelated functionality. My personal contribution was **[replace with the real module, API, tests, containerization, or pipeline work]**. I describe this as a gradual modernization effort, not as rewriting the complete platform or personally designing every microservice.

## What is environment provisioning, and how did you improve it?

Environment provisioning means preparing everything required for the application to run in a target environment. Before automation, that could involve building the application, configuring environment values, preparing the Docker image, connecting the service to its dependencies, deploying it, and validating it manually.

I compare it to preparing a new employee laptop. If someone installs and configures every tool manually, the process takes time and may be slightly different each time. An automated workflow performs the same approved steps consistently. We used GitHub Actions or Jenkins and Docker to automate the repeated build, test, packaging, deployment, and validation steps for the AWS-based environment. This reduced manual effort and made the process repeatable.

> Only say “nearly two hours to under twenty minutes” after confirming exactly what the timing included and where that measurement came from.

## Why Air Canada?

I’m interested in Air Canada because its digital systems directly affect the traveller’s journey, including searching, booking, payment, check-in, and notifications. That makes backend reliability meaningful. A failed or delayed transaction is not only a technical metric; it can directly affect someone’s trip.

My Cognizant experience was also in a customer-facing transaction system where different services had to work together correctly and production issues had to be investigated quickly. I would like to bring my Java, Spring Boot, API, testing, integration, and production-support experience into an environment where reliability, security, and scalability have a very visible customer impact.

## Why should we hire you?

I think my strongest fit is practical Java backend experience combined with production ownership. I can build and test Spring Boot APIs, understand how JSON and business state move through microservices, investigate failures using logs and correlation IDs, and work with upstream, downstream, QA, and DevOps teams without turning technical issues into blame.

I also understand that reliable delivery involves more than writing code. It includes validation, failure handling, testing, monitoring, and safe deployment. I can contribute with those skills immediately while continuing to learn Air Canada’s domain and architecture.

## Why are you leaving Siemens?

I value what I have learned at Siemens, especially working with enterprise backend services and operationally important systems. I’m exploring Air Canada because this position aligns closely with my Java and Spring Boot strengths and gives me the opportunity to work on highly visible customer-facing distributed systems. I’m moving toward a role and domain that strongly match my backend interests, rather than moving away from a negative situation.

---

# 2. Behavioral and Project Questions

## Tell me about a difficult production issue

In the Cognizant cart workflow, we had a case where a bundle-related method was executing unexpectedly. The difficult part was that our service did not make the decision independently. It reacted to values that had travelled through JSON responses from upstream services.

My responsibility was to find the exact point where the behavior changed. I used the transaction or correlation ID in ELK to reconstruct the request flow and inspect the payload our service received. I compared it with the expected contract and found the field and conditional branch that triggered the method. I then reproduced the case in a lower environment and added focused logging and tests around that decision point.

If our handling was wrong, I corrected the condition and tested the fix. If the payload did not match the agreed contract, I created a ticket for the owning team with the request, response, timestamp, correlation ID, expected value, and impact. The main lesson was that debugging a microservice issue means understanding the complete data journey, not only reading your own service’s code.

## Tell me about working with another team

In one cart issue, our service received a value that caused an unexpected bundle path to execute. Before contacting the upstream team, I verified our own logic first and traced the complete transaction in ELK. I documented the exact field, the expected contract, the method that executed, the timestamp, and the correlation ID.

When I contacted the other team, I did not say that they had simply sent the wrong response. I explained that the payload did not match the expected contract and shared the evidence. That kept the discussion technical and objective, reduced back-and-forth, and helped us identify the correct owner and resolution without blaming anyone.

## Tell me about a performance improvement

A checkout workflow included several backend operations that were increasing the response time. I first reviewed the flow to understand which operations depended on each other and which calls were independent. Where the business rules allowed it, independent calls could be handled concurrently instead of waiting for each one sequentially.

I helped refactor that part of the workflow, improved the related timeout and exception handling, and validated the change through unit, integration, and performance testing. The important point was that we did not make everything asynchronous just to claim better speed. We kept dependent steps in order and controlled the thread usage and failure behavior. The result was a more responsive checkout flow under the measured test conditions.

> Before the interview, replace the final sentence with the real measured result and the actual tool or report. Do not quote a percentage unless you can explain the baseline.

## Tell me about a disagreement

I try to bring a disagreement back to the shared requirement and technical evidence. In one case, there were different views about whether validation in the frontend was enough. I agreed that frontend validation was useful because it gives the user immediate feedback, but I explained that it could be bypassed and therefore could not be the authoritative security or data-integrity layer.

We reviewed the API contract and failure cases together and agreed to keep the user-friendly checks in the frontend while enforcing the critical rules again in the backend. That solution protected the data without reducing the user experience. The disagreement was resolved by focusing on the requirement rather than on whose idea was correct.

## Tell me about a mistake

Use a real example and say it this way:

I made a mistake while working on **[specific task]** when I **[state the mistake clearly]**. I discovered it through **[test, review, or monitoring]** and informed the affected people instead of trying to hide it. I corrected the immediate issue, checked whether it had affected any related flow, and added **[a test, validation, checklist item, or review step]** so the same type of mistake would be detected earlier in the future. The lesson for me was **[specific lesson]**, and after that I changed my process by **[specific action]**.

Do not use “I work too hard” or another disguised strength.

## Tell me about a tight deadline

In a tight-deadline situation, my first step is to clarify the minimum safe scope and the dependencies rather than trying to do everything at once. In **[real example]**, I broke the work into the critical path and optional improvements, raised the main risks early, and coordinated with QA and the dependent teams so testing could begin as soon as each stable part was ready.

I protected the essential validation, code review, and rollback steps instead of removing them to save time. We delivered **[real outcome]**, and the experience taught me that early risk communication and clear prioritization are more reliable than depending only on overtime.

## Tell me about difficult feedback

When I received feedback that **[real feedback]**, I first asked for a specific example so I could understand the behavior rather than react defensively. I changed **[specific behavior or process]**, asked for a follow-up review, and used **[real evidence]** to check whether I had improved. The feedback helped me become better at **[communication, testing, estimation, code quality, or another real area]**.

## Tell me about learning a technology quickly

I needed to learn **[real technology]** for **[specific deliverable]**. I started with the official documentation and built a small isolated example so I could understand the basic behavior without risking the main application. Then I implemented the smallest useful part, asked someone familiar with the system to review it, and validated it through tests and monitoring.

That approach helped me deliver **[real result]** while still being honest about what I knew and what needed review. I prefer learning through a small verified implementation rather than reading a large amount of material without applying it.

## Tell me about something you did independently

For a university deployment project, I reviewed the deployment requirements, containerized the application, configured the repository and GitHub Actions workflow, and set up the deployment to the server. I also documented the process so another person could repeat and maintain it. I worked independently on the implementation, but I still validated the result through testing and clear documentation rather than treating independent work as work without review.

## Describe a project you are most proud of

One project I am proud of is my work on the Cognizant cart workflow because it combined business logic, service integration, performance, testing, and production responsibility. The cart did not simply store products; it had to understand pricing, promotions, bundle eligibility, availability, and downstream checkout behavior.

My contribution was in the Spring Boot service logic and integration flow. I worked on the conditional rules, handled optional values defensively, wrote tests, and investigated issues across services using ELK and correlation IDs. What made the work meaningful was that the result directly affected a customer’s checkout experience. It taught me to think beyond one method or one service and understand the full transaction.

## Tell me about working with QA

I treat QA as part of the engineering process, not as the team that only finds defects after development. For **[real feature or defect]**, I reviewed the acceptance criteria and failure scenarios with QA before final testing. When QA reported unexpected behavior, I used the request details, environment, timestamps, and correlation ID to reproduce it and determine whether the issue was in our code, test data, configuration, or a dependency.

After the fix, I shared the affected paths and asked QA to verify both the original scenario and related regression cases. This reduced repeated handoffs and helped us confirm the business behavior rather than only checking that the exception disappeared.

## Tell me about deploying a feature

For a production feature, my responsibility was to make sure the code was not only complete but ready to operate. After implementation and code review, the pipeline built the application, ran tests and checks, created the versioned Docker image, and deployed it through the approved release process. We validated the health endpoint and critical smoke scenarios and watched error rate, latency, and business transactions during the rollout.

For higher-risk changes, the team used controlled strategies such as blue-green or canary deployment. If the health checks or monitored behavior became worse, we stopped the rollout or returned traffic to the stable version. I describe the deployment strategy as a team practice unless I personally configured the traffic rules.

## Tell me about handling multiple priorities

When I have several priorities, I first separate customer-impacting incidents, committed release work, and lower-risk improvements. I confirm deadlines and dependencies with the relevant people instead of deciding silently. In **[real example]**, I handled **[urgent issue]** first because it affected **[customer or release impact]**, then divided the remaining work into clear deliverables and communicated what could realistically be completed.

I kept stakeholders updated when the situation changed and avoided starting many tasks without finishing the critical path. This helped the team protect the most important outcome without hiding the trade-offs.

## Tell me about communicating a technical issue to a non-technical stakeholder

I begin with the customer or business impact, not the internal class or exception. For example, instead of saying that a downstream JSON field selected the wrong branch, I would explain that a small set of bundle orders was following the wrong promotion path, that we had identified the affected workflow, and that the team was validating the correction.

I keep the technical evidence available for engineers, but for a non-technical stakeholder I explain what is affected, what is not affected, the current mitigation, the next update, and any decision I need from them. That keeps the communication accurate without overwhelming them with implementation details.

---

# 3. Spring Boot — Spoken Answers

## What is Spring Boot?

Spring Boot is built on top of the Spring Framework and helps us create production-ready Java applications more quickly. It provides starter dependencies, auto-configuration, and an embedded server such as Tomcat, so we do not have to configure every common framework component manually.

In my projects, we used Spring Boot to build REST APIs and microservices because it let us focus on the business logic, such as cart rules or device access, instead of spending time wiring the basic application setup. Spring Boot does not replace Spring; it gives Spring sensible defaults, and we can still override them when needed.

## What is MVC?

MVC separates the application into clear responsibilities. In a Spring Boot REST service, the controller handles the HTTP request and response, the service contains the business logic, and the repository handles database access. The data moves through request and response objects, and the view is normally the JSON returned to the client.

For example, in the cart flow, the controller received the add-to-cart request, the service applied bundle and promotion rules and called the required services, and the repository retrieved or saved cart data. I think of it like a restaurant: the waiter takes the request, the kitchen applies the recipe, and the store room manages the ingredients. One person should not do every job.

## What happens when a request reaches Spring Boot?

When a request reaches Spring Boot, the web server passes it to the `DispatcherServlet`, which is Spring MVC’s front controller. It finds the correct controller method based on the URL and HTTP method. Spring then binds the path values, headers, parameters, or JSON to Java objects and performs the configured validation.

The controller calls the service, the service applies authorization and business rules, and the repository uses JPA and Hibernate to access the database. The result is mapped to a response object, Jackson converts it to JSON, and the response goes back to the client. I remember it as one flow: request, controller, service, repository, database, and JSON response.

## What is `DispatcherServlet`?

`DispatcherServlet` is the front controller in Spring MVC. Every incoming web request goes through it, and it decides which controller method should handle that request. It is like a receptionist: the receptionist does not perform the specialist’s work but directs each request to the correct person and coordinates the response.

## What is IoC?

IoC means that instead of creating application objects myself every time with `new`, I let Spring create and manage them. For example, if my `CartService` needs a `CartRepository` and a `PromotionClient`, the service does not construct those objects itself. Spring creates them, manages their lifecycle, and makes them available where they are needed.

This is useful because manually creating and connecting many controllers, services, repositories, and clients would make a large application tightly coupled and difficult to test.

## What is dependency injection?

Dependency injection is how Spring gives a class the objects it depends on. For example, `CartService` depends on `CartRepository`. I declare that dependency in the constructor, and Spring injects the repository when it creates the service.

The class receives what it needs instead of building everything itself. That makes the dependency clear and also makes unit testing easier because I can provide a mock repository during the test.

## What is a Spring bean?

A Spring bean is simply an object that Spring creates and manages. When Spring finds a class such as a controller, service, repository, or component during startup, it can create the object, inject its dependencies, manage its lifecycle, and reuse it wherever required.

The connection I remember is: IoC means Spring controls object creation, the managed objects are beans, and dependency injection connects those beans together.

## Why constructor injection?

I prefer constructor injection because it makes the required dependencies visible and mandatory when the object is created. It also lets me keep the fields final and makes testing easy because I can construct the class with mocks. Field injection hides the dependencies inside the class and usually makes isolated testing more difficult.

## What is Maven?

Maven is the tool we use to manage Java dependencies and automate the build. It reads the `pom.xml`, downloads the required libraries, compiles the code, runs the tests, and packages the application as a JAR or WAR.

I think of the `pom.xml` as the project’s ingredient list. A developer’s machine and the CI server can use the same file to build the application consistently. In our pipeline, Maven created and tested the JAR, and that artifact was then used to build the Docker image.

## What happens during `mvn clean install`?

`clean` removes the previous build output so we start fresh. Maven then compiles the main code, compiles and runs the tests, packages the application, performs the configured verification steps, and installs the final artifact into the local Maven repository. In a CI pipeline, `verify` may be enough if another local project does not need the artifact installed.

## What is JPA, and what is Hibernate?

JPA is the Java specification that defines how Java objects should be mapped to relational database tables. Hibernate is a common implementation that actually performs that work by generating SQL, mapping rows to objects, tracking entity changes, and communicating with the database. Spring Data JPA sits on top and gives us repository interfaces such as `JpaRepository`.

The easiest way I remember it is: JPA defines the contract, Hibernate implements it, and Spring Data JPA removes repetitive repository code.

## What is the persistence context?

The persistence context is where Hibernate keeps track of the entities it is managing during a transaction. If the same entity is requested again in that context, Hibernate can return the same managed object instead of creating another one. It also watches changes to managed entities and can write those changes to the database when the transaction is flushed.

I compare it to having a file already open on my desk. If I need the same file again, I use the open copy instead of going back to the archive.

## Lazy loading versus eager loading

Lazy loading means related data is retrieved only when it is needed, while eager loading retrieves it immediately with the parent. I do not treat one option as always better.

For example, a machine could have thousands of sensor readings. If an API only needs the machine name and status, loading every reading would be wasteful. I prefer to design the query for the endpoint using a projection, fetch join, or another explicit approach rather than making every relationship eager.

`## What is `@Transactional`?

`@Transactional` makes a group of database operations one unit of work. If all the operations succeed, the database changes commit together. If an unchecked exception occurs, Spring normally rolls them back. I usually keep the transaction boundary in the service layer because that is where the business operation is coordinated.

One important limitation is that a local database transaction cannot undo a payment that already succeeded in another service. For a cross-service workflow, we need idempotency and compensating actions rather than assuming one annotation can control the complete distributed system.`

---

# 4. REST APIs and Security — Spoken Answers

## How would you design a production-quality REST API?

I would start with clear resource-oriented endpoints and the correct HTTP methods. I would validate request payloads, return consistent response objects and status codes, use centralized exception handling, and secure the API with authentication and authorization.

For production, I would also include pagination where needed, idempotency for sensitive operations such as payment or booking creation, structured logging with correlation IDs, metrics, API documentation, unit tests, integration tests, and failure-case testing. My goal is not only to make the successful request work, but also to make invalid input, duplicate requests, timeouts, and dependency failures predictable.

## What is a REST API?

A REST API is an HTTP interface that lets a frontend or another service work with backend resources. The client sends a request to an endpoint, the backend validates the caller and the data, applies the business rules, accesses the database or other services, and returns a response, normally as JSON.

I compare it to a controlled bank counter. The customer does not enter the vault or update the ledger directly. The counter checks identity, permission, and the requested operation before returning only the allowed information.

## Why should React not access the database directly?

The frontend is not a trusted authority. If it accessed the database directly, credentials and data would be exposed and the client could bypass validation, authorization, business rules, and auditing. The backend is the controlled layer that checks whether the data exists, whether the user has permission, and whether the requested operation is valid.

## GET, POST, PUT, PATCH, and DELETE

I use `GET` to retrieve data, `POST` to create a resource or start an operation, `PUT` to replace a complete resource, `PATCH` for a partial update, and `DELETE` to remove a resource. I also think about idempotency. A repeated `GET` or `PUT` should have a predictable result, while a payment `POST` needs an idempotency key so a retry does not create a duplicate charge.

## Authentication versus authorization

Authentication verifies who the user is. Authorization decides what that authenticated user is allowed to do. At an airport, a passport verifies identity, while the boarding pass or employee role decides whether the person can enter a flight or restricted area.

## What is JWT?

A JWT is a signed token containing claims about the authenticated user. The client sends it with requests, and the backend validates the signature, issuer, audience, and expiry before trusting the claims and applying authorization.

I would not say the JWT payload is encrypted by default. It is encoded and signed, so I do not put sensitive secrets inside it and I never log the full token.

## How do you validate a request?

I validate at more than one level. Basic request validation checks required fields, formats, and ranges. The service then checks business rules such as whether the device or cart exists and whether the caller is allowed to access it. Database constraints provide final protection for important integrity rules.

Frontend validation is useful for user experience, but it can be bypassed, so the backend remains the authoritative layer.

## How do you handle exceptions in Spring Boot?

I use meaningful custom exceptions for domain problems and handle them centrally with `@RestControllerAdvice`. The handler maps them to a consistent response containing an error code, a safe message, a timestamp, and a correlation ID.

I do not expose stack traces, database details, or internal class names to the client. Those technical details stay in the server logs, where they can be used for investigation.

## How would you secure a backend API?

I would use HTTPS for data in transit, OAuth or JWT-based authentication where appropriate, and role-based authorization at the endpoint and service levels. I would validate every input, use parameterized queries, store secrets outside the source code, apply least-privilege permissions, and avoid logging tokens, passwords, card data, or other sensitive information.

I would also consider rate limiting, dependency scanning, audit logs, secure error responses, and encryption for sensitive stored data. Security is not one annotation; it has to be applied across the complete request and deployment flow.

---

# 5. Java Core — Spoken Answers

## Heap versus stack

Each thread has its own stack, which contains method calls, local primitive values, and local references. The heap is shared and contains the actual objects and their instance data. When a method finishes, its stack frame is removed automatically. Objects on the heap remain until they are no longer reachable and the garbage collector can reclaim them.

For `Device device = new Device()`, the local reference is in the current stack frame, while the `Device` object is on the heap.

## Is Java pass-by-reference?

Java is always pass-by-value. When I pass an object to a method, Java copies the value of the reference. Both references can point to the same object, so the method can change that object, but if the method assigns its local parameter to a different object, it does not change the caller’s variable.

## `==` versus `equals()`

For primitives, `==` compares the values. For objects, `==` checks whether the references point to the same object. `equals()` checks logical equality when the class implements it that way. For Strings, `equals()` compares the characters.

If I override `equals()`, I also need a consistent `hashCode()` so the object behaves correctly in `HashMap` and `HashSet`.

## Why are Strings immutable?

Strings are immutable, which means their value cannot change after creation. That makes them safe to share in the String pool, gives them a stable hash code for use as map keys, and makes them easier to use safely across threads. It is also important because Strings are commonly used for URLs, class names, configuration, and security-related values.

If I am repeatedly building a String, especially inside a loop, I use `StringBuilder` so I do not create many temporary String objects.

## ArrayList versus LinkedList

`ArrayList` uses a dynamic array, so indexed reads and iteration are efficient. `LinkedList` uses connected nodes, so inserting or removing can be efficient when I already have the position, but finding that position still requires traversal.

In most backend work, I use `ArrayList` much more often because we normally read, iterate, and append data. For an Air Canada passenger list, `ArrayList` would normally be the practical choice. For queue behavior, I would usually choose `ArrayDeque` rather than selecting `LinkedList` automatically.

## How does HashMap work?

`HashMap` stores key-value pairs in buckets. It uses the key’s `hashCode()` to find a bucket and then uses `equals()` to find the correct key inside that bucket. That is why lookup is O(1) on average, but not guaranteed in every case because collisions can occur.

I also avoid using a mutable object as a key if the fields used by `equals()` and `hashCode()` can change, because the map may no longer be able to find the key after it is inserted.

## HashMap versus ConcurrentHashMap

`HashMap` is not safe for concurrent updates. `ConcurrentHashMap` provides thread-safe access without locking the complete map for every operation, so it supports better concurrency.

I would still use atomic operations such as `computeIfAbsent` for compound changes. Also, `ConcurrentHashMap` only shares state inside one JVM. If the service runs in multiple Kubernetes Pods, every Pod has its own map, so shared business state should be in Redis or a database.

## Checked versus unchecked exceptions

Checked exceptions must be caught or declared at compile time and often represent recoverable external conditions such as file or network I/O. Unchecked exceptions extend `RuntimeException` and normally represent programming errors, invalid arguments, or domain failures.

In Spring Boot services, I prefer meaningful custom exceptions and central handling rather than catching a generic `Exception` in every controller.

## What is a Stream?

A Stream lets me process collection data as a pipeline using operations such as `filter`, `map`, and `sorted`. Those intermediate operations are lazy, and the processing begins when a terminal operation such as `toList`, `count`, or `findFirst` is called.

I use Streams when they make the transformation clearer. I do not force complex business logic with many side effects into one long stream chain just to make the code shorter.

## What is Optional?

`Optional` represents a value that may be present or absent. I commonly use it for repository lookups, where I can either use the result or throw a meaningful not-found exception with `orElseThrow`.

I avoid calling `get()` without checking and do not use `Optional` for every field or parameter, because that can make the model more complicated without adding value.

## What is multithreading?

Multithreading allows multiple threads in the same process to make progress concurrently. Threads share heap memory, so shared mutable data can cause race conditions, visibility problems, and deadlocks if it is not handled carefully.

In enterprise applications, I use controlled thread pools and higher-level tools such as `CompletableFuture` rather than creating unlimited threads. More threads do not automatically make the application faster; the pool size and design depend on whether the work is CPU-bound or waiting on I/O.

## How did you use asynchronous processing?

In the cart workflow, some downstream operations, such as pricing, promotion, or availability checks, could be independent. If they were called sequentially, the total response time included the wait for every service. Where the business dependencies allowed it, independent calls could run concurrently and the results could be combined before returning the final cart response.

The main consideration was not only speed. We also needed controlled thread usage, timeouts, error handling, and clear fallback behavior. We did not make dependent steps asynchronous simply to claim a performance improvement.

---

# 6. Database and Performance — Spoken Answers

## What is an index?

An index is a database structure that helps the database find rows without scanning the complete table. I compare it to the index at the back of a book: instead of reading every page, I can jump to the relevant location.

Indexes improve read performance, but they also use storage and make inserts, updates, and deletes more expensive because the index must be maintained. That is why I create indexes based on real query patterns instead of indexing every column.

## How do you optimize a slow API?

I avoid optimizing based only on assumptions. I first use metrics, logs, and traces to identify whether the delay is coming from application logic, a database query, an external service, connection waiting, or infrastructure limits.

If the database is the slow layer, I inspect the execution plan, index usage, join conditions, result size, repeated queries, and N+1 behavior. I also check whether the API is returning more data than it needs. If an external call is slow, I review timeouts, call count, and whether independent calls can safely run concurrently.

After making the change, I repeat the same test under comparable conditions and compare latency percentiles, throughput, error rate, and resource usage. I want to prove that the bottleneck improved without moving the problem somewhere else.

## What is `EXPLAIN`?

`EXPLAIN` shows how the database plans to execute a query. It helps me see whether the database is using an index or performing a full table scan, how tables are joined, how many rows are expected, and which operations are expensive. I use it to understand the real query plan instead of assuming that an index or query change will help.

## What is connection pooling?

Opening a new database connection for every API request is expensive. A connection pool keeps a limited set of connections ready. The request borrows one, performs the database work, and returns it to the pool.

Spring Boot commonly uses HikariCP. I would size the pool based on the application workload and the database capacity, because a very large pool can overload the database rather than improve performance.

## When would you use Redis?

I would use Redis for frequently read data with a clear freshness rule, short-lived sessions, rate-limit counters, idempotency records, or distributed coordination. Before adding a cache, I define the key, TTL, invalidation behavior, and what happens if Redis is unavailable.

For a flight system, search results may be cached, but final seat availability must always be checked with the inventory source of truth because stale cache data could cause an incorrect booking decision.

## Explain SQL joins

An inner join returns only rows that match in both tables. A left join returns every row from the left table and any matching row from the right, with null values when there is no match. A right join does the opposite, and a full outer join returns rows from both sides whether or not they match.

For example, if I need only customers who have bookings, I use an inner join. If I need every customer and any booking they may have, I use a left join. I choose the join based on the business result I need, not only the table structure.

## What is normalization?

Normalization organizes relational data to reduce duplication and update anomalies. In first normal form, values are atomic and rows are identifiable. Second normal form removes partial dependency on part of a composite key, and third normal form removes dependencies between non-key columns.

In practice, I normalize transactional data for correctness and maintainability, but I may use a carefully designed denormalized read model for reporting or high-volume reads when the performance need is proven.

## Primary, foreign, and composite keys

A primary key uniquely identifies a row. A foreign key enforces a relationship to a key in another table. A composite key uses more than one column to identify a row. For example, a seat inventory record may be uniquely identified by a combination such as flight instance and seat number, although the exact schema depends on the booking model.

## Clustered versus non-clustered index

A clustered index determines how the table’s rows are organized at the storage level, so a table normally has only one clustered ordering. A non-clustered index is a separate lookup structure that points to the underlying row. The exact behavior and terminology vary by database, so I confirm it for the database being used rather than treating every engine as identical.

## What are transaction isolation levels?

Isolation levels control how much one transaction can observe another transaction’s work. Read committed prevents dirty reads. Repeatable read also keeps a row read consistently during the transaction, although phantom behavior depends on the database. Serializable provides the strongest isolation by making concurrent work behave as if it ran one transaction at a time, but it has the highest concurrency cost.

I choose the level based on the consistency requirement and expected contention. For seat inventory, I would combine the database’s concurrency controls with an atomic update or version check rather than assuming the default isolation level alone prevents every double-booking case.

## Optimistic versus pessimistic locking

Optimistic locking assumes conflicts are uncommon. A version value is checked when updating, and if another transaction changed the row first, the update fails and the application can retry or return a conflict. Pessimistic locking acquires a database lock before changing the row and makes competing transactions wait.

Optimistic locking usually supports better concurrency, while pessimistic locking can help for short, highly contested operations. The choice depends on conflict frequency, transaction duration, and the customer experience during retries or waits.

## What is a database deadlock?

A deadlock occurs when two transactions each hold a lock the other needs, so neither can continue. The database normally detects the cycle and aborts one transaction. I reduce the risk by updating resources in a consistent order, keeping transactions short, indexing queries so fewer rows are locked, and handling the database’s retryable deadlock exception safely.

---

# 7. Microservices, Kafka, and Reliability — Spoken Answers

## What is a microservice?

A microservice is an independently deployable service responsible for one business capability. In the Cognizant platform, cart, pricing, promotions, inventory, and checkout had separate responsibilities and communicated through APIs and JSON.

This can improve team ownership, independent deployment, scaling, and fault isolation, but it also introduces network failures, contract dependencies, distributed debugging, security, and data-consistency challenges. I would not say microservices are always better than a monolith; the business boundaries and scaling needs must justify the added complexity.

## How do microservices communicate?

For synchronous communication, services usually use REST or gRPC when the caller needs an immediate response. For asynchronous communication, they use messaging or event platforms such as Kafka or RabbitMQ when the work should be decoupled, buffered, retried, or processed independently.

In my Cognizant work, my strongest hands-on experience was REST and JSON-based service integration and asynchronous Java workflows.

## What if one service goes down?

I would start with strict connection and response timeouts so request threads do not wait indefinitely. For a transient and safe operation, I may use a small number of retries with exponential backoff and jitter. A circuit breaker prevents the application from repeatedly calling an unhealthy dependency, and bulkheads help prevent one dependency from consuming all resources.

The fallback depends on the business function. If a recommendation service is unavailable, the checkout may continue without recommendations. If payment or final booking confirmation is unavailable, the application should fail safely rather than return a false success. I would also make the failure visible through logs, metrics, traces, and alerts.

## What is Kafka?

Kafka is a distributed event-streaming platform. Producers publish events to topics, topics are divided into partitions for scale and ordering within a partition, and consumers read the events through consumer groups. Kafka is useful when services need to be decoupled, events need to be retained or replayed, and the system has a high-throughput event flow.

My strongest production experience is asynchronous Java processing and service integration rather than Kafka platform administration. I understand the main concepts and how I would apply them, but I would be honest about that distinction.

## Kafka versus RabbitMQ

Kafka is generally a better fit for durable event streams, high throughput, retention, and replay. RabbitMQ is commonly used for queue-based message delivery, acknowledgements, and flexible routing. I would choose based on the business need rather than saying one is always better.

## How do you avoid processing the same message twice?

I design the consumer to be idempotent. The event can contain a unique event ID, and the consumer can use a database uniqueness constraint or processed-event record so the same business action is not applied twice.

I also commit or acknowledge the message only after successful processing. I do not rely only on the messaging platform to guarantee that the business operation can never be repeated.

---

# 8. Java and Spring Rapid-Fire Follow-ups

## What are the SOLID principles?

SOLID is a set of design principles that helps keep object-oriented code maintainable. Single Responsibility means a class should have one clear reason to change. Open/Closed means behavior should be extendable without repeatedly modifying stable code. Liskov Substitution means an implementation must honor the contract of the type it replaces. Interface Segregation means clients should not depend on methods they do not need. Dependency Inversion means high-level business logic should depend on abstractions rather than concrete details.

The principles I use most visibly in Spring code are Single Responsibility and Dependency Inversion. A controller handles HTTP, a service handles business logic, and a repository handles persistence. The service depends on a repository interface, and Spring injects the implementation.

## Which design patterns have you used?

I use patterns when they make the design clearer, not only to name them. Spring beans are singleton-scoped by default within one application context. A factory centralizes the decision about which implementation to create. A builder helps construct a complex object without a long constructor. Strategy lets me select one of several business behaviors behind the same interface, such as email, SMS, or push notification. Observer-style event handling lets interested components react to an event without the producer calling each one directly.

For example, a notification service could select an `EmailStrategy` or `SmsStrategy` through a common interface. That keeps the booking logic independent from the delivery-specific code.

## What happens after you run `java Main`?

The JVM starts, the class-loader system locates and loads the required classes, bytecode is verified, and the runtime creates the required memory areas such as thread stacks and the heap. The execution engine interprets or just-in-time compiles frequently used bytecode into native machine code. The JVM begins the `main` method, and the garbage collector manages unreachable heap objects while the application runs.

The JVM gives Java portability because the application is compiled to bytecode and each supported platform has a JVM that executes that bytecode.

## How does garbage collection work?

Java garbage collection reclaims heap objects that are no longer reachable. Most collectors use a generational idea because many objects become unreachable quickly. New objects usually begin in a young area; surviving objects may eventually move to an old area. A young or minor collection focuses mainly on newer objects, while a major or full collection involves more of the heap and is generally more expensive.

Some phases require application threads to pause, often called stop-the-world pauses, although modern collectors try to reduce pause duration through concurrent work. I normally investigate GC through JVM metrics and logs rather than manually calling `System.gc()`.

## What is the String pool?

The String pool allows identical String literals to share one object. If I create `String a = "ABC"` and `String b = "ABC"`, both references normally point to the same pooled literal. `new String("ABC")` explicitly creates a separate String object even though the text is equal.

That is why `a == b` can sometimes be true for literals, but I still use `equals()` to compare String content because reference identity is not the same as logical equality.

## Comparable versus Comparator

`Comparable` defines the natural ordering inside the class through `compareTo`, such as ordering a flight by its flight number. `Comparator` defines an external or alternative ordering, such as sorting flights by departure time, price, or duration.

I use `Comparator` when the same type needs several orderings or when I should not modify the class. I also make sure comparison is consistent enough for the collection or sort behavior being used.

## HashMap, LinkedHashMap, and TreeMap

`HashMap` provides average constant-time lookup and does not guarantee iteration order. `LinkedHashMap` maintains insertion order, or access order when configured, with a small additional cost. `TreeMap` keeps keys sorted and normally provides logarithmic operations because it uses a balanced tree.

I choose based on whether I need fast lookup, predictable iteration, or sorted keys. I do not use `TreeMap` merely because sorted output looks convenient if sorting once at the boundary is simpler.

## HashSet, TreeSet, Queue, Deque, and PriorityQueue

`HashSet` stores unique values with no guaranteed order. `TreeSet` stores unique values in sorted order. A queue normally models first-in, first-out processing. A deque supports adding and removing at both ends, and `ArrayDeque` is a good general implementation. `PriorityQueue` returns elements according to priority rather than insertion order.

For an airline example, normal check-in requests may use queue semantics, while an internal scheduler may use a priority queue when urgent operational work must be considered first.

## Explain thread safety: `synchronized`, `volatile`, and `AtomicInteger`

`synchronized` provides mutual exclusion and visibility around a protected block or method, so only one thread holding the same monitor executes it at a time. `volatile` makes the latest value visible across threads but does not make a compound operation such as `count++` atomic. `AtomicInteger` provides thread-safe atomic operations for a counter without a manually written synchronized block.

For more flexible locking, Java also provides `Lock` implementations with operations such as timed acquisition. My first preference is to avoid unnecessary shared mutable state and then use the simplest correct concurrency tool.

## What causes a Java memory leak?

A Java memory leak occurs when objects are no longer useful but remain reachable, so the garbage collector cannot reclaim them. Common causes include static collections that grow without limits, caches without eviction, listeners that are never removed, `ThreadLocal` values not cleared in reused threads, and resources retained by long-running tasks.

I investigate a suspected leak using heap and GC metrics, heap dumps, and object-retention analysis rather than assuming every high-memory condition is a leak.

## What are functional interfaces?

A functional interface has one abstract method and can be implemented with a lambda. `Predicate<T>` accepts a value and returns a boolean, `Function<T,R>` transforms one value into another, `Consumer<T>` accepts a value without returning a result, and `Supplier<T>` produces a value.

For example, a Stream `filter` uses a predicate, while `map` uses a function. I use them when they make the intention clearer and avoid hiding complex business behavior inside a difficult lambda.

## What are the common Spring stereotype annotations?

`@RestController` handles REST requests and writes response bodies, while `@Controller` is commonly used for MVC views. `@Service` marks business logic, `@Repository` marks persistence components and participates in exception translation, and `@Component` is the general stereotype.

These annotations all make a class eligible for component scanning, but the more specific annotation communicates the class’s role to the team.

## `@Bean`, `@Configuration`, and component scanning

`@Configuration` marks a class that defines Spring configuration. An `@Bean` method explicitly creates an object for Spring to manage, which is useful for third-party classes or construction that needs custom logic. Component scanning discovers annotated application classes in the configured packages.

I use component scanning for my own controllers and services and `@Bean` when I need to configure an external client, executor, mapper, or another object that I do not annotate directly.

## `@RequestBody`, `@PathVariable`, and `@RequestParam`

`@RequestBody` maps the HTTP request body, normally JSON, to a Java object. `@PathVariable` reads a value that identifies a resource in the URL path, such as `/devices/{deviceId}`. `@RequestParam` reads a query parameter, such as pagination, filtering, or sorting options.

I use path variables for resource identity and request parameters for optional query behavior, while keeping more complex input in a validated request DTO.

## `@Value`, `@Qualifier`, and `@Primary`

`@Value` can inject a configuration value, although I prefer type-safe configuration properties for groups of related settings. `@Primary` marks the default bean when several implementations match, while `@Qualifier` requests a specific implementation.

If several implementations exist, I make the selection explicit and tied to a real design rule rather than resolving ambiguity accidentally.

## What is Spring Boot auto-configuration?

Spring Boot auto-configuration creates common framework beans based on what is available on the classpath, existing configuration, properties, and conditional rules. For example, when the web starter is present, Spring Boot configures the common MVC infrastructure and an embedded server; when JPA and a data source are present, it configures the common persistence components.

It does not configure everything blindly. The configuration is conditional, and application-defined beans or properties can override the defaults.

## What are Spring bean scopes?

Singleton is the default and creates one bean per Spring application context. Prototype creates a new instance each time Spring supplies the bean. Request scope creates one instance for an HTTP request, and session scope creates one for a user session.

A Spring singleton is not global across the whole distributed system. Every application instance or Kubernetes Pod has its own application context and its own singleton.

## What is Spring Boot Actuator?

Actuator exposes production endpoints for application health, metrics, information, and operational integration. Common endpoints include health, info, and metrics, and Micrometer can expose data in a Prometheus-compatible form when configured.

I expose only the endpoints needed for operations, secure sensitive endpoints, and separate liveness and readiness behavior. Actuator is a source of operational data; the monitoring platform still needs to collect, visualize, and alert on it.

## How do you log in a Spring Boot application?

I normally write through the SLF4J API, with Logback commonly providing the implementation in Spring Boot. I use `TRACE` and `DEBUG` for detailed diagnostics, `INFO` for important normal events, `WARN` for unexpected but recoverable situations, and `ERROR` for failures that require attention.

I use structured fields and correlation IDs, avoid logging the same exception repeatedly at every layer, and never log passwords, tokens, card data, or unnecessary personal information.

## What is API versioning?

API versioning allows an interface to evolve without immediately breaking existing clients. A common approach is a path such as `/api/v1/bookings`, although header or media-type versioning is also possible.

I version only when there is a breaking contract change and support a clear deprecation and migration plan. I do not create a new version for every internal implementation change.

## What is CORS?

CORS is a browser security mechanism that controls whether a page from one origin may call a server at another origin. The server returns headers that allow specific origins, methods, and headers, and the browser may send a preflight `OPTIONS` request first.

CORS is not authentication. I configure only the required trusted origins and methods instead of using a broad wildcard with credentials.

## How do you implement pagination?

For Spring Data, I can accept a `Pageable` and return a `Page` when the client needs the content and total count. A `Slice` tells the client whether another page exists without always running an expensive total-count query. Limit and offset are simple but can become slow or inconsistent for very large, changing datasets, where cursor or keyset pagination may be more appropriate.

I also use a stable sort key so records do not move unpredictably between pages.

## How do you implement caching in Spring?

Spring’s caching abstraction allows methods to use annotations such as `@Cacheable`, while a provider such as Redis stores the data. Before enabling it, I define the cache key, TTL, eviction or invalidation behavior, and failure behavior.

I do not cache correctness-critical final decisions such as confirmed seat ownership without validating the source of truth. A cache improves performance only when its freshness rules are clear.

## OAuth 2.0 versus JWT

OAuth 2.0 is an authorization framework that defines flows for a client to obtain delegated access. JWT is a token format that can carry signed claims. An OAuth access token may be a JWT, but OAuth and JWT are not the same thing.

In a typical API flow, the authorization server authenticates the user and issues an access token. The client sends the token in the Authorization header, a Spring Security filter validates it, and the application then applies role or scope-based authorization.

## What is serialization, and what does Jackson do?

Serialization converts a Java object into a transferable format such as JSON, and deserialization converts JSON back into a Java object. Spring Boot commonly uses Jackson for this conversion.

Annotations such as `@JsonProperty` can define the external field name and `@JsonIgnore` can exclude a field, but I prefer explicit request and response DTOs so the API contract is not accidentally tied to a JPA entity.

## Swagger versus OpenAPI

OpenAPI is the specification used to describe an HTTP API, including endpoints, inputs, responses, schemas, and security. Swagger refers to a set of tools around that specification, such as interactive documentation.

I use API documentation to make the contract clear to frontend, QA, and integration teams, but I still verify that the documented schema matches the behavior through tests.

## What is SonarQube?

SonarQube performs static code analysis and reports issues such as bugs, security concerns, duplication, maintainability problems, and test coverage. A CI quality gate can prevent promotion when agreed standards are not met.

Coverage is only one signal. I care more about whether business-critical branches, validation, failures, and integrations are tested than about increasing a percentage with low-value tests.

## Git merge, rebase, revert, reset, cherry-pick, stash, and squash

Merge combines histories and records a merge when appropriate. Rebase replays commits onto a new base to create a linear history, so I avoid rebasing shared published history without team agreement. Revert creates a new commit that undoes an earlier commit and is safe for shared branches. Reset moves a branch reference and can discard local work, so I use it carefully. Cherry-pick applies selected commits, stash temporarily stores local changes, and squash combines commits before integration.

I follow the team’s branch and review policy rather than choosing commands only for a cleaner-looking history.

## How do you work in Agile?

In sprint planning, the team agrees on a realistic goal and selected work. Refinement clarifies upcoming stories, dependencies, and acceptance criteria. Daily stand-up is for coordination and blockers, not a status report to one manager. The retrospective identifies improvements to the team’s process.

Story points represent relative effort, complexity, and uncertainty rather than exact hours. Definition of Done means the work meets the team’s agreed quality conditions, such as review, testing, documentation, and deployment readiness.

## Explain the SDLC

The software development lifecycle moves from understanding the requirement through design, implementation, testing, deployment, monitoring, and continued maintenance. In practice, those steps are iterative rather than a single one-way sequence.

As a backend developer, I contribute beyond coding by clarifying acceptance criteria, reviewing design and API contracts, writing tests, supporting deployment, monitoring the release, and applying what production incidents teach us to the next cycle.

## Spring profiles and external configuration

Spring profiles allow environment-specific beans or configuration to be activated, while properties or YAML files define settings in a structured form. I keep environment-specific values outside the code and allow deployment-time environment variables or a secrets manager to supply sensitive configuration.

I never commit passwords, private keys, or production secrets into `application.properties`, YAML files, or the repository.

---

# 9. AWS, CI/CD, Docker, Kubernetes, and Monitoring

## What is AWS Lambda?

AWS Lambda is a serverless compute service that runs code when an event triggers it, without requiring us to manage the underlying server. For example, EventBridge, an S3 upload, an API request, or a queue message can trigger a Lambda function.

In the Siemens story, an event-driven alert or notification task could run after sensor processing so the main API did not have to wait for every background step. If the Lambda used a container image, ECR stored the image and Lambda ran it. Lambda is not the AWS CLI, and Lambda does not store the Docker image.

## Lambda versus EC2

I would use Lambda for short-running, stateless, event-driven work where automatic scaling and reduced server management are useful. I would use EC2 or a container runtime for long-running APIs, continuous background services, or workloads that need more runtime and infrastructure control.

## Explain the AWS services on your resume

I explain each service by the problem it solves. EC2 provides virtual servers when the application needs operating-system and runtime control. Lambda runs short event-driven functions without managing servers. S3 stores objects such as files and raw data. RDS provides a managed relational database for SQL and transactions. DynamoDB is a managed NoSQL database designed around key-based access patterns. API Gateway is a managed entry point for APIs. ECR stores Docker images. IAM controls identities, roles, and permissions. EventBridge routes events between producers and targets, and CloudWatch provides AWS-native metrics, logs, dashboards, and alarms.

I would not list all of these as personal production expertise. I would identify which ones the platform used and then explain the subset I personally integrated with.

## ECR, ECS, EKS, and Lambda — how do they connect?

Amazon ECR is the image registry; it stores Docker images. ECS runs and manages containers using AWS’s container orchestration service. EKS is AWS’s managed Kubernetes service. Lambda can also run a compatible container image for an event-driven function.

I remember it like a delivery flow: GitHub Actions or Jenkins builds the package, ECR is the warehouse that stores it, and ECS, EKS, or Lambda is the runtime that opens and runs it. Long-running Spring Boot APIs normally belong on a long-running container or server runtime, while Lambda is a better fit for bounded event-driven work. I only name the runtime that was actually used by the component.

## Explain your CI/CD flow

We used GitHub Actions or Jenkins for CI/CD and AWS as the target cloud environment. After code was merged, Maven compiled the application, ran the tests, and created the JAR. The pipeline then performed the configured quality and security checks, built a versioned Docker image, and pushed it to Amazon ECR.

From ECR, the image was deployed to the verified AWS runtime. After deployment, we ran health checks and smoke tests and monitored the release before full promotion. The important distinction is that Jenkins or GitHub Actions controlled the pipeline, Docker packaged the application, ECR stored the image, and AWS ran it.

> Only say ECS, EKS, EC2, or Lambda if you know the real runtime used by that component.

## What is Docker?

Docker packages the application, runtime, and dependencies into a consistent image so the same package can run in development, testing, and production. The image is the blueprint, and the container is a running instance of that image.

It solves the common problem where an application works on one machine but fails in another environment because the runtime or dependencies are different.

## What is Kubernetes?

Kubernetes manages containerized applications. It handles deployment, scaling, service discovery, health checks, rolling updates, and replacement of failed Pods. Docker packages and runs a container; Kubernetes manages many container instances as a system.

I understand Kubernetes from the application side—Deployments, Pods, Services, configuration, probes, scaling, and troubleshooting—but I would not describe myself as a cluster-platform administrator.

## Liveness versus readiness

A liveness probe checks whether the application process is stuck and should be restarted. A readiness probe checks whether the application is currently ready to receive traffic. An application can be alive but not ready while it is starting, draining, or temporarily unable to serve safely.

I would not make the liveness check depend on every external service, because one dependency outage could restart all healthy application instances and make the incident worse.

## Blue-green versus canary deployment

Blue-green deployment keeps the stable version and the new version in separate environments. We deploy and validate the new version, then switch traffic. If the application has a problem, traffic can return quickly to the stable version.

Canary deployment sends only a small percentage of traffic to the new version first. We watch error rate, latency, resource health, and important business transactions, and increase the traffic gradually if the version is healthy. Blue-green gives a fast switch and rollback path, while canary reduces the number of users exposed during validation.

## How do you monitor an application on AWS?

I monitor infrastructure, application, and business health. At the infrastructure level, I look at CPU, memory, disk, network, and container or instance health. At the application level, I monitor traffic, latency, error rate, JVM memory, thread pools, database pool usage, and downstream calls. At the business level, I monitor outcomes such as successful bookings, payment failures, delayed alerts, or cart failures.

Spring Boot Actuator can expose health and metrics, application logs include correlation IDs, and CloudWatch can collect AWS metrics, logs, dashboards, and alarms. At Cognizant, we used ELK mainly for centralized log search and tracing requests across services.

## Logs versus metrics versus traces

Logs show detailed events, such as an exception or a business decision for one request. Metrics show numerical behavior over time, such as traffic, latency, error rate, CPU, and memory. Distributed traces show how one request travelled across services and where it spent time or failed.

I use all three together. A metric tells me that errors increased, a trace shows which service path failed, and the logs provide the detailed reason.

## How do you investigate a production issue?

I first assess the customer impact and stabilize the service. Then I check recent deployments, dashboards, error rate, latency, resource saturation, logs, and traces. I use a correlation ID to follow the request across services and determine whether the problem is in the application, database, infrastructure, configuration, or an external dependency.

If needed, I stop the rollout, redirect traffic, or follow the approved rollback process. After recovery, I document the root cause and add the appropriate prevention step, such as a test, alert, validation rule, or runbook update.

---

# 10. Testing — Spoken Answers

## How do you test a Spring Boot application?

I test the application in layers. Unit tests use JUnit and Mockito to test business logic with dependencies mocked. Controller tests verify request validation, JSON handling, and HTTP status codes. Repository tests verify database queries and mappings. Integration tests confirm that multiple components work together and that important external contracts behave as expected.

I do not test only the successful path. I include invalid input, missing data, timeouts, dependency failures, duplicate requests, and boundary cases. Coverage is a useful indicator, but a high percentage does not prove that the important behavior is tested.

## What is Mockito?

Mockito lets me replace a real dependency with a controlled test double. For example, I can test `CartService` with a mocked pricing client and repository, define their responses, and verify how the service behaves without calling a real database or remote service.

I use mocks for isolation, but I still need integration tests because a mock cannot prove that the real SQL, serialization, network contract, or configuration works.

## What is TDD?

In TDD, I first write a test that describes the required behavior and see it fail. Then I implement the minimum code required to make it pass and refactor while keeping the tests green. This is the red, green, refactor cycle.

I would not claim that every line of every project was developed with strict TDD unless that is true, but I understand and use the approach where it helps clarify behavior.

---

# 11. Flight Booking System — Spoken Answer

## Design a flight-booking backend

I would first clarify the scope: flight search, fare and availability, seat hold, booking, payment, confirmation, and cancellation, along with expected traffic and consistency requirements.

At a high level, I would separate search, inventory, booking, payment, customer, and notification responsibilities. Search is read-heavy and can use a cache or optimized read model, but final seat availability must always be checked with the inventory source of truth.

The most critical operation is preventing two customers from booking the same seat. The inventory service would create a short-lived seat hold using an atomic database update, a version check, and a database constraint. The booking service would create a pending booking and call payment using an idempotency key. If payment succeeds, the booking is confirmed and an event is published for notification and downstream systems. If payment fails or the hold expires, the seat is released according to the workflow.

I would also include timeouts, retries only where safe, circuit breakers, monitoring, audit history, and reconciliation for uncertain payment states.

## How do you prevent double booking?

The inventory source of truth must change the seat from available to held atomically. Two requests may arrive at the same time, but only one conditional update should succeed. A uniqueness constraint provides final protection.

I would not use only an in-memory Java lock because the booking service may run on multiple instances, and each instance would have a different lock.

## What if payment succeeds but the response times out?

A timeout does not prove that payment failed. The client should retry with the same idempotency key, and the booking service should return the existing result instead of charging again. If the state is uncertain, a reconciliation process should query the payment provider and move the booking to the correct state.

## Why can’t one `@Transactional` method handle the complete booking?

`@Transactional` can control the local booking database transaction, but it cannot roll back a payment that already succeeded in another system or change another service’s database. The complete flow is a distributed transaction, so I would use a saga-style workflow with clear states, idempotent operations, durable events, retries, and compensating actions such as releasing a seat or refunding a payment.

## What if the confirmation email fails?

The booking and the email are separate outcomes. Once the booking is confirmed, the service should publish a durable `BookingConfirmed` event. The notification service can retry the email independently and send unrecoverable failures to a dead-letter path. A failed email should not cancel a valid paid booking.

---

# 12. Honest Answers for Gaps

## Have you used Kafka?

My strongest production experience is with asynchronous Java workflows and REST-based service integration. I understand Kafka topics, partitions, consumer groups, offsets, replay, and idempotent consumer design, but I would not describe myself as a production Kafka administrator. The event-driven and failure-handling concepts are familiar to me, and I am comfortable deepening the implementation knowledge.

## How much AWS experience do you have?

I have application-level experience working with AWS services and AWS-based deployments. I understand how services such as EC2, Lambda, S3, ECR, RDS, EventBridge, IAM, and CloudWatch support an application, but my deepest expertise is Java backend development rather than cloud infrastructure architecture. I distinguish clearly between integrating an application with AWS and owning the complete infrastructure.

## How much Kubernetes experience do you have?

I understand Kubernetes from the application and deployment perspective, including Pods, Deployments, Services, configuration, health probes, scaling, and release troubleshooting. I would describe my experience as practical application-level experience rather than cluster administration.

## Do you have airline-domain experience?

I have not worked directly in the airline domain. My related experience is in customer-facing e-commerce workflows involving cart, checkout, payment, service integration, and production reliability. Those concerns transfer well to flight search, booking, payment, and notification systems, and I am comfortable learning the airline-specific rules.

## Have you used MuleSoft?

I have not used MuleSoft directly in production. However, I have worked with REST APIs, service integrations, request and response transformation, authentication, error handling, and enterprise backend systems. Those integration fundamentals are transferable, and I am comfortable learning the platform.

---

# 13. Resume Metric Defense

For every percentage or number on the resume, be ready to say:

We measured **[metric]** using **[actual dashboard, report, CI result, load test, or deployment record]** under **[conditions]**. The original value was **[baseline]**. My specific change was **[your work]**. Under comparable conditions, the result became **[new value]**, which was approximately **[percentage]** improvement.

Before the interview, verify:

- 22% page-load improvement
- 54% to 81% test coverage
- deployment or provisioning time
- 35% backend response improvement
- transaction volume
- availability or uptime
- defect reduction

If you cannot explain the source, do not present the figure as an exact personal result.

---

# 14. Questions to Ask the Interviewers

1. What types of backend systems or customer workflows would this developer work on first?
2. Is the current architecture primarily Java-based, Node.js-based, or a combination?
3. What reliability or modernization challenges are most important to the team now?
4. What would success look like during the first three to six months?

---

# 15. Last-Minute Speaking Rules

- Answer the question directly in the first sentence.
- Give one project example and then pause.
- Use an analogy only when it makes the answer easier to understand.
- Say “I” for your work and “we” for team architecture or outcomes.
- Never invent a metric, AWS service, deployment runtime, or ownership claim.
- Say “the payload did not match the expected contract,” not “the other team gave the wrong response.”
- Say “complex conditional business rules and defensive null handling,” not “too many if statements.”
- Say “GitHub Actions or Jenkins ran the CI/CD pipeline and deployed to AWS,” not “AWS is CI/CD.”
- If you do not know something, explain the closest experience honestly and show that you understand the transferable fundamentals.
- A simple story you can defend is stronger than a large architecture you cannot.

---

# Final 15-Minute Revision Order

1. Tell me about yourself.
2. Cognizant project and personal contribution.
3. Cognizant production issue.
4. Siemens project and personal contribution.
5. Why Air Canada?
6. IoC, dependency injection, and bean connection.
7. Spring request flow.
8. REST API and security.
9. Slow API investigation.
10. Microservice failure handling.
11. CI/CD and AWS deployment flow.
12. Flight booking, double booking, and idempotent payment.
13. Disagreement, mistake, and tight deadline.
14. Resume metric evidence.
15. Questions for the interviewers.
