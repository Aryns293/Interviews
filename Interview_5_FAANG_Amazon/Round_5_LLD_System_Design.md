# Round 5 — LLD & Light System Design
**Interview:** FAANG-Style / Amazon Bar Raiser Loop
**Duration:** 60–75 minutes
**Style:** Harder LLD, rapid-fire pattern questions, AND a scale-up of your own project as the HLD.

---

## LLD Problem — Parking Lot (Full Depth)

> "Design a multi-floor parking lot system. Multi-floor, multiple vehicle types, a payment strategy, and a spot assignment algorithm."

**Classes:**
```
VehicleType: enum { MOTORCYCLE, CAR, TRUCK }
Vehicle(licensePlate, type)

ParkingSpot(id, floor, spotType, isOccupied)
ParkingFloor(id, spots: ParkingSpot[])
  - getAvailableSpot(vehicleType): ParkingSpot | null

ParkingTicket(id, vehicle, spot, entryTime)

PaymentStrategy (interface)
  - calculateFee(ticket): float
HourlyPayment implements PaymentStrategy
FlatRatePayment implements PaymentStrategy

ParkingLot (Singleton)
  - floors: ParkingFloor[]
  - parkVehicle(vehicle): ParkingTicket
  - exitVehicle(ticket, paymentStrategy): float
```

**Design questions I'll ask:**
1. "A motorcycle can park in a motorcycle spot OR a car spot if motorcycle spots are full. How does your spot assignment algorithm handle this priority logic?"
2. "Where does `PaymentStrategy` get injected — at `ParkingLot` init time, or per `exitVehicle` call? What's the tradeoff?"

---

## Thread-Safety — Last 3 Spots, 50 Concurrent Entries

> "50 cars arrive simultaneously. There are 3 spots left. How do you guarantee exactly 3 get tickets and 47 get turned away, with no race conditions?"

**Option 1 — Database-level:**
```sql
BEGIN;
SELECT * FROM spots WHERE is_occupied = false LIMIT 1 FOR UPDATE;
UPDATE spots SET is_occupied = true WHERE id = <spot_id>;
INSERT INTO tickets ...;
COMMIT;
```
`FOR UPDATE` places a row lock — only one transaction can hold it at a time. Serializes spot assignment.

**Option 2 — Application-level:**
A `synchronized` block (Java) or mutex (Node.js with a Redis lock) around the check-and-assign operation.

**Which is better?** Database-level for correctness and simplicity. Application-level if you need sub-millisecond latency and accept eventual consistency tradeoffs.

---

## Design Patterns — Rapid-Fire From YOUR Projects

I'll say a pattern name. You give me a real example from QueueFlow, CodeSync AI, or gitlight. No prep time. 30 seconds each.

| Pattern | Your real example |
|---|---|
| **Strategy** | QueueFlow retry strategies (fixed / exponential / fibonacci) |
| **Observer** | CodeSync AI Socket.IO room broadcast on change events |
| **Factory** | QueueFlow job type creation (different job schemas per type) |
| **Singleton** | Postgres connection pool in any project |
| **State** | QueueFlow job state machine (pending → running → done/failed/dead-lettered) |
| **Command** | gitlight command dispatcher (add, commit, log, diff) |

If you can't give a real example from your own code for any of these, that's a flag.

---

## Light HLD — QueueFlow at 1,000,000 Jobs/Day

> "Design QueueFlow to handle 1,000,000 jobs per day across multiple regions with sub-second p99 enqueue latency. What changes?"

**Current bottlenecks at scale:**
1. Single Redis instance: ~100k ops/sec max → shard by queue name or hash of job ID
2. Single Postgres instance: ~10k writes/sec with pooling → read replica for status queries, write to primary only
3. Single worker process: limited by CPU cores → horizontal scaling with Kubernetes

**Cross-region design:**
- Each region has its own Redis cluster and Postgres replica
- Jobs are enqueued to the nearest region's Redis
- Priority-tier jobs with SLA requirements might need global coordination

**Priority tiers surviving regional failover:**
- If the `high` priority Redis shard fails, you need a fallback — either promote a replica automatically (Redis Sentinel) or reroute to another region's queue with a TTL-based re-enqueue

**p99 latency:**
- Enqueue (LPUSH): Redis is in-memory, sub-millisecond even at scale — the bottleneck is network RTT
- Deploy Redis in the same availability zone as your app servers
- Async Postgres write: decouple the durable write from the enqueue response — return to the caller after LPUSH, write to Postgres asynchronously via a drain worker

**Exactly what I'm listening for:**
- You identify sharding before I ask
- You think about the Postgres write path as a separate concern from the Redis enqueue path
- You acknowledge the consistency tradeoff in async Postgres writes
