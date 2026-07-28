# Enterprise Java Interview Playbook

This playbook is written as a conversation with a senior backend engineer. Every answer is designed to create the next natural follow-up.

Use the answers as speaking patterns. Understand the connection and then use your own words.

## How to use the handbook

For every question:

1. Start with the exact interview answer.
2. Stop after 45–60 seconds.
3. Let the interviewer choose a follow-up.
4. Answer the follow-up using the connected section.
5. Add the production example only when it supports the question.

## The answer-depth rule

Real interviews move in layers:

```text
30-second answer
        ↓
Interviewer shows interest
        ↓
60–90 second explanation
        ↓
Interviewer asks how or why
        ↓
Technical depth, code, and production example
```

Start concise. Do not give the deepest answer before the interviewer asks for it.

> **Air Canada interview note:** connect technical decisions to reliability, customer impact, secure transactions, and production support. Do not introduce Kafka, Kubernetes, or another technology unless it solves the problem being discussed.

---

# Interview Chain 1: MVC → DispatcherServlet → IoC → DI → Bean

## What is MVC?

### Interview Answer

MVC is a way of separating responsibilities in the application. In a Spring Boot REST service, the controller receives the HTTP request and returns the response, the service contains the business logic, and the repository communicates with the database. The model or DTO carries the data between these layers, and the response is normally returned as JSON. I use this separation because I do not want HTTP handling, business conditions, and database code inside the same class. It makes the code easier to maintain and test.

### Why this answer is correct

It explains the purpose rather than only expanding the letters Model, View, and Controller. It also describes how MVC is normally used in an enterprise Spring REST application.

### Production example

In the Cognizant cart application, the controller received the add-to-cart request. The service applied cart, bundle, pricing, and promotion rules. The repository handled cart persistence. That separation allowed us to test business rules without calling the real HTTP endpoint or database.

### Simple analogy

The controller is the restaurant waiter, the service is the kitchen applying the recipe, and the repository is the storage area managing ingredients.

### One-line revision

> Controller handles HTTP, service handles business logic, and repository handles data access.

---

## What happens when a request reaches Spring Boot?

### Interview Answer

Suppose the frontend calls `GET /devices/101`. The request reaches the web server and then Spring MVC’s DispatcherServlet. During startup, Spring already scanned annotations such as `@RestController` and `@GetMapping` and created handler mappings. The DispatcherServlet uses those mappings to find the correct controller method. Spring converts the path value and request data into Java parameters, the controller calls the service, and the service calls the repository or other services. The result is mapped into a response DTO, Jackson converts it to JSON, and the response travels back through the DispatcherServlet to the client.

### Request flow

```text
Client
  ↓
Tomcat
  ↓
DispatcherServlet
  ↓
Controller
  ↓
Service
  ↓
Repository
  ↓
Database
  ↓
DTO → Jackson → JSON
```

### Enterprise code

```java
@RestController
@RequestMapping("/api/v1/devices")
public class DeviceController {
    private final DeviceService deviceService;

    public DeviceController(DeviceService deviceService) {
        this.deviceService = deviceService;
    }

    @GetMapping("/{deviceId}")
    public ResponseEntity<DeviceResponse> getDevice(
            @PathVariable long deviceId) {
        return ResponseEntity.ok(
                deviceService.getDevice(deviceId));
    }
}
```

### One-line revision

> DispatcherServlet receives the request, finds the controller, and coordinates the response.

---

## What is DispatcherServlet?

### Interview Answer

DispatcherServlet is the front controller in Spring MVC. Every matching HTTP request reaches it first. It does not contain our business logic. Its job is to find the correct controller method, coordinate request binding and validation, invoke the handler, and return the final response. I compare it to a receptionist who receives every visitor and sends each person to the correct department.

### Common mistake

Do not say DispatcherServlet performs database operations or contains the business logic.

### One-line revision

> DispatcherServlet is Spring MVC’s receptionist and request coordinator.

---

## How is the controller object created?

### Interview Answer

The controller is not normally created manually using `new`. When the application starts, Spring scans the application packages. When it finds an annotation such as `@RestController`, it creates that controller object and manages it inside the Spring container. The same process happens for classes marked with `@Service`, `@Repository`, or `@Component`.

This naturally leads to IoC, because Spring has taken responsibility for creating and managing those application objects.

---

## What is IoC?

### Interview Answer

IoC means that instead of creating application objects myself every time using `new`, I allow Spring to create and manage them. For example, if `DeviceService` needs `DeviceRepository`, I do not create the repository inside the service. Spring creates both objects, manages their lifecycle, and connects them. This makes the application loosely coupled and easier to maintain. IoC is the overall concept, and Spring implements that concept mainly through dependency injection.

### Why this answer is correct

It explains:

