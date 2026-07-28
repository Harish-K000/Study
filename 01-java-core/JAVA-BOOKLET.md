# Complete Java Booklet

[Master index](../README.md) · [Enterprise Java playbook](../FINAL-INTERVIEW-HANDBOOK.md) · [Answer book](../FINAL-INTERVIEW-ANSWER-BOOK.md) · [Java interview notes](README.md)

This booklet teaches Java as one connected story. We will build a small industrial device-monitoring application, beginning with basic values and gradually improving it using object-oriented and enterprise Java principles.

---

# 1. How a Java program runs

You write source code in a `.java` file:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Device monitor started");
    }
}
```

The Java compiler converts the source code into bytecode:

```text
Main.java → javac compiler → Main.class bytecode
```

The Java Virtual Machine, or JVM, executes that bytecode:

```text
Main.class → JVM → operating system and hardware
```

## JDK, JRE, and JVM

- JVM executes Java bytecode.
- JRE contains the JVM and libraries needed to run Java applications.
- JDK contains the tools needed to develop Java applications, including the compiler.

### Simple analogy

The Java source file is a recipe written by the developer. The compiler translates the recipe into standardized kitchen instructions called bytecode. The JVM is the kitchen that understands those instructions on Windows, macOS, or Linux.

### Interview answer

Java source code is compiled into platform-independent bytecode, and the JVM executes that bytecode on the target machine. The JDK contains development tools such as the compiler, while the JVM is responsible for execution and runtime services such as memory management.

---

# 2. Variables and data types

A variable is a named location used to hold a value.

```java
int temperature = 95;
double vibration = 7.5;
boolean active = true;
char grade = 'A';
String deviceName = "Motor";
```

## Primitive types

Primitive variables hold simple values directly:

| Type | Example | Purpose |
|---|---|---|
| `byte` | `byte level = 10;` | Small integer |
| `short` | `short rpm = 3000;` | Medium integer |
| `int` | `int count = 500;` | Common integer |
| `long` | `long events = 2_000_000L;` | Large integer |
| `float` | `float value = 2.5F;` | Decimal |
| `double` | `double temp = 95.4;` | Common decimal |
| `char` | `char grade = 'A';` | Single character |
| `boolean` | `boolean active = true;` | True or false |

## Reference types

Objects, arrays, and Strings are reference types:

```java
Device device = new Device();
int[] readings = new int[10];
String name = "Motor";
```

The variable does not contain the full object. It contains a reference that allows Java to find the object.

---

# 3. References: Java’s pointer-like concept

Java does not expose raw pointers like C or C++. You cannot manually perform pointer arithmetic or directly access a memory address. Java uses managed references.

```java
Device firstDevice = new Device();
Device secondDevice = firstDevice;
```

Both variables refer to the same object:

```text
STACK                       HEAP

firstDevice  ─────────┐
                     ├────→ Device object
secondDevice ─────────┘
```

If one reference changes the object, the other sees the change:

```java
firstDevice.name = "Motor";
System.out.println(secondDevice.name); // Motor
```

This happens because there is one object and two references—not two objects.

## Two separate objects

```java
Device firstDevice = new Device();
Device secondDevice = new Device();
```

Now each variable refers to a different object:

```text
firstDevice  ─────→ Device object 1
secondDevice ─────→ Device object 2
```

## `null`

`null` means a reference currently points to no object:

```java
Device device = null;
```

Calling a method through that reference causes a `NullPointerException`:

```java
device.getName(); // NullPointerException
```

### Easy explanation

A reference is like a house address written on a piece of paper. The paper is not the house; it tells you how to reach the house. Two papers can contain the same address and lead to the same house. `null` means the paper has no address.

### Interview answer

Java uses references rather than exposing raw pointers. A reference identifies an object managed by the JVM, but Java does not allow direct memory-address manipulation or pointer arithmetic. Multiple references may point to the same mutable object.

---

# 4. Stack, heap, and method calls

Consider:

```java
public static void main(String[] args) {
    int limit = 95;
    Device device = new Device();
}
```

The stack contains the active `main` method frame, the primitive `limit`, and the `device` reference. The heap contains the `Device` object.

```text
STACK                       HEAP

