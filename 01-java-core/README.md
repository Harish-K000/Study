# Java Core

[Master index](../README.md) · [Enterprise Java playbook](../FINAL-INTERVIEW-HANDBOOK.md) · [Answer book](../FINAL-INTERVIEW-ANSWER-BOOK.md) · Next: [Spring Boot](../02-spring-boot/README.md)

For a beginner-friendly, connected explanation of Java from variables and references through OOP and enterprise design, use the [Complete Java Booklet](JAVA-BOOKLET.md).

## Heap and stack

### Interview answer

The stack stores method frames, local primitive values, and object references, while the heap stores objects and their instance data. Each thread has its own stack, but the heap is shared across threads and is managed by the garbage collector. A useful analogy is a personal desk versus a shared warehouse: the desk is fast and cleared when the employee finishes, while the warehouse holds larger shared items until they are no longer needed.

### Technical explanation

When `Device device = new Device("Motor");` executes, the local reference `device` belongs to the current stack frame and the `Device` object is allocated on the heap. When the method ends, its frame is removed. If no live reference points to the object, it becomes eligible for garbage collection.

```java
public final class Device {
    private final String name;

    public Device(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

### Production example

If a service continuously adds objects to a long-lived static collection and never removes them, the references remain reachable and the garbage collector cannot reclaim the objects. This can eventually produce an `OutOfMemoryError`.

### Follow-ups

- Why is the stack faster than the heap?
- What causes a Java memory leak?
- What happens when heap space is exhausted?
- Does the garbage collector manage stack memory?

## String pool, equality, and immutability

### Interview answer

The String pool is a special area in the heap where Java reuses equal string literals. The `==` operator compares whether two references point to the same object, whereas `equals()` compares string content. Strings are immutable, so their value cannot change after construction; this makes pooled sharing safe, supports thread safety, and keeps hash codes stable when Strings are used as map keys. Think of a shared master document that everyone may read but nobody may edit.

```java
String first = "Air";
String second = "Air";
String third = new String("Air");

System.out.println(first == second);      // true
System.out.println(first == third);       // false
System.out.println(first.equals(third));  // true
```

Use `StringBuilder` when repeatedly building text:

```java
public String buildStatus(String deviceName, String status) {
    return new StringBuilder()
            .append("Device: ")
            .append(deviceName)
            .append(", status: ")
            .append(status)
            .toString();
}
```

### Follow-ups

- Why is String a good `HashMap` key?
- `StringBuilder` versus `StringBuffer`?
- What does `intern()` do?
- Why is concatenation in a large loop inefficient?

## Collections

### Interview answer

I choose a collection based on access patterns. `ArrayList` is my default for ordered data because it provides fast indexed reads and efficient iteration. `LinkedList` is useful only for specific queue or deque behavior; although link updates are cheap once a node is known, locating a middle position is still O(n), so it is not automatically faster for middle insertions. `HashMap` provides average O(1) key lookup, and `HashSet` provides uniqueness using a map internally.

### ArrayList versus LinkedList

An `ArrayList` is like numbered airplane seats: the system can jump directly to seat 350, but inserting in the middle requires shifting later elements. A linked structure is like train coaches connected to neighboring coaches, but reaching coach 350 still requires walking through the train.

For an Air Canada passenger list, `ArrayList` is normally better because reads, iteration, and appends dominate. For a queue, prefer the `Deque` interface with `ArrayDeque` in most modern Java code.

```java
List<String> passengerNames = new ArrayList<>();
passengerNames.add("Asha");
passengerNames.add("David");

Deque<String> boardingQueue = new ArrayDeque<>();
boardingQueue.offerLast("Asha");
String nextPassenger = boardingQueue.pollFirst();
```

### HashMap internals

`HashMap` uses the key’s `hashCode()` to locate a bucket and `equals()` to identify the correct key within that bucket. Average lookup is O(1). Collisions place multiple entries in one bucket; modern Java may treeify a heavily populated bucket, improving its worst-case lookup behavior.

```java
Map<Long, Device> devicesById = new HashMap<>();
devicesById.put(100L, new Device("Motor"));