- What control was transferred
- Why the transfer is useful
- A small service/repository example
- The connection to DI

### Without IoC

```java
DeviceRepository repository =
        new SqlDeviceRepository();
DeviceService service =
        new DeviceService(repository);
```

The application code manually constructs and connects everything.

### With Spring IoC

```java
@Service
public class DeviceService {
    private final DeviceRepository deviceRepository;

    public DeviceService(
            DeviceRepository deviceRepository) {
        this.deviceRepository = deviceRepository;
    }
}
```

Spring creates the required objects.

### Common mistake

Do not only say, “IoC means Spring controls the application.” Explain what it controls: object creation, configuration, dependency connection, and lifecycle.

### One-line revision

> IoC means Spring takes responsibility for creating and managing application objects.

---

## What is dependency injection?

### Interview Answer

Dependency injection is how Spring provides the objects that another class needs. For example, `DeviceService` depends on `DeviceRepository`. Instead of writing `new DeviceRepository()` inside the service, I declare the repository in the constructor. Spring finds the repository object it already created and injects it into the service. This reduces tight coupling and also makes unit testing easier because I can inject a mock repository during the test.

### Why this answer is correct

The answer connects DI to:

- A real dependency
- Constructor injection
- Loose coupling
- Unit testing

### Testing example

```java
DeviceRepository repository =
        mock(DeviceRepository.class);

DeviceService service =
        new DeviceService(repository);
```

No Spring application context is required for this unit test.

### One-line revision

> DI means the class receives its dependency instead of creating it.

---

## What is a Spring Bean?

### Interview Answer

A Bean is simply an object that Spring creates and manages. When Spring sees annotations such as `@Service`, `@Repository`, `@Component`, or `@RestController`, it creates those objects during application startup, manages their lifecycle, and reuses them where required. In my project, classes such as `CartController`, `CartService`, and repository implementations were Spring Beans.

By default, most of those beans are singleton-scoped inside the application context. That means Spring normally creates one service bean and reuses it; it does not create a new service object for every request. That is why service beans should usually remain stateless, with request-specific information kept in local variables.

### One-line revision

> A Bean is an object whose creation and lifecycle are managed by Spring.

---

## How are IoC, DI, and Beans connected?

### Interview Answer

They are parts of the same process. IoC is the overall concept where Spring takes control of object creation and management. The objects Spring creates and manages are called Beans. Dependency injection is the mechanism Spring uses to connect those Beans. So Spring creates the repository bean, creates the service bean, sees that the service constructor needs the repository, and injects the repository bean automatically.

```text
IoC
Spring takes control
        ↓
Beans
Spring creates managed objects
        ↓
DI
Spring connects those objects
```

### Cognizant example

Spring created the cart controller, cart service, and repository objects. The cart service did not manually construct its repository or other Spring-managed collaborators. Spring injected the required beans.

### Common mistakes

- A bean is not a Java keyword.
- IoC and DI are related but not identical.
- Spring does not normally create a new singleton service bean for every request.

### Two-line revision

> IoC gives object control to Spring. Spring-managed objects are Beans, and DI connects those Beans.

---

## Why constructor injection?

### Interview Answer

I prefer constructor injection because the dependencies are visible and mandatory. Spring cannot create `DeviceService` without providing its repository, so the object cannot exist in an incomplete state. I can keep the dependency in a `final` field, and unit testing is easy because the test can pass a mock directly into the constructor. When a class has only one constructor, modern Spring does not require `@Autowired` on it.

### Common mistake

Do not say field injection is completely invalid. Explain that constructor injection is normally preferred because dependencies are explicit and testable.

---

## What is `@Autowired`?

### Interview Answer

`@Autowired` tells Spring to resolve and inject a matching bean. In older code, it is commonly used on fields, setters, or constructors. I prefer constructor injection. If the class has only one constructor, modern Spring can inject it without writing `@Autowired`, so the dependency remains visible without an extra annotation.

### Senior engineer tip

If multiple beans implement the same interface, Spring needs help choosing one. Depending on the design, we can use `@Qualifier`, `@Primary`, or explicit configuration.

---

## What are Bean scopes?

### Interview Answer

Bean scope controls how many bean instances Spring creates and how long they live. Singleton is the default, so Spring normally creates one instance per application context and reuses it. Prototype creates a new instance each time the bean is requested. Web applications also have request and session scopes. Most service and repository beans are singleton and should avoid storing request-specific mutable state in instance fields.

### If I explain it simply

Singleton is one shared office employee serving many requests. Request scope creates one employee for one request. Prototype creates a new employee every time someone asks for that bean.

---

## What is the Bean lifecycle?

### Interview Answer