main frame
├── limit = 95
└── device ───────────────→ Device object
```

When `main` finishes, its stack frame is removed. If nothing else refers to the `Device`, it becomes eligible for garbage collection.

## Java is pass-by-value

Java always passes arguments by value.

For a primitive, Java copies the primitive value:

```java
public static void changeNumber(int number) {
    number = 100;
}

int original = 10;
changeNumber(original);
System.out.println(original); // 10
```

For an object, Java copies the reference value:

```java
public static void rename(Device device) {
    device.setName("Pump");
}

Device original = new Device("Motor");
rename(original);
System.out.println(original.getName()); // Pump
```

The method receives a copy of the reference, but both references reach the same mutable object.

Reassigning the copied reference does not change the caller’s reference:

```java
public static void replace(Device device) {
    device = new Device("Compressor");
}

Device original = new Device("Motor");
replace(original);
System.out.println(original.getName()); // Motor
```

### Interview answer

Java is always pass-by-value. For object arguments, the value being copied is the object reference. The called method can mutate the shared object through its copied reference, but it cannot replace the caller’s reference by reassigning its local parameter.

---

# 5. Classes and objects

A class is a blueprint. An object is a real instance created from that blueprint.

```java
public class Device {
    String name;
    double temperature;

    void displayStatus() {
        System.out.println(name + ": " + temperature);
    }
}
```

Create objects:

```java
Device motor = new Device();
motor.name = "Motor";
motor.temperature = 92.5;

Device pump = new Device();
pump.name = "Pump";
pump.temperature = 70.0;

motor.displayStatus();
pump.displayStatus();
```

`motor` and `pump` are separate objects with their own state.

### Analogy

A building plan is the class. Every building constructed from the plan is an object. The buildings share the same design but have different addresses, paint, and occupants.

### Interview answer

A class defines the state and behavior of a type, while an object is a runtime instance of that class. Instance fields hold an object’s state, and instance methods define operations on that state.

---

# 6. Constructors and `this`

A constructor initializes a new object and has the same name as the class.

```java
public class Device {
    private String name;
    private double temperature;

    public Device(String name, double temperature) {
        this.name = name;
        this.temperature = temperature;
    }
}
```

Use it:

```java
Device motor = new Device("Motor", 92.5);
```

Inside the constructor:

```java
this.name = name;
```

- `this.name` is the current object’s field.
- `name` is the constructor parameter.

## Constructor overloading

```java
public Device(String name) {
    this(name, 0.0);
}

public Device(String name, double temperature) {
    this.name = name;
    this.temperature = temperature;
}
```

`this(name, 0.0)` calls another constructor in the same class.

### Interview answer

A constructor establishes a valid initial state for a new object. `this` refers to the current object and can distinguish fields from parameters or call another constructor.

---

# 7. Encapsulation

Encapsulation means keeping an object’s internal state protected and allowing controlled access through methods.

Bad design:

```java
public class Device {
    public double temperature;
}
```

Any code could assign an invalid value:

```java
device.temperature = -1000;
```

Better design:

```java
public class Device {
    private final String name;
    private double temperature;

    public Device(String name, double temperature) {
        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException("Name is required");
        }
        validateTemperature(temperature);

        this.name = name;
        this.temperature = temperature;
    }

    public String getName() {
        return name;
    }

    public double getTemperature() {
        return temperature;
    }

    public void updateTemperature(double temperature) {
        validateTemperature(temperature);
        this.temperature = temperature;
    }

    private void validateTemperature(double temperature) {
        if (temperature < -100 || temperature > 300) {
            throw new IllegalArgumentException(
                    "Temperature is outside the supported range");
        }
    }
}
```

Now every update must follow the class’s rule.

### Analogy

A bank account does not expose its balance so anyone can type a new value. It provides controlled deposit and withdrawal operations that enforce rules.

### Interview answer

Encapsulation hides internal state and exposes controlled behavior. It protects invariants, reduces coupling, and allows an implementation to change without forcing every caller to change.

---

# 8. Access modifiers

| Modifier | Accessible from |
|---|---|
| `private` | Same class only |
| package-private | Same package |
| `protected` | Same package and subclasses |
| `public` | Everywhere |

Use the narrowest access level that meets the requirement.

```java
public class Device {
    private String serialNumber;

