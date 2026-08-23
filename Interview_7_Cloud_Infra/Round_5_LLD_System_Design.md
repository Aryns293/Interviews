# Round 5 — LLD & Light System Design
**Interview:** Cloud Infrastructure / Platform Engineering Company

---

## LLD Problem — Vending Machine (State Pattern)

> "Design a Vending Machine using the State pattern. Work it to actual classes — I want to see the state transitions, not just a description."

**States and transitions:**
```
IDLE          → item selected with valid money → SELECTING
SELECTING     → payment confirmed → DISPENSING
SELECTING     → cancel → IDLE (refund money)
DISPENSING    → item dispensed → IDLE
OUT_OF_STOCK  → (terminal until restocked externally)
```

**Classes:**
```
VendingMachineState (interface)
  - selectItem(item, machine)
  - insertCoin(amount, machine)
  - dispense(machine)
  - cancel(machine)

IdleState implements VendingMachineState
SelectingState implements VendingMachineState
DispensingState implements VendingMachineState
OutOfStockState implements VendingMachineState

VendingMachine
  - currentState: VendingMachineState
  - inventory: Map<Item, int>
  - balance: float
  - setState(state)
  - getState(): VendingMachineState
```

**Questions I'll ask:**
1. "What happens if `dispense()` is called in `IdleState`? Does it throw, silently ignore, or log?" (It should throw `InvalidStateTransitionException` or return an error — defensive state machine design)
2. "How do you handle partial payment — user inserts ₹10 for a ₹15 item?" (Balance accumulates across `insertCoin` calls. `SELECTING` state stays until balance ≥ item price.)

---

## Design Pattern Follow-Ups

**Singleton — Docker Client:**
> "Your CodeSync AI needs a Docker client object to spin up containers. Why should there be exactly one Docker client instance shared across all request handlers?"

*Expected:* The Docker client manages a connection pool to the Docker daemon. Creating a new client per request is expensive (TCP connection overhead) and may exhaust the daemon's connection limit. A Singleton ensures one shared pool.

**Observer — Health Check Dashboard:**
> "Design the notification flow when a node goes down: your health-check subsystem discovers it, and a dashboard needs to update in real time."

*Expected:* Health-check runner = Subject. Dashboard = Observer. On health-check failure:
1. `HealthChecker` calls `notifyObservers(NodeDownEvent)`
2. `DashboardUpdater` (Observer) receives the event, pushes a WebSocket message to the UI
3. `AlertingService` (another Observer) sends a PagerDuty alert

---

## Thread-Safety — Container Pool Concurrency

**Q:**
> "10 container-spin-up requests arrive simultaneously, but your host only has capacity for 6 concurrent sandbox containers. How do you enforce that limit correctly, without optimistically over-allocating?"

*Expected:* A semaphore with a count of 6.
```js
const Semaphore = require('semaphore');
const sandboxLimit = Semaphore(6);

async function executeInSandbox(code, language) {
  await sandboxLimit.acquire(); // blocks if 6 containers are already running
  try {
    return await runInDocker(code, language);
  } finally {
    sandboxLimit.release(); // always releases, even on error
  }
}
```

**Follow-up:** "What happens to the 4 requests that can't immediately acquire the semaphore? Do they queue indefinitely, timeout, or reject immediately?"
- Queue with timeout: `await semaphore.acquire(5000)` — wait up to 5 seconds, then reject with `503 Service Unavailable`

---

## Light System Design — Log Aggregation & Alerting

> "Design a log aggregation and alerting system for a fleet of microservices. Ingestion pipeline, search/indexing, alert thresholds, and how you avoid the ingestion pipeline itself becoming the outage."

**Architecture:**
```
Services → Structured Logs (JSON) → Log Shipper (Fluentd/Vector)
         → Kafka (buffer, absorbs spikes, prevents backpressure)
         → Indexer (Elasticsearch / OpenSearch) → Search UI (Kibana/Grafana)
         → Alerting Engine (checks log patterns, fires PagerDuty/Slack)
```

**Key design decisions:**
- **Why Kafka between shippers and indexer?** Indexer can be slow or go down without dropping logs. Kafka is the durable buffer.
- **Avoiding the ingestion pipeline becoming the outage:** The pipeline itself must be simpler than what it monitors. If your logging system depends on the services it's monitoring (e.g., same DB), a service failure can cascade into a logging failure — you lose observability exactly when you need it most.
- **Alert fatigue:** Don't alert on raw log counts. Alert on rate-of-change (error rate spikes from baseline) and sustained thresholds, not brief spikes.
- **Retention:** Hot storage (ES) for last 7 days (fast search). Cold storage (S3) for long-term (cheap, slow).
