# Spring Boot, REST, and Maven

[Master index](../README.md) · [Enterprise Java playbook](../FINAL-INTERVIEW-HANDBOOK.md) · [Answer book](../FINAL-INTERVIEW-ANSWER-BOOK.md) · Previous: [Java core](../01-java-core/README.md) · Next: [Database](../03-database/README.md)

## MVC and layered architecture

### Interview answer

Spring MVC separates HTTP handling, business rules, and persistence. The controller accepts and validates the web request, the service coordinates business logic, and the repository performs data access. The model carries data between layers, and in a REST service the “view” is normally the JSON response. It is like a restaurant: the waiter handles the customer, the kitchen applies the recipe, and the store room manages ingredients; one person does not perform every job.

```text
Client → Controller → Service → Repository → Database
       ← JSON DTO  ←         ←            ←
```

### Production example

In the Cognizant cart flow, the controller received an add-to-cart request, the service applied bundle and promotion rules and coordinated downstream calls, and the repository stored or retrieved cart data. Keeping SQL and business rules out of the controller made the code easier to test and reuse.

## Request lifecycle and DispatcherServlet

### Interview answer

The `DispatcherServlet` is Spring MVC’s front controller. An incoming request reaches the web server and then the `DispatcherServlet`, which uses registered handler mappings to select the correct controller method. Spring binds path values, query parameters, headers, and JSON to Java parameters, invokes the controller, and serializes the returned DTO to JSON through an HTTP message converter.

```text
HTTP request
  → Tomcat
  → DispatcherServlet
  → HandlerMapping
  → Controller
  → Service
  → Repository/Hibernate
  → Database
  → Jackson serialization
  → HTTP response
```

### Follow-ups

- How does Spring discover controller routes?
- What role does Jackson play?
- What is a filter versus an interceptor?
- Where does validation execute?

## IoC, dependency injection, and beans

### Interview answer

Inversion of Control means Spring, rather than application code, creates and manages component objects. Dependency injection is how Spring supplies those managed objects to classes that need them. A Spring bean is simply an object managed by the IoC container. Think of plugging a device into the electrical grid: the device consumes power but does not construct a power station.

Constructor injection is preferred because dependencies are explicit, mandatory, testable, and can be stored in `final` fields.

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
        return ResponseEntity.ok(deviceService.getDevice(deviceId));
    }
}
```

Spring discovers stereotypes such as `@Component`, `@Service`, `@Repository`, and `@RestController`. If multiple beans implement the same interface, select one using design-specific configuration, `@Primary`, or `@Qualifier`.

## REST API design

### Interview answer

A production REST API exposes resource-oriented endpoints over HTTP and returns predictable representations, usually JSON. I use correct HTTP methods and status codes, request validation, authentication and authorization, DTOs rather than entities, centralized error responses, pagination, idempotency where required, structured logs, metrics, and tests. An API is like a controlled bank counter: clients never enter the vault; the counter verifies identity, permissions, and the requested operation.

### Add-to-cart flow

```text
POST /api/v1/carts/{cartId}/items
  → validate JWT and authorization
  → validate product ID and quantity
  → load cart
  → call pricing/promotion/inventory services
  → apply bundle rules
  → persist cart change
  → return updated cart DTO
```

Recommended semantics:

- `GET` retrieves a resource.
- `POST` creates a resource or begins a non-idempotent operation.
- `PUT` replaces a resource.
- `PATCH` partially updates it.
- `DELETE` removes it.

Common statuses include `200`, `201`, `204`, `400`, `401`, `403`, `404`, `409`, `500`, and `503`.

### Authentication versus authorization

Authentication proves who the caller is; authorization decides what that identity may do. At an airport, a passport verifies identity, while boarding access determines whether the person may enter a particular flight or restricted area.

### JWT

A JWT is a signed token containing claims. A client presents it with requests, and the server validates its signature, issuer, audience, and expiry before trusting the claims. Never describe JWT payloads as encrypted by default, and never log tokens.

### Follow-ups

- `401` versus `403`?
- `PUT` versus `PATCH`?
- How do you make payment creation idempotent?
- Why use DTOs instead of returning JPA entities?
- How do you version an API?

## Validation and exception handling

```java
public record AddDeviceRequest(
        @NotBlank String serialNumber,
        @NotBlank String plantId) {
}

public record ApiError(
        String code,
        String message,
        String correlationId,
        Instant timestamp) {
}

@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(DeviceNotFoundException.class)
    public ResponseEntity<ApiError> handleNotFound(
            DeviceNotFoundException exception,
            HttpServletRequest request) {
        ApiError error = new ApiError(
                "DEVICE_NOT_FOUND",
                exception.getMessage(),
                request.getHeader("X-Correlation-Id"),
                Instant.now());
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
}
```

Return safe, consistent client messages; keep internal diagnostics in structured logs.

## Maven

### Interview answer

Maven is a Java build and dependency-management tool. It reads the `pom.xml`, resolves declared libraries from configured repositories, caches them locally, compiles code, runs tests, and packages the application as a JAR or WAR. It is like giving a supplier a standardized bill of materials: every developer and CI server can reproduce the same build.

`mvn clean install` removes prior build output, runs the lifecycle through compilation, tests, packaging, verification, and installs the artifact in the local Maven repository. In CI, `mvn clean verify` is often sufficient when another local project does not need the artifact.

```text
pom.xml
  → resolve dependencies
  → compile
  → test
  → package JAR
  → Docker image
  → deployment
```

A Spring Boot starter is a curated dependency bundle. For example, `spring-boot-starter-web` brings the libraries needed for Spring MVC, JSON conversion, and an embedded web server.

### Follow-ups

- What is transitive dependency resolution?
- What is dependency scope?
- Maven versus Gradle?
- JAR versus WAR?
- Why commit `pom.xml` but not dependency JAR files?

## JPA and Hibernate

### Interview answer

JPA is the Java specification for persistence and object-relational mapping; Hibernate is a common implementation of that specification. Spring Data JPA adds repository abstractions such as `JpaRepository`. In simple terms, JPA defines the translation contract between Java objects and relational tables, Hibernate performs that translation, and Spring Data reduces repository boilerplate.

```java
@Entity
@Table(name = "device")
public class DeviceEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String serialNumber;

    protected DeviceEntity() {
    }
}

public interface DeviceRepository
        extends JpaRepository<DeviceEntity, Long> {
    Optional<DeviceEntity> findBySerialNumber(String serialNumber);
}
```

The persistence context is Hibernate’s first-level cache and identity map for managed entities. Inside a transaction, Hibernate tracks changes and uses dirty checking to synchronize modified managed entities at flush time.

### Lazy versus eager loading

Lazy loading retrieves related data when it is accessed; eager loading retrieves it with the parent. For a machine with thousands of readings, loading the readings only for endpoints that need them avoids unnecessary work. Prefer explicit query design, DTO projections, or fetch joins rather than treating one global fetch mode as universally correct.

### Transaction caveat

`@Transactional` groups database operations within one local transaction. It cannot automatically roll back a successful remote payment call or another microservice’s database. Cross-service workflows normally require idempotency and a saga/compensating-action design.

### Follow-ups

- Persistence context and dirty checking?
- N+1 query problem?
- `save()` insert versus update?
- Lazy initialization exception?
- Why place transactions in the service layer?