    public String getSerialNumber() {
        return serialNumber;
    }
}
```

Enterprise code normally keeps fields private.

---

# 9. The four OOP concepts

The four commonly discussed OOP principles are:

1. Encapsulation
2. Inheritance
3. Polymorphism
4. Abstraction

They are related but solve different problems.

---

# 10. Inheritance

Inheritance allows one class to reuse and specialize another class.

```java
public class Device {
    private final String id;
    private final String name;

    public Device(String id, String name) {
        this.id = id;
        this.name = name;
    }

    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getStatus() {
        return "UNKNOWN";
    }
}
```

A motor is a specialized device:

```java
public class Motor extends Device {
    private final int rpm;

    public Motor(String id, String name, int rpm) {
        super(id, name);
        this.rpm = rpm;
    }

    public int getRpm() {
        return rpm;
    }

    @Override
    public String getStatus() {
        return rpm > 5000 ? "HIGH_SPEED" : "NORMAL";
    }
}
```

- `extends` creates the inheritance relationship.
- `super(...)` calls the parent constructor.
- `@Override` tells the compiler the child replaces inherited behavior.

### Analogy

A motor is a device, but not every device is a motor. A motor inherits general device information and adds RPM-specific behavior.

### Use inheritance carefully

Inheritance is appropriate for a genuine “is-a” relationship. Do not use it only to reuse a few lines of code. Deep inheritance trees are difficult to understand. Composition is often safer.

---

# 11. Polymorphism

Polymorphism means the same parent type can represent different child implementations, and Java selects the correct overridden behavior at runtime.

```java
public class Pump extends Device {
    private final double pressure;

    public Pump(String id, String name, double pressure) {
        super(id, name);
        this.pressure = pressure;
    }

    @Override
    public String getStatus() {
        return pressure > 120 ? "HIGH_PRESSURE" : "NORMAL";
    }
}
```

Use both child types through the parent type:

```java
List<Device> devices = List.of(
        new Motor("M-1", "Main Motor", 6000),
        new Pump("P-1", "Water Pump", 80)
);

for (Device device : devices) {
    System.out.println(device.getStatus());
}
```

At runtime:

- A `Motor` runs `Motor.getStatus()`.
- A `Pump` runs `Pump.getStatus()`.

The loop does not need `if` statements checking every concrete type.

### Analogy

The same “start” command works on different machines, but each machine performs its own implementation.

### Interview answer

Polymorphism lets code depend on a parent type or interface while runtime objects provide different behavior. It reduces conditional logic and makes new implementations easier to add.

## Overriding versus overloading

Overriding changes inherited behavior:

```java
@Override
public String getStatus() {
    return "NORMAL";
}
```

Overloading uses the same method name with different parameter lists:

```java
public void updateTemperature(double temperature) {
}

public void updateTemperature(
        double temperature,
        Instant recordedAt) {
}
```

- Overriding is runtime polymorphism.
- Overloading is selected at compile time.

---

# 12. Abstraction

Abstraction exposes what an object can do while hiding how it does it.

An abstract class may contain shared state, implemented methods, and abstract methods:

```java
public abstract class AlertRule {
    private final String ruleName;

    protected AlertRule(String ruleName) {
        this.ruleName = ruleName;
    }

    public String getRuleName() {
        return ruleName;
    }

    public abstract boolean isTriggered(SensorReading reading);
}
```

Concrete rule:

```java
public final class TemperatureAlertRule extends AlertRule {
    private final double threshold;

    public TemperatureAlertRule(double threshold) {
        super("High temperature");
        this.threshold = threshold;
    }