Spring creates the bean, resolves and injects its dependencies, applies framework post-processing, runs initialization callbacks, makes the bean available for use, and later runs destruction callbacks when the application context closes. If initialization work is required, I prefer explicit configuration and keep it small. I avoid putting slow network operations into bean startup unless the application genuinely cannot become ready without them.

### Technical depth

Common lifecycle extension points include `@PostConstruct`, `@PreDestroy`, initialization interfaces, and `BeanPostProcessor`. In many applications, we rarely need custom lifecycle code, but understanding it explains how Spring applies features around beans.

---

## Spring core chapter: answer-depth card

### If I have only 30 seconds

IoC means Spring takes responsibility for creating and managing application objects. Those managed objects are Beans. When one Bean needs another, Spring connects them using dependency injection. I prefer constructor injection because the dependencies are mandatory, visible, and easy to mock in unit tests.

### If the interviewer asks for more

Use the `DeviceService` and `DeviceRepository` example. Explain component scanning, singleton bean reuse, constructor injection, and why a singleton service should remain stateless.

### If they go deeper

Discuss `@Qualifier`, `@Primary`, Bean scopes, lifecycle callbacks, proxies, and why self-invocation can affect proxy-based features.

### Where I used it

```text
CartController
      ↓ injected
CartService
 ├── PricingClient
 ├── PromotionClient
 ├── InventoryClient
 └── CartRepository
```

Spring created and connected these application components. I describe only the dependencies that existed in the real project.

### Common follow-ups

- How does component scanning work?
- What is `@Autowired`?
- Why constructor injection?
- Singleton versus prototype?
- Can singleton Beans be thread-safe?
- What is Bean lifecycle?
- What happens when two Beans implement one interface?

### Two-line revision

> Spring takes control through IoC, creates managed objects called Beans, and connects them using DI. Constructor injection keeps those dependencies explicit and testable.

---

# Interview Chain 2: JPA → Hibernate → Repository → Persistence Context → Transaction

## What is JPA?

### Interview Answer

JPA is a specification that defines how Java objects should be mapped to relational database tables. It does not perform the work itself. Hibernate is the implementation that actually manages entities, generates SQL, and communicates with the database. In Spring Boot, we commonly use Spring Data JPA, which sits on top and gives us repository interfaces such as `JpaRepository`, so we do not have to write boilerplate code for basic CRUD operations.

### Connection

```text
Spring Data JPA
provides repository abstraction
        ↓
JPA
defines persistence rules
        ↓
Hibernate
implements those rules
        ↓
JDBC driver
communicates with database
```

### One-line revision

> JPA defines the ORM contract, Hibernate implements it, and Spring Data simplifies repository code.

---

## What is Hibernate?

### Interview Answer

Hibernate is an ORM framework and a common implementation of JPA. It converts operations on Java entities into SQL, maps database rows back into Java objects, manages entity relationships, and tracks managed objects through the persistence context. For example, when I call `deviceRepository.findById(id)`, Spring Data delegates the operation through JPA, and Hibernate performs the required database work.

---

## What does JpaRepository provide?

### Interview Answer

`JpaRepository` gives us common data-access operations such as `save`, `findById`, `findAll`, `delete`, sorting, and pagination. Spring Data creates the repository implementation at runtime, so I normally define only the interface and any query methods required by the application. For complicated or performance-sensitive queries, I can still write JPQL, native SQL, or DTO projections.

### Enterprise code

```java
public interface DeviceRepository
        extends JpaRepository<DeviceEntity, Long> {

    Optional<DeviceEntity> findBySerialNumber(
            String serialNumber);
}
```

---

## What happens during `findById()`?

### Interview Answer

Spring Data passes the lookup to the JPA provider. Hibernate first checks whether that entity is already managed in the current persistence context. If it is not available there, Hibernate generates and executes the SQL through the database connection. It maps the row into an entity and places that entity under persistence-context management. If I request the same entity again in the same context, Hibernate can return the same managed object.

---

## What is the persistence context?

### Interview Answer

The persistence context is where Hibernate keeps and tracks managed entity objects during a unit of work. It acts as a first-level cache and identity map. If a managed entity changes during a transaction, Hibernate detects the change and synchronizes it with the database at flush time. That behavior is called dirty checking.

### Common mistake

Do not describe the first-level cache as a shared application-wide Redis-style cache. It belongs to the persistence context and is not a distributed cache.

---

## What is `@Transactional`?

### Interview Answer

`@Transactional` tells Spring to execute the related database operations inside one transaction. Spring begins the transaction before the method’s database work, commits it when the method succeeds, and rolls it back according to the configured exception rules if the operation fails. I normally place the transaction boundary in the service layer because the service represents the business operation and may coordinate multiple repository calls.

For example, updating a device status and creating an audit record in the same database can be one transaction. However, that transaction cannot automatically roll back another microservice or an external payment provider.

