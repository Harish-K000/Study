# Database and Performance

[Master index](../README.md) · [Enterprise Java playbook](../FINAL-INTERVIEW-HANDBOOK.md) · [Answer book](../FINAL-INTERVIEW-ANSWER-BOOK.md) · Previous: [Spring Boot](../02-spring-boot/README.md) · Next: [AWS and DevOps](../04-aws-devops/README.md)

## API validation and database checks

### Interview answer

The backend first validates the request’s shape and basic constraints, then checks business facts that require trusted data. For a Siemens device request, I would validate that the device ID is syntactically valid, query whether the device exists, verify the authenticated user has access to its tenant or plant, and only then fetch readings. The frontend may perform early validation for user experience, but the backend remains authoritative.

```text
Authenticate → validate request → check device → authorize plant access
→ fetch readings → map DTO → return JSON
```

## Indexes and query plans

### Interview answer

An index is a data structure that lets the database locate rows without scanning the full table. It works like a book index: rather than reading every page, you jump to the relevant location. Indexes improve reads but consume storage and make writes more expensive because every affected index must be maintained.

Use `EXPLAIN` or the database’s execution-plan tool to determine whether a query uses an index, performs a full scan, reads too many rows, or has an expensive join order. Do not add an index based only on intuition.

```sql
EXPLAIN
SELECT device_id, recorded_at, temperature
FROM sensor_reading
WHERE device_id = ?
  AND recorded_at >= ?
  AND recorded_at < ?
ORDER BY recorded_at DESC;
```

A useful composite index could begin with `(device_id, recorded_at)` because it matches the equality and range access pattern. Column order matters.

### Follow-ups

- Why not index every column?
- Composite index and leftmost-prefix behavior?
- Clustered versus non-clustered index?
- Covering index?
- Why can an index be ignored?

## Joins

### Interview answer

An inner join returns only matching rows from both sides. A left join returns all rows from the left table and matching rows from the right, using `NULL` when no match exists. I choose a join from the business requirement, not from habit.

```sql
SELECT d.device_id, d.name, a.severity
FROM device d
LEFT JOIN alert a
  ON a.device_id = d.device_id
 AND a.status = 'OPEN'
WHERE d.plant_id = ?;
```

This returns every device in the plant, including devices with no open alert.

## Transactions and ACID

### Interview answer

A database transaction treats related operations as one unit. Atomicity means all changes succeed or none do; consistency preserves rules; isolation controls interactions between concurrent transactions; durability ensures committed data survives failure. For a seat reservation, an atomic conditional update prevents two customers from claiming the same available seat.

```sql
UPDATE seat_inventory
SET status = 'HELD',
    hold_id = ?,
    hold_expires_at = ?
WHERE flight_id = ?
  AND seat_number = ?
  AND status = 'AVAILABLE';
```

Exactly one successful affected row means the hold was acquired. Zero means another request won or the seat was unavailable.

### Important distributed-systems correction

A local `@Transactional` block cannot make seat inventory, an external payment provider, and a notification service one ACID transaction. Use a state machine and saga:

```text
Create pending booking → hold seat → request idempotent payment
→ confirm booking OR compensate by releasing hold/refunding
```

Notifications are retried independently and do not undo a valid booking.

## Optimistic and pessimistic locking

Optimistic locking uses a version column and detects a conflicting update when saving. It is suitable when conflicts are uncommon. Pessimistic locking holds a database lock while work executes and may be appropriate for short, highly contended critical sections, but it reduces concurrency and can create deadlocks.

```java
@Version
private long version;
```

## Connection pooling

### Interview answer

Opening a database connection for every request is expensive. A pool such as HikariCP maintains reusable connections that requests borrow and return. It is like a taxi stand with a controlled fleet: reuse is efficient, but the fleet must be sized so the database is not overwhelmed.

Monitor active connections, wait time, timeouts, query latency, and database capacity. A larger pool is not automatically better.

## N+1 and JPA performance

The N+1 problem occurs when one query loads N parent objects and then N additional queries load their children. Use fetch joins, entity graphs, batch fetching, or DTO projections based on the endpoint.

```java
@Query("""
       select d
       from DeviceEntity d
       left join fetch d.openAlerts
       where d.plantId = :plantId
       """)
List<DeviceEntity> findDevicesWithOpenAlerts(String plantId);
```

Avoid exposing entities directly as JSON because serialization can trigger unplanned lazy loads or circular relationships.

## Redis caching

### Interview answer

Redis is an in-memory data store useful for frequently read, short-lived data, rate-limit counters, sessions, and distributed coordination. A cache is like keeping commonly requested documents at the front desk rather than retrieving them from an archive every time. Every cache needs a TTL, invalidation policy, capacity plan, and a decision about acceptable staleness.

For flight search, cached results can improve speed, but final fare and seat availability must be revalidated against the source of truth before booking.

## Slow API investigation

1. Measure p50, p95, and p99 latency and error rate.
2. Use a correlation ID and distributed trace to locate the slow component.
3. Inspect downstream-call timing and connection pools.
4. Inspect SQL execution plans, rows scanned, indexes, and N+1 behavior.
5. Reduce unnecessary payloads and round trips.
6. Cache only when freshness rules allow it.
7. Load-test the change and compare the same metrics against the baseline.

### Follow-ups

- How did you prove a 35% improvement?
- What is a deadlock?
- Which isolation level would you use?
- How do cache stampedes happen?
- How do you handle stale cache entries?