    @Override
    public boolean isTriggered(SensorReading reading) {
        return reading.temperature() > threshold;
    }
}
```

The caller knows it can ask any `AlertRule` whether it is triggered. It does not need to know the exact comparison logic.

### Analogy

A driver uses the steering wheel and pedals without needing to understand the engine’s internal combustion process.

### Interview answer

Abstraction presents the essential operation while hiding implementation details. In Java, interfaces and abstract classes allow callers to depend on stable contracts instead of concrete implementations.

---

# 13. Interfaces

An interface defines a capability or contract.

```java
public interface NotificationSender {
    void send(Alert alert);
}
```

Email implementation:

```java
public final class EmailNotificationSender
        implements NotificationSender {

    @Override
    public void send(Alert alert) {
        System.out.println(
                "Sending email for alert: " + alert.message());
    }
}
```

SMS implementation:

```java
public final class SmsNotificationSender
        implements NotificationSender {

    @Override
    public void send(Alert alert) {
        System.out.println(
                "Sending SMS for alert: " + alert.message());
    }
}
```

Service using the abstraction:

```java
public final class AlertService {
    private final NotificationSender notificationSender;

    public AlertService(NotificationSender notificationSender) {
        this.notificationSender = notificationSender;
    }

    public void publish(Alert alert) {
        notificationSender.send(alert);
    }
}
```

The service does not create an email sender directly. It receives the contract it needs.

### Why this matters

The same service can use email, SMS, or a test double:

```java
NotificationSender sender = new EmailNotificationSender();
AlertService service = new AlertService(sender);
```

This leads naturally to dependency injection in Spring.

## Interface versus abstract class

Use an interface when you need a capability contract that unrelated classes may implement. Use an abstract class when closely related subclasses need shared state or base implementation.

| Interface | Abstract class |
|---|---|
| Defines a contract/capability | Defines a shared base type |
| A class can implement several | A class can extend only one class |
| Normally avoids instance state | Can contain instance fields |
| Excellent dependency boundary | Useful for shared implementation |

---

# 14. Composition over inheritance

Composition means an object contains or uses another object.

```java
public final class DeviceMonitoringService {
    private final AlertEvaluator alertEvaluator;
    private final NotificationSender notificationSender;

    public DeviceMonitoringService(
            AlertEvaluator alertEvaluator,
            NotificationSender notificationSender) {
        this.alertEvaluator = alertEvaluator;
        this.notificationSender = notificationSender;
    }
}
```

The service “has an” evaluator and sender. It does not extend either one.

### Why composition is often preferred

- Dependencies can be replaced independently.
- Classes remain focused.
- Testing is easier.
- There is less coupling than in a deep inheritance tree.

### Interview answer

I use inheritance for a stable and genuine “is-a” relationship. I prefer composition for “has-a” or “uses-a” relationships because it provides more flexibility and lower coupling.

---

# 15. `static`, `final`, and constants

## `static`

A static member belongs to the class rather than one object.

```java
public final class TemperatureConverter {
    private TemperatureConverter() {
    }

    public static double celsiusToFahrenheit(double celsius) {
        return celsius * 9 / 5 + 32;
    }
}
```

Call it without creating an object:

```java
double value =
        TemperatureConverter.celsiusToFahrenheit(95);
```

Avoid using mutable static fields as global application state.

## `final`

`final` has different meanings:

```java
final int limit = 95;           // variable cannot be reassigned
public final class DeviceId {}  // class cannot be extended
public final void validate() {} // method cannot be overridden
```

A final reference cannot point to another object, but the referenced mutable object may still change:

```java
final List<String> names = new ArrayList<>();
names.add("Motor"); // allowed
// names = new ArrayList<>(); // not allowed
```

## Constant

```java
public static final int DEFAULT_ALERT_LIMIT = 95;
```

Constants use uppercase names with underscores.

---

# 16. Immutability

An immutable object cannot change after construction.

Java records are convenient for simple immutable data carriers:

```java
public record SensorReading(
        String deviceId,
        double temperature,
        Instant recordedAt) {

    public SensorReading {
        if (deviceId == null || deviceId.isBlank()) {
            throw new IllegalArgumentException(
                    "Device ID is required");
        }
        if (recordedAt == null) {
            throw new IllegalArgumentException(
                    "Recorded time is required");
        }
    }
}
```

Benefits:

- Easier reasoning
- Safer sharing between threads
- Stable hash codes
- Fewer accidental state changes

If a field contains a mutable collection, make a defensive copy:

```java
public record DeviceGroup(List<String> deviceIds) {
    public DeviceGroup {
        deviceIds = List.copyOf(deviceIds);
    }
}
```

---

# 17. `Object`, `equals()`, `hashCode()`, and `toString()`

Every Java class ultimately extends `Object`.

Important methods:

- `equals()` compares logical equality.
- `hashCode()` supports hash-based collections.
- `toString()` creates a readable representation.

For value objects, a record generates these correctly:

```java
public record DeviceId(String value) {
}
```

```java
DeviceId first = new DeviceId("M-1");
DeviceId second = new DeviceId("M-1");