### Important follow-up

If the interviewer asks why a private or self-invoked transactional method may not work as expected, explain that standard Spring transaction management is commonly applied using proxies. A call from one method to another method on the same object may bypass the proxy.

---

## What are ACID properties?

### Interview Answer

ACID describes reliable transaction behavior. Atomicity means all related changes complete or none are kept. Consistency means the database rules remain valid. Isolation controls how concurrent transactions affect one another. Durability means committed data survives a system failure. In a flight-booking example, these properties help prevent a seat update from being left in a partial or invalid state.

### Common mistake

Do not claim ACID across independent microservices simply because one service method has `@Transactional`.

---

# Interview Chain 3: OOP → Interface → Polymorphism → Overloading and Overriding

## What is OOP?

### Interview Answer

OOP is a way of designing software using objects that combine state and behavior. For example, a `Device` object can contain its ID, name, and temperature, and also provide behavior such as updating temperature or checking status. The main principles are encapsulation, inheritance, polymorphism, and abstraction. In enterprise Java, I use these principles to separate responsibilities and make implementations easier to replace and test.

---

## Explain encapsulation

### Interview Answer

Encapsulation means protecting an object’s internal state and changing it through controlled methods. For example, I keep the device temperature private and expose an `updateTemperature()` method that validates the supported range before making the change. It is similar to a bank account: we cannot directly edit the balance; we use deposit and withdrawal operations that enforce the rules.

---

## Explain abstraction

### Interview Answer

Abstraction means exposing the operation a caller needs while hiding the implementation details. For example, a service can depend on a `NotificationSender` interface with a `send()` method. The service does not need to know whether the implementation sends email or SMS. This lowers coupling because the business service depends on the contract rather than a specific provider.

---

## Explain polymorphism

### Interview Answer

Polymorphism means I can use one parent type or interface while different objects provide their own implementation. For example, both `EmailNotificationSender` and `SmsNotificationSender` implement the same `NotificationSender` interface. The service calls `send()` through the interface, and Java runs the implementation belonging to the actual object. This removes the need for a large `if` statement checking every sender type.

---

## Overloading versus overriding?

### Interview Answer

Overloading means I use the same method name with a different parameter list. For example, a calculator can have `add(int first, int second)` and `add(int first, int second, int third)`. Java chooses the matching method at compile time from the arguments I provide.

Overriding means a child class or interface implementation keeps the same method signature but provides different behavior. For example, `Motor.start()` and `Pump.start()` can both override `Device.start()`. If I hold a Motor inside a Device reference and call `start()`, Java runs the Motor version at runtime.

```text
Overloading = same name, different inputs
Overriding  = same signature, different implementation
```

### Enterprise code

```java
public class Device {
    public void start() {
        System.out.println("Starting device");
    }
}

public class Motor extends Device {
    @Override
    public void start() {
        System.out.println("Starting motor");
    }
}

Device device = new Motor();
device.start(); // Starting motor
```

### Common mistakes

- Getter and setter methods are not overriding each other.
- Changing only the return type does not create a valid overload.
- An overriding method keeps a compatible signature.

---

# Interview Chain 4: HashMap → hashCode → equals → ConcurrentHashMap

## How does HashMap work?

### Interview Answer

HashMap stores data as key-value pairs. Whenever I insert a key, Java calculates the key’s hash code and converts it into a bucket location. During retrieval, Java goes directly to that bucket instead of scanning every map entry, which is why lookup is O(1) on average. If several keys map to the same bucket, that is a collision. Java compares the keys using `equals()`, and modern Java may use a balanced tree when one bucket becomes heavily populated.

### One-line revision

> Hash code finds the bucket; equals finds the correct key inside the bucket.

---

## Why do we need both `hashCode()` and `equals()`?

### Interview Answer

`hashCode()` narrows the search to one bucket, while `equals()` confirms the actual logical key. If two objects are equal, they must return the same hash code. Otherwise, HashMap may search in the wrong bucket and fail to retrieve the value.

---

## Why is String a good key?

### Interview Answer

String is immutable, so its content and hash code do not change after it is inserted into the map. If a mutable key changed after insertion, HashMap might calculate a different bucket during retrieval and the entry could become difficult to find.

---

## HashMap versus ConcurrentHashMap?

### Interview Answer

HashMap is not safe for concurrent updates. ConcurrentHashMap is designed for multiple threads to read and update safely and gives better concurrency than protecting the complete map with one global lock. I would use it only for required in-memory shared state. In a distributed application with several instances, one local ConcurrentHashMap is not a shared system-wide source of truth.

---

# Interview Chain 5: REST → Validation → JWT → Error Handling

## What is a REST API?

### Interview Answer

