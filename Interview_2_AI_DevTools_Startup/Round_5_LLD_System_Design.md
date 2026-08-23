# Round 5 — LLD & Light System Design
**Interview:** AI / Dev-Tools Startup
**Duration:** 60–75 minutes

---

## LLD Problem — Design a Real-Time Collaborative Code Editor

**The ask:** Design the core class/object model for CodeSync AI. Not the UI — the backend domain model.

**Core entities to design:**
```
- Room       — represents one collaborative session
- User       — a participant in a room
- Document   — the shared code document with current content
- ChangeEvent — an edit operation (insert/delete at position)
- Session    — a user's connection state within a room
- ExecutionRequest / ExecutionResult — code run events
```

**Questions I'll ask as you design:**

**Q1:** Where does the source of truth for the document content live? In the Room? The Document? In a database? What are the tradeoffs of each?

**Q2:** When a `ChangeEvent` comes in from User A, what happens step by step before User B's editor reflects that change?

**Q3:** Where would Operational Transform or CRDT slot into this design if you added conflict resolution later? Which class is responsible for applying transformations?

---

## Design Pattern Rapid-Fire

**Q1 — Strategy:**
> "What pattern would you use to swap between the Docker executor and the Judge0 executor at runtime, based on availability?"

*Expected:* Strategy Pattern. Define an `Executor` interface with `execute(code, language): Promise<result>`. `DockerExecutor` and `Judge0Executor` implement it. `ExecutionService` holds a reference to the current strategy and can switch at runtime.

**Q2 — Observer:**
> "What pattern models the relationship between a single Socket.IO room and all the connected clients who should receive every code change event?"

*Expected:* Observer Pattern. The `Room` is the Subject. Connected `Session` objects are Observers. When a `ChangeEvent` is received, the `Room` notifies all observers (broadcasts via Socket.IO).

**Q3 (your own project, no safety net):**
> "In your actual CodeSync AI codebase, which socket event handler most closely implements the Observer pattern? Walk me through the exact socket events and callbacks."

---

## Concurrency — Code Execution in the Same Room

**Q:**
> "Two users hit 'Run Code' in the same room within the same second. Do they share a container? Does each get their own? What's your isolation guarantee?"

*What I want:*
- Each execution must be isolated — sharing a container between users means User A's global variables leak into User B's execution
- Each `ExecutionRequest` should spin up its own ephemeral container, run, and destroy it
- The isolation guarantee is at the container level — not the process level, not the thread level
- **Scaling concern:** 1,000 users hitting "Run Code" simultaneously = 1,000 Docker containers. You need a container pool or a maximum concurrency limit + queue.

---

## Light System Design — CodeSync AI at Scale

> "Design the backend for CodeSync AI at 100x your current scale — thousands of concurrent collaborative sessions across multiple regions. What breaks first, and what do you change?"

**What breaks first:**
- Socket.IO rooms are in-process memory — if you have 2 server instances, a user connected to Server 1 can't receive events from a user in the same room connected to Server 2

**Fix:**
- Use Redis Pub/Sub (or Redis Adapter for Socket.IO) as a shared message bus — all server instances subscribe to the same channels
- OR: Sticky sessions (route a room to one specific server) — simpler, but creates hotspots

**Next bottleneck — sandboxing cost:**
- Docker container spin-up is ~300–500ms per run
- Solution: pre-warmed container pool. Keep N containers alive, ready to accept code. Return them to the pool after execution.
- Cost vs cold-start latency tradeoff.

**AI review calls at scale:**
- Rate-limiting Gemini API calls per room per minute
- Caching: if the same code block was reviewed 5 seconds ago, serve the cached result
- Async: AI review doesn't block code execution — trigger it in the background, push result via socket when ready