System.out.println(first.equals(second)); // true
```

## Equality contract

If two objects are equal according to `equals()`, they must return the same hash code.

This matters in a `HashMap`:

```java
Map<DeviceId, Device> devices = new HashMap<>();
devices.put(new DeviceId("M-1"), motor);

Device found = devices.get(new DeviceId("M-1"));
```

Without consistent equality and hash behavior, lookup can fail.

---

# 18. Collections and generics

Generics provide compile-time type safety.

Without a generic type, code might store unrelated values. With generics:

```java
List<Device> devices = new ArrayList<>();
devices.add(motor);
// devices.add("wrong value"); // compile error
```

## List

A `List` is ordered and permits duplicates.

```java
List<Device> devices = new ArrayList<>();
devices.add(motor);
devices.add(pump);
```

`ArrayList` is the common default for reading, iteration, and appending.

## Set

A `Set` stores unique elements:

```java
Set<DeviceId> activeDeviceIds = new HashSet<>();
activeDeviceIds.add(new DeviceId("M-1"));
activeDeviceIds.add(new DeviceId("M-1"));

System.out.println(activeDeviceIds.size()); // 1
```

## Map

A `Map` stores key-value pairs:

```java
Map<DeviceId, Device> devicesById = new HashMap<>();
devicesById.put(new DeviceId("M-1"), motor);

Device found = devicesById.get(new DeviceId("M-1"));
```

## Queue and deque

```java
Deque<SensorReading> processingQueue = new ArrayDeque<>();
processingQueue.offerLast(reading);
SensorReading next = processingQueue.pollFirst();
```

For normal queue behavior, `ArrayDeque` is generally preferred over `LinkedList`.

---

# 19. Exceptions

An exception interrupts the normal flow because an operation cannot complete as expected.

## Checked exception

The compiler requires handling or declaration:

```java
public String readConfiguration(Path path)
        throws IOException {
    return Files.readString(path);
}
```

## Unchecked exception

An unchecked exception extends `RuntimeException`:

```java
public final class DeviceNotFoundException
        extends RuntimeException {

    public DeviceNotFoundException(DeviceId deviceId) {
        super("Device not found: " + deviceId.value());
    }
}
```

## `throw` versus `throws`

- `throw` creates and sends an exception now.
- `throws` declares that a method may propagate an exception.

```java
public Device findDevice(DeviceId id) {
    throw new DeviceNotFoundException(id);
}
```

## Good exception handling

```java
try {
    configuration = readConfiguration(path);
} catch (IOException exception) {
    throw new ConfigurationException(
            "Unable to read configuration: " + path,
            exception);
}
```

Preserve the original cause. Do not silently swallow exceptions.

---

# 20. Lambdas, functional interfaces, and Streams

A lambda is a compact implementation of a functional interface—an interface with one abstract method.

```java
Predicate<SensorReading> isCritical =
        reading -> reading.temperature() > 95;
```

The same idea with a Stream:

```java
List<SensorReading> criticalReadings =
        readings.stream()
                .filter(reading ->
                        reading.temperature() > 95)
                .sorted(Comparator.comparing(
                        SensorReading::recordedAt))
                .toList();
```

Step by step:

1. `readings.stream()` creates a processing pipeline.
2. `filter(...)` keeps readings above 95.
3. `sorted(...)` orders them by time.
4. `toList()` executes the pipeline and returns a list.

Map objects into response values:

```java
List<String> criticalDeviceIds =
        readings.stream()
                .filter(reading ->
                        reading.temperature() > 95)
                .map(SensorReading::deviceId)
                .distinct()
                .toList();