A REST API allows clients and services to communicate through HTTP. The client sends a request to an endpoint, and the backend authenticates the caller, validates the request, applies business logic, accesses the database or dependent services, and returns a predictable response, normally JSON. In my Cognizant project, when a customer added an item, the frontend called our cart API, and the service coordinated pricing, promotion, inventory, and cart rules before returning the updated cart.

---

## Why should React not access the database?

### Interview Answer

The frontend is not a trusted environment and can be inspected, changed, or bypassed. Direct database access would expose credentials and allow clients to bypass authorization and business rules. The backend is the controlled boundary where we enforce authentication, permissions, validation, logging, and safe data access.

---

## Why validate again in the backend?

### Interview Answer

Frontend validation improves the user experience, but it is not a security boundary. The backend must validate the request because a caller can bypass the UI. I first validate the token and request format, then verify trusted business facts. For example, in the Siemens platform, I would check that the device exists and that the user has access to that device’s tenant or plant before returning readings.

---

## Authentication versus authorization?

### Interview Answer

Authentication verifies who the user is. Authorization checks what that authenticated user is allowed to access. At an airport, a passport proves identity, while a boarding pass or staff credential controls where the person is allowed to go.

---

## What is JWT?

### Interview Answer

A JWT is a signed token containing claims about the authenticated user or client. The client sends it with each API request. The backend validates its signature, issuer, audience, and expiry before trusting the claims. The payload is encoded but not encrypted by default, so I do not place passwords or sensitive information inside it, and I never log the full token.

---

## How do you handle exceptions?

### Interview Answer

I use meaningful domain exceptions and handle them centrally using `@RestControllerAdvice`. For example, `DeviceNotFoundException` can be converted into a 404 response with a consistent error code, safe message, timestamp, and correlation ID. I keep stack traces and database details in protected logs rather than exposing them to the client.

---

# Interview Chain 6: Microservices → JSON Flow → ELK Debugging → Resilience

## What is a microservice?

### Interview Answer

A microservice is an independently deployable service responsible for one business capability. In the Cognizant application, cart, pricing, promotion, inventory, and checkout had different responsibilities. That allowed separate development and deployment, but it also introduced network failures, contract dependencies, distributed debugging, and data-consistency challenges.

---

## Explain your cart flow

### Interview Answer

When a customer added a product, our cart service exchanged JSON with pricing, promotion, inventory, and checkout services. I compare the JSON to an order sheet moving through departments. Each service reads the fields relevant to it, applies its rules, and returns information.

In our service, values such as bundle eligibility and promotion status controlled which method or conditional branch executed. Therefore, when the cart behaved unexpectedly, I had to verify both our business logic and the data received from upstream services.

---

## Tell me about an issue you debugged

### Interview Answer

We had a case where an unexpected bundle flow executed. I used the correlation ID to locate the transaction in ELK and inspected the JSON received by our cart service. I compared the payload with the expected contract and identified the exact field that caused the conditional branch to run. I reproduced the behavior in a lower environment and added focused logging or tests around that decision point. If our condition was wrong, I corrected our code. If the payload violated the contract, I raised an evidence-based ticket containing the request, response, timestamp, correlation ID, and expected value.

The main lesson was that microservice debugging requires understanding the complete data journey, not only reading one service’s code.

---

## What if a dependency is down?

### Interview Answer

I start with strict connection and response timeouts so request threads do not wait indefinitely. I use limited retries with exponential backoff and jitter only for temporary failures and safe or idempotent operations. A circuit breaker stops repeated calls to an unhealthy dependency, and a bulkhead prevents one dependency from exhausting every shared resource. For asynchronous work, failed messages may go to a dead-letter queue. Logs, metrics, and traces must make the failure visible.

---

# Interview Chain 7: Maven → Docker → ECR → Runtime → Kubernetes → Monitoring

## Explain your CI/CD flow

### Interview Answer

We used GitHub Actions or Jenkins to orchestrate the pipeline, while AWS was the target cloud environment. When code was merged, Maven compiled the application and ran tests. The pipeline performed the required quality and security checks, built a versioned Docker image, and pushed that image to Amazon ECR. The image was then deployed to the correct runtime. A long-running Spring Boot API could run on ECS, EKS, or EC2, while an event-driven component could use Lambda. After deployment, health checks and smoke tests verified the release before promotion.

---

## What is Docker?

### Interview Answer

Docker packages an application with its runtime and dependencies into a consistent image. An image is the immutable blueprint, and a container is a running instance of that image. This helps the same application package run consistently in development, QA, and production.

---

## What is ECR?

### Interview Answer

Amazon ECR is AWS’s managed container-image registry. The CI/CD pipeline pushes versioned Docker images to ECR, and services such as ECS, EKS, or Lambda can pull the required image for deployment. ECR stores the image; it does not run the application.

---