Device device = devicesById.get(100L);
```

### Follow-ups

- Why is `Map` not a subtype of `Collection`?
- How are collisions resolved?
- Why must `equals()` and `hashCode()` agree?
- `HashMap` versus `ConcurrentHashMap`?
- `HashSet` versus `TreeSet`?

## Exception handling

### Interview answer

Checked exceptions are enforced by the compiler and must be handled or declared, while unchecked exceptions extend `RuntimeException` and usually represent programming errors, invalid state, or domain failures. In a Spring application, I use meaningful custom exceptions and a global handler so clients receive consistent status codes without internal stack traces.

```java
public final class DeviceNotFoundException extends RuntimeException {
    public DeviceNotFoundException(long deviceId) {
        super("Device not found: " + deviceId);
    }
}
```

Avoid empty `catch` blocks and broad exception handling that hides the root cause. Log contextual identifiers, but never passwords, access tokens, payment data, or sensitive personal information.

### Follow-ups

- `throw` versus `throws`?
- When does `finally` run?
- Which exceptions trigger Spring transaction rollback?
- Why create domain-specific exceptions?

## Streams and Optional

### Interview answer

A collection stores data; a Stream describes a processing pipeline over that data. I use Streams for readable filtering, mapping, sorting, and aggregation, but I prefer ordinary control flow when a stream chain would hide complex business logic. `Optional` explicitly represents a value that may be absent and is most useful as a return type.

```java
public List<String> activeDeviceNames(List<DeviceRecord> devices) {
    return devices.stream()
            .filter(DeviceRecord::active)
            .map(DeviceRecord::name)
            .sorted()
            .toList();
}

public DeviceRecord requireDevice(
        DeviceRepository repository,
        long deviceId) {
    return repository.findById(deviceId)
            .orElseThrow(() -> new DeviceNotFoundException(deviceId));
}
```

Intermediate Stream operations such as `filter()` and `map()` are lazy. Work begins when a terminal operation such as `toList()`, `count()`, or `findFirst()` executes. `orElseGet()` creates a fallback lazily, whereas `orElse()` evaluates its fallback eagerly.

### Follow-ups

- `map()` versus `filter()`?
- Can a Stream be reused?
- When are parallel streams risky?
- Where should `Optional` not be used?

## Multithreading and concurrency

### Interview answer

A process is an independent running application; a thread is a unit of execution inside it. Concurrency means multiple tasks make progress during the same period, while parallelism means tasks actually execute at the same time. Threads can improve throughput for independent work, but shared mutable state introduces race conditions, visibility problems, and deadlocks. The analogy is a restaurant: multiple chefs share one kitchen, so responsibilities and shared equipment must be coordinated.

For independent downstream calls, a controlled executor and `CompletableFuture` can reduce total waiting time:

```java
@Service
public class CartEnrichmentService {
    private final PricingClient pricingClient;
    private final PromotionClient promotionClient;
    private final Executor applicationExecutor;

    public CartEnrichmentService(
            PricingClient pricingClient,
            PromotionClient promotionClient,
            Executor applicationExecutor) {
        this.pricingClient = pricingClient;
        this.promotionClient = promotionClient;
        this.applicationExecutor = applicationExecutor;
    }

    public CartDetails enrich(long productId) {
        CompletableFuture<Price> priceFuture =
                CompletableFuture.supplyAsync(
                        () -> pricingClient.getPrice(productId),
                        applicationExecutor);

        CompletableFuture<Promotion> promotionFuture =
                CompletableFuture.supplyAsync(
                        () -> promotionClient.getPromotion(productId),
                        applicationExecutor);

        return new CartDetails(priceFuture.join(), promotionFuture.join());
    }
}
```

Use timeouts, bounded pools, exception handling, and fallbacks. More threads do not automatically make a system faster.

### Key distinctions

- `synchronized` provides mutual exclusion and visibility around a lock.
- `volatile` provides visibility but does not make `count++` atomic.
- `AtomicInteger` is appropriate for simple atomic counters.
- `ConcurrentHashMap` supports safe concurrent access more efficiently than one global map lock.
- Deadlocks are reduced through consistent lock order and small lock scopes.

### Production example

At Cognizant, pricing, promotion, and availability checks could be run concurrently only when they were independent. The final cart response was combined after completion, with controlled thread usage and failure handling.

### Follow-ups

- Race condition versus deadlock?
- `volatile` versus `synchronized`?
- CPU-bound versus I/O-bound pool sizing?
- Why not use the common ForkJoin pool blindly?