```

- `filter` decides whether an item remains.
- `map` changes what an item becomes.
- `distinct` removes duplicates.

Use Streams when the pipeline remains easy to read. Use an ordinary loop for complicated stateful logic.

---

# 21. Optional

`Optional<T>` represents a value that may be absent.

```java
public interface DeviceRepository {
    Optional<Device> findById(DeviceId id);
}
```

Service:

```java
public Device requireDevice(DeviceId id) {
    return deviceRepository.findById(id)
            .orElseThrow(() ->
                    new DeviceNotFoundException(id));
}
```

Avoid:

```java
Device device = optional.get();
```

unless presence was already proved.

Use `Optional` mainly for return values—not for every field, parameter, or DTO property.

---

# 22. Concurrency in simple terms

A thread is one execution path inside a process. Several threads may share heap objects.

## Race condition

```java
public class Counter {
    private int value;

    public void increment() {
        value++;
    }
}
```

`value++` is a read-modify-write sequence. Two threads can read the same value and lose one update.

Safer counter:

```java
public class Counter {
    private final AtomicInteger value =
            new AtomicInteger();

    public int incrementAndGet() {
        return value.incrementAndGet();
    }
}
```

## `volatile`

```java
private volatile boolean running = true;
```

`volatile` makes writes visible to other threads. It does not make compound operations such as `count++` atomic.

## Thread pools

Do not create an unlimited number of threads. An executor reuses a controlled pool:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(10);

Future<Alert> future =
        executor.submit(() -> evaluate(reading));
```

In Spring enterprise code, configure and inject the executor rather than creating it inside every service.

---

# 23. From plain Java to enterprise layering

Now combine the earlier concepts.

## Domain object

```java
public record DeviceId(String value) {
    public DeviceId {
        if (value == null || value.isBlank()) {
            throw new IllegalArgumentException(
                    "Device ID is required");
        }
    }
}
```

## Repository abstraction

```java
public interface DeviceRepository {
    Optional<Device> findById(DeviceId id);
    Device save(Device device);
}
```

## Service

```java
public final class DeviceService {
    private final DeviceRepository deviceRepository;
    private final AlertEvaluator alertEvaluator;
    private final NotificationSender notificationSender;

    public DeviceService(
            DeviceRepository deviceRepository,
            AlertEvaluator alertEvaluator,
            NotificationSender notificationSender) {
        this.deviceRepository = deviceRepository;
        this.alertEvaluator = alertEvaluator;
        this.notificationSender = notificationSender;
    }

    public DeviceStatus processReading(
            DeviceId deviceId,
            SensorReading reading) {

        Device device = deviceRepository.findById(deviceId)
                .orElseThrow(() ->
                        new DeviceNotFoundException(deviceId));

        device.updateTemperature(reading.temperature());
        Device savedDevice = deviceRepository.save(device);

        Optional<Alert> alert =
                alertEvaluator.evaluate(savedDevice, reading);

        alert.ifPresent(notificationSender::send);

        return new DeviceStatus(
                savedDevice.getId(),
                savedDevice.getName(),
                savedDevice.getTemperature(),
                alert.isPresent());
    }
}
```

## What this code demonstrates

- Encapsulation: `Device` controls temperature updates.
- Abstraction: repository, evaluator, and sender are interfaces.
- Composition: `DeviceService` uses those collaborators.
- Dependency injection: collaborators arrive through the constructor.
- Polymorphism: different repository or sender implementations can be supplied.
- Optional: alert may be absent.
- Exception: missing device becomes a meaningful domain error.
- Immutability: IDs, readings, and response values can be records.
- Single responsibility: the service coordinates one use case.

This is the bridge between OOP theory and Spring Boot architecture.

---

# 24. SOLID principles in easy language

## S — Single Responsibility

A class should have one main reason to change.

`DeviceRepository` accesses data. `AlertEvaluator` evaluates rules. `NotificationSender` sends messages. One giant class should not do all three.

## O — Open/Closed

Code should allow new behavior without repeatedly changing stable code.

Adding `SmsNotificationSender` should not require rewriting `AlertService`.

## L — Liskov Substitution

An implementation should behave correctly wherever its parent contract is expected.

If a `NotificationSender` implementation silently deletes messages instead of sending them, it violates the expected behavior.

## I — Interface Segregation

Prefer focused interfaces instead of forcing a class to implement unrelated methods.

Better:

```java
interface AlertReader {
    Optional<Alert> findById(long id);
}

interface AlertWriter {
    Alert save(Alert alert);
}
```

Use this separation only when callers genuinely need different contracts.

## D — Dependency Inversion

High-level business logic should depend on abstractions, not concrete infrastructure.

`DeviceService` depends on `DeviceRepository`, not directly on `MySqlDeviceRepository`.

---

# 25. Common OOP interview questions

## What is OOP?

Object-oriented programming organizes software around objects that combine state and behavior. Its main principles are encapsulation, inheritance, polymorphism, and abstraction. In enterprise Java, these ideas help separate responsibilities and allow business logic to depend on stable contracts.

## Encapsulation versus abstraction

Encapsulation protects an object’s internal state and controls access. Abstraction hides implementation details behind a useful contract. A bank account encapsulates its balance; the ATM interface abstracts the banking system.

## Inheritance versus composition

Inheritance models an “is-a” relationship. Composition models “has-a” or “uses-a.” I prefer composition for most service dependencies because it produces lower coupling and easier testing.

## Interface versus abstract class

An interface defines a capability contract and supports multiple implementations. An abstract class provides a common base that may contain shared state and partial implementation. I choose based on whether the design needs a capability boundary or a shared family base.

## Overloading versus overriding

Overloading uses one method name with different parameter lists and is resolved at compile time. Overriding replaces inherited behavior and is selected at runtime based on the object.

## Can Java have multiple inheritance?

A class can extend only one class, avoiding multiple-class inheritance ambiguity. It may implement multiple interfaces.

## Why is constructor injection useful?

It makes dependencies explicit and mandatory, supports `final` fields, prevents partially initialized objects, and makes unit testing easy because test implementations can be passed directly.

---

# 26. Code-reading practice

Read this declaration from left to right:

```java
public final class TemperatureAlertRule
        implements AlertRule {
```

- `public`: usable from other packages.
- `final`: cannot be subclassed.
- `class`: defines an object type.
- `TemperatureAlertRule`: class name.
- `implements AlertRule`: promises to satisfy the interface contract.

Read this field:

```java
private final DeviceRepository deviceRepository;
```

- `private`: only this class can access it directly.
- `final`: the field must be assigned once and cannot be reassigned.
- `DeviceRepository`: the abstraction type.
- `deviceRepository`: variable name.

Read this method:

```java
public Optional<Device> findById(DeviceId id)
```

- `public`: callable from outside.
- `Optional<Device>`: return type; a Device may or may not be present.
- `findById`: method name.
- `DeviceId id`: required parameter.

Read this lambda:

```java
reading -> reading.temperature() > 95
```

- Input parameter: `reading`
- Operation: read its temperature
- Result: `true` when above 95, otherwise `false`

---

# 27. Final connected story

The whole application can be understood in one sequence:

```text
Primitive values describe individual measurements.
References connect variables to objects.
Classes define device objects.
Constructors create valid devices.
Encapsulation protects device state.
Interfaces define repository and notification contracts.
Polymorphism allows different implementations.
Composition connects services to those implementations.
Collections store many devices and readings.
Generics protect collection types.
Streams process readings.
Optional represents a missing device or alert.
Exceptions represent failed operations.
Concurrency processes independent work carefully.
Spring later creates and injects these Java objects as beans.
```

Once these connections are clear, Spring Boot becomes easier: it is using the same Java classes, interfaces, objects, references, polymorphism, and constructor injection—while the framework manages their creation and web lifecycle.

---

# 28. Study checkpoints

After reading, explain these without looking:

1. What is the difference between a primitive value and an object reference?
2. Does Java have pointers?
3. Why is Java pass-by-value even when an object changes?
4. Class versus object?
5. What does a constructor do?
6. What problem does encapsulation solve?
7. Inheritance versus composition?
8. Overloading versus overriding?
9. Interface versus abstract class?
10. How does polymorphism remove `if` statements?
11. Why must `equals()` and `hashCode()` agree?
12. Why use generics?
13. List, Set, Map, and Queue?
14. Checked versus unchecked exception?
15. `filter()` versus `map()`?
16. Why use `Optional`?
17. What is a race condition?
18. How does this lead to Spring dependency injection?