## What is Kubernetes?

### Interview Answer

Kubernetes orchestrates containerized applications. Docker packages the container, while Kubernetes schedules Pods, maintains the required number of replicas, provides service discovery and traffic routing, performs rolling updates, and replaces failed Pods. I compare Docker to creating taxis and Kubernetes to managing the complete taxi fleet.

---

## Liveness versus readiness?

### Interview Answer

Liveness asks whether the process is unhealthy and should be restarted. Readiness asks whether that instance is currently able to receive traffic. An application can be alive but temporarily not ready while it is starting or while a required initialization step is incomplete.

---

## How do you monitor it?

### Interview Answer

I monitor infrastructure and application behavior together. CloudWatch provides AWS metrics, logs, dashboards, and alarms. The CloudWatch Agent can collect EC2 memory, disk, and application logs. Spring Boot Actuator provides health and application metrics, while correlation IDs and traces connect a request across services.

I monitor latency, traffic, errors, and saturation, along with JVM memory, garbage collection, database-pool pressure, downstream latency, and business success rates. Alarms notify the support team when a threshold remains unhealthy.

---

# Interview Chain 8: Flight Booking → Locking → Idempotency → Saga

## Design a flight-booking backend

### Interview Answer

I would first confirm the scope: search, fare and availability, seat selection, temporary seat hold, booking, payment, confirmation, and cancellation. I would separate responsibilities into Search, Inventory, Booking, Payment, Customer, and Notification services.

Search is read-heavy, so common results can be cached, but final fare and seat availability must be checked against the source of truth before booking. The Inventory Service controls seat state. The Booking Service coordinates the workflow. Payment uses an idempotency key, and notification happens asynchronously after confirmation.

---

## How do you stop double booking?

### Interview Answer

The Inventory Service should perform an atomic conditional update, unique constraint, or appropriate lock. Only one request can change seat 10A from `AVAILABLE` to `HELD`. The hold includes an expiry time so an abandoned booking does not block the seat permanently.

---

## What if payment succeeds but the response is lost?

### Interview Answer

The payment request includes a stable idempotency key linked to the booking attempt. If the response times out, the system does not blindly submit a new charge. It queries or reconciles the provider transaction using the same identifiers. The booking remains in a pending state until the actual payment result is known.

---

## Why not one `@Transactional` method?

### Interview Answer

`@Transactional` controls one local database transaction. Inventory, booking, an external payment provider, and notification do not all share that database transaction. The complete workflow needs a saga or state machine. If a later step fails, the system performs a compensating action, such as releasing a held seat or refunding a captured payment.

---

## What if notification fails?

### Interview Answer

A notification failure should not cancel a confirmed booking. The Notification Service consumes the booking event asynchronously and retries according to policy. After repeated failure, the event can move to a dead-letter queue for investigation and replay.

---

# Interview Chain 9: Behavioral questions

## Tell me about a production issue

### Interview Answer

In the Cognizant cart service, we had an issue where a bundle-related flow executed unexpectedly. The service depended on JSON fields received from upstream services, so the visible behavior did not immediately identify the owner.

I traced the request in ELK using its correlation ID, compared the payload with the expected contract, and identified the exact field that selected the branch. I reproduced the issue in a lower environment and added focused diagnostics and tests. Based on the evidence, I either corrected our logic or worked with the upstream team using the request, response, timestamp, and expected value.

This improved the way I approach distributed issues: I first reconstruct the complete data flow and use evidence before deciding where the defect belongs.

---

## Tell me about working with another team

### Interview Answer

When our cart service received an unexpected field, I did not begin by blaming the upstream team. I first verified our own logic and confirmed the expected API contract. I then shared the exact request, response, timestamp, and correlation ID with the upstream team. Because both teams could see the same evidence, we avoided an opinion-based discussion and identified the correct resolution faster.

---

## Tell me about a performance improvement

### Interview Answer

I first establish a baseline rather than changing code based on assumptions. I separate application-processing time from database and downstream-service time. Once I identify the actual bottleneck, I make the targeted change, such as improving a query, reducing an unnecessary payload, adding the correct cache, or running independent calls concurrently. I repeat the same test under comparable conditions and compare the result with the baseline.

If I provide a percentage, I am prepared to explain the measurement tool, before value, exact change, test conditions, and final value.

---

## Tell me about a disagreement

### Interview Answer

When I disagree with a technical approach, I bring the discussion back to the requirement, risks, and evidence. I first understand the other person’s concern, explain my reasoning using the API contract, dependency order, test result, or production risk, and suggest a small test or proof of concept if needed. My goal is not to win the argument; it is to help the team choose the safest and most maintainable solution.

---

# Answer-depth cards for every interview chain

These cards show how to control the depth of an answer. Start with 30 seconds and expand only when the interviewer asks.

