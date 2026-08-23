# Round 5 — LLD & Light System Design
**Interview:** Systems / Low-Level Deep-Dive Company

---

## LLD Problem 1 — Git Object Model Class Hierarchy (Direct Project Tie-In)

> "Design the class hierarchy for Git's core object model: Blob, Tree, Commit. Use your gitlight experience. Where does content-addressing fit — baked into each class, or handled by a separate ObjectStore?"

**Expected design:**
```
ObjectStore
  - store(content: Buffer): string (returns SHA-256 hex)
  - retrieve(hash: string): Buffer

GitObject (abstract)
  - abstract serialize(): Buffer
  - hash(): string → calls ObjectStore.store(this.serialize())

Blob extends GitObject
  - content: Buffer
  - serialize(): "blob <size>\0<content>"

Tree extends GitObject
  - entries: TreeEntry[] (name, mode, childHash)
  - serialize(): binary tree object format

Commit extends GitObject
  - tree: string (hash of root Tree)
  - parent: string[] (hashes of parent Commits)
  - author, message, timestamp
  - serialize(): text commit object format
```

**Pattern follow-up:** Is content-addressing a cross-cutting concern here? Where does the Singleton pattern appear naturally? (ObjectStore — one shared store instance for the whole repository)

**Composition vs Inheritance:**
- `Blob`, `Tree`, `Commit` all inherit from `GitObject` — is this appropriate? (Yes — they share the `hash()` and `store()` behavior)
- Content of a `Tree` entry IS a `GitObject` — composition here (Tree holds hashes, not object instances directly)

---

## LLD Problem 2 — Elevator System

> "Design an elevator system. Work it to the State pattern — I want to see the exact states and transitions."

**States:**
```
IDLE → receives request → MOVING_UP or MOVING_DOWN
MOVING_UP → reaches target floor → DOOR_OPEN
MOVING_DOWN → reaches target floor → DOOR_OPEN
DOOR_OPEN → door timer expires → IDLE (if no more requests) or MOVING_UP/DOWN
```

**Classes:**
```
ElevatorController
  - currentFloor: int
  - state: ElevatorState
  - requestQueue: int[]
  - setState(state)
  - addRequest(floor)

ElevatorState (interface)
  - handleRequest(floor, controller)
  - move(controller)

IdleState implements ElevatorState
MovingUpState implements ElevatorState
MovingDownState implements ElevatorState
DoorOpenState implements ElevatorState
```

**Thread-safety follow-up:** "Two people press the call button on different floors simultaneously. How do you ensure both requests are processed and neither is lost?"
- `requestQueue` must be a thread-safe queue (`ConcurrentLinkedQueue` in Java, or mutex-protected in any language)
- The `addRequest` method must be atomic

---

## Command Pattern — Extending gitlight With Undo

> "You used the Command Pattern for git commands. If you added `unexecute()` to each command class, what would `git add --undo` do, and how would you implement it?"

*Expected:*
- `AddCommand.execute()` = hash content, write to object store, update index
- `AddCommand.unexecute()` = remove entry from index (don't remove the object — it might be referenced elsewhere)
- Maintain a command history stack — `undo` pops from the stack and calls `unexecute()`

---

## Light System Design — Rate Limiter

> "Design a rate limiter service. Token bucket vs sliding window counter vs sliding window log — what are the tradeoffs of each? Where do you store the counters at scale?"

| Algorithm | Pro | Con |
|---|---|---|
| Token bucket | Allows controlled bursts, simple | State per user, clock sync issues |
| Fixed window counter | Simplest, cheapest | Edge case: double rate at window boundary (00:59–01:01) |
| Sliding window log | Perfectly smooth rate | Stores every request timestamp — expensive for high rates |
| Sliding window counter | Good balance | Slight approximation (~10% error at window edges) |

**Storage at scale:**
- In-memory: fast but doesn't survive restarts, doesn't work across multiple servers
- Redis: `INCR + EXPIRE` for fixed window. `ZADD + ZREMRANGEBYSCORE + ZCARD` for sliding window log.
- Dedicated service (Envoy, Kong): external rate limiting, no code changes needed