## Persistence: JPA, Hibernate, and transactions

### If I have only 30 seconds

JPA defines how Java objects map to relational tables. Hibernate implements that specification and performs the database work. Spring Data JPA adds repositories such as `JpaRepository`. The service transaction defines the local unit of work, and Hibernate tracks managed entities through the persistence context.

### If the interviewer asks for more

Trace `findById()` from the repository through Hibernate to the database. Explain first-level caching, dirty checking, and the difference between lazy and eager loading.

### If they go deeper

Discuss flush versus commit, N+1 queries, transaction propagation, isolation, optimistic locking, and why proxy-based transaction management can be bypassed through self-invocation.

### Production example

In a Siemens device flow, a service could retrieve a device, validate access, update its controlled state, and create an audit record inside one local database transaction. Cross-service alert or notification work would not be described as part of that same transaction.

### Senior engineer tip

Do not say JPA removes the need to understand SQL. Production JPA work still requires knowledge of generated queries, indexes, fetch plans, and transaction boundaries.

### Related questions

- JPA versus Hibernate?
- Persistence context?
- Dirty checking?
- Lazy versus eager?
- N+1?
- Transaction propagation?
- ACID and isolation?

---

## OOP and polymorphism

### If I have only 30 seconds

OOP organizes software around objects that combine state and behavior. Encapsulation protects internal state, abstraction exposes a useful contract, inheritance models a genuine “is-a” relationship, and polymorphism lets different implementations work through one common type.

### If the interviewer asks for more

Use `Device`, `Motor`, and `Pump` for inheritance and overriding. Use `NotificationSender`, `EmailNotificationSender`, and `SmsNotificationSender` for interfaces and polymorphism.

### If they go deeper

Explain composition over inheritance, immutable value objects, the equality contract, SOLID principles, and how interface-based design enables Spring DI and unit testing.

### Production example

A cart service can depend on interfaces for pricing, promotions, inventory, and persistence. Spring injects the production implementations, while unit tests inject mocks.

### Senior engineer tip

Do not force inheritance merely to reuse code. Prefer composition unless the model has a stable and meaningful “is-a” relationship.

### Related questions

- Interface versus abstract class?
- Encapsulation versus abstraction?
- Overloading versus overriding?
- Composition versus inheritance?
- What is an immutable class?

---

## HashMap and collections

### If I have only 30 seconds

HashMap stores key-value pairs. It uses `hashCode()` to locate a bucket and `equals()` to find the correct key in that bucket. Lookup is O(1) on average—not guaranteed—because collisions can place several keys in one bucket.

### If the interviewer asks for more

Explain collisions, treeification in modern Java, capacity, load factor, resizing, and the `equals()`/`hashCode()` contract.

### If they go deeper

Discuss mutable keys, thread safety, compound operations on `ConcurrentHashMap`, and why local memory is not shared across distributed service instances.

### Production example

In the Siemens project, a HashMap could support a temporary lookup during one operation, such as mapping device IDs to objects already retrieved. We would not use one service instance’s HashMap as the authoritative store for shared plant data. Shared business state belongs in a database or an appropriate distributed store such as Redis.

### Senior engineer tip

Say “O(1) on average.” Do not promise constant time in every case.

### Related questions

- Why `equals()` and `hashCode()`?
- What is a collision?
- What happens during resize?
- How does HashSet work?
- HashMap versus ConcurrentHashMap?
- Why are mutable keys dangerous?

---

## REST API and security

### If I have only 30 seconds

A REST API receives an HTTP request, authenticates and validates it, executes business logic, and returns a predictable response, normally JSON. The controller handles HTTP concerns, while services and repositories handle business logic and persistence.

### If the interviewer asks for more

Walk through Add to Cart, including JWT validation, DTO validation, pricing and promotion calls, persistence, and the returned HTTP status.

### If they go deeper

Discuss idempotency, API versioning, pagination, optimistic concurrency, secure error responses, rate limiting, and observability.

### Production example

In Cognizant, the frontend called the cart API rather than the database. The backend applied trusted business rules and coordinated downstream services before returning the updated cart JSON.

### Senior engineer tip

Do not describe REST only as “JSON over HTTP.” Explain resource design, HTTP semantics, stateless requests, and correct status behavior.

### Related questions

- `POST` versus `PUT`?
- `401` versus `403`?
- What is idempotency?
- Why use DTOs?
- How does Jackson work?
- How do you validate and secure an API?

---

## Microservices and production debugging

### If I have only 30 seconds

A microservice owns one business capability and can be deployed independently. This provides team and scaling flexibility, but introduces network failure, contract, observability, security, and distributed-consistency challenges.

### If the interviewer asks for more

Explain the Cognizant cart, pricing, promotion, inventory, and checkout flow and how the correlation ID traced the JSON between them.

### If they go deeper

Discuss synchronous versus asynchronous communication, timeouts, idempotent retries, circuit breakers, bulkheads, distributed tracing, and saga patterns.

### Production example

Use the bundle issue traced through ELK. Keep the field names and implementation details aligned with what you genuinely remember.

### Senior engineer tip

Do not say microservices are always better than a monolith. Explain that they are justified when business boundaries, scaling, ownership, or release independence are worth the distributed-system cost.

### Related questions

- REST versus messaging?
- What if a service is down?
- How do you trace one request?
- How do you manage API contracts?
- What is eventual consistency?

---

## CI/CD, AWS, and monitoring

### If I have only 30 seconds

The pipeline uses Maven to build and test the application, Docker to package it, ECR to store the versioned image, and the chosen AWS runtime to execute it. Health checks validate the deployment, while CloudWatch provides metrics, logs, dashboards, and alarms.

### If the interviewer asks for more

Trace a commit through GitHub Actions or Jenkins, Maven, quality and security scanning, Docker, ECR, deployment, smoke tests, and controlled promotion.

### If they go deeper

Discuss immutable image tags, secret management, rollback-compatible database migrations, readiness versus liveness, blue-green versus canary, and the signals that stop a rollout.

### Production example

At Cognizant, describe blue-green or canary as a team release practice. At Siemens, mention only CI/CD stages and AWS runtime details that you can personally defend.

### Senior engineer tip

AWS is the cloud environment. It is not automatically the CI/CD tool. ECR stores images; ECS, EKS, EC2, and Lambda run workloads.

### Related questions

- Maven lifecycle?
- Image versus container?
- ECR versus ECS?
- Lambda versus ECS/EKS?
- Readiness versus liveness?
- What triggers a rollback?
- How do you monitor the JVM?

---

## Flight-booking system design

### If I have only 30 seconds

I separate read-heavy search from correctness-sensitive inventory and booking. Search may be cached, but inventory is authoritative. Booking uses a temporary atomic seat hold, payment idempotency, and a saga for cross-service failures. Confirmation events drive recoverable downstream work such as notifications.

### If the interviewer asks for more

Walk through search, seat hold, pending booking, payment, confirmation, event publication, and release of the seat on failure.

### If they go deeper

Discuss concurrency control, outbox patterns, message deduplication, payment reconciliation, hold expiration, audit records, and regional availability.

### Air Canada production note

Tie design decisions to customer impact: preventing duplicate charges, stopping double booking, making confirmations recoverable, and maintaining availability during peak travel periods.

### Senior engineer tip

State the correctness requirement before naming a technology. Explain why inventory needs atomic control before discussing locks or databases.

### Related questions

- How do you prevent double booking?
- What happens during a payment timeout?
- Saga versus local transaction?
- What is an outbox?
- What data can be cached?
- How do you deploy safely?

---

## Behavioral answers

### If I have only 30 seconds

State the situation, your responsibility, the main action you personally took, and the result. Keep the first answer concise and allow the interviewer to choose where to go deeper.

### If the interviewer asks for more

Explain your decision, alternatives, collaboration, measurement, and lesson. Use “I” for personal actions and “we” for team outcomes.

### If they go deeper

Be prepared to name the field, log, API contract, test, query, pipeline step, baseline, or stakeholder involved.

### Air Canada note

Strong behavioral themes include customer impact, reliability, teamwork, communication under pressure, ownership, safe production changes, and learning.

### Senior engineer tip

A simple real story is stronger than a large story that becomes inconsistent under follow-up questions.

### Related questions

- What exactly did you do?
- What alternatives did you consider?
- How did you measure the result?
- What would you do differently?
- What did you learn?

---

# Final revision maps

## Spring

```text
MVC
↓
DispatcherServlet
↓
IoC
↓
Bean
↓
Dependency Injection
↓
Constructor Injection
↓
JPA
↓
Hibernate
↓
Persistence Context
↓
Transaction
↓
ACID
```

## Deployment

```text
Git push
↓
GitHub Actions / Jenkins
↓
Maven build and test
↓
Docker image
↓
Amazon ECR
↓
ECS / EKS / EC2 / Lambda
↓
Health checks
↓
CloudWatch monitoring
```

## Cognizant issue

```text
Unexpected cart behavior
↓
Correlation ID
↓
ELK search
↓
Inspect JSON
↓
Compare API contract
↓
Find triggering condition
↓
Fix or evidence-based ticket
↓
Test
↓
Safe deployment
```

## Two-line interview rule

> Explain what it is, why it is used, and one example. Then stop and let the interviewer create the next question.

The objective is not to sound like a book. The objective is to sound like a backend developer explaining work they genuinely understand.
