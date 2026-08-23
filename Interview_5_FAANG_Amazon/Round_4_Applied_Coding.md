# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** FAANG-Style / Amazon Bar Raiser Loop
**Duration:** 60–90 minutes
**Style:** Under time pressure. Every problem has a follow-up that tests whether you know WHY, not just HOW.

---

## Build 1 — Distributed Lock With Redis (25 minutes)

### The Problem
Implement a distributed lock using Redis that can be acquired and released safely across multiple processes.

**Requirements:**
1. `acquire(lockKey, ttlMs)` → returns a unique token if successful, `null` if lock already held
2. `release(lockKey, token)` → releases the lock ONLY if the token matches (prevents releasing another process's lock)
3. The lock must auto-expire after `ttlMs` milliseconds even if the holder crashes

```js
const redis = require('redis');
const { v4: uuid } = require('uuid');
const client = redis.createClient();

async function acquire(lockKey, ttlMs) {
  // TODO: SET lockKey <unique-token> NX PX ttlMs
}

async function release(lockKey, token) {
  // TODO: only release if the stored token matches ours
  // CRITICAL: this must be atomic — use a Lua script or GET+DEL is a race condition
}
```

**The classic bug I'm watching for:**
```js
// WRONG — race condition between GET and DEL
const stored = await client.get(lockKey);
if (stored === token) {
  await client.del(lockKey); // Another process could acquire between GET and DEL!
}
```

**Correct approach:** Atomic Lua script via `client.eval()`:
```lua
if redis.call("get", KEYS[1]) == ARGV[1] then
  return redis.call("del", KEYS[1])
else
  return 0
end
```

**Follow-up:** "What happens if the process that holds the lock pauses for longer than the TTL — GC pause, network stall — and another process acquires the lock? Now two processes think they own the lock. How do you handle this?"

*Expected:* This is the fundamental distributed systems problem (no perfect solution). Mitigations: fencing tokens (monotonically increasing ID, downstream service rejects older tokens), heartbeat-based lock renewal, or accepting that rare duplicate processing is okay for your use case.

---

## Build 2 — Retry With Exponential Backoff + Jitter (15 minutes)

### The Problem
Implement a generic `withRetry(fn, options)` utility. You built this at Rolewize. Do it from memory.

```js
async function withRetry(fn, { maxAttempts = 3, baseDelayMs = 2000, jitterFactor = 0.2 } = {}) {
  // Attempt sequence: 2s → 4s → 8s, each ±20% jitter
  // On last attempt failure, throw the error
}
```

**Jitter implementation:**
```js
const jitter = 1 + (Math.random() * 2 - 1) * jitterFactor; // ±20%
const delay = baseDelayMs * Math.pow(2, attempt) * jitter;
```

**Follow-up:** "Why does jitter matter? What's the thundering herd problem, and how does jitter solve it?"

*Expected:* Without jitter, all retrying clients fire at exactly 2s, 4s, 8s simultaneously — creating synchronized load spikes on the downstream service. Jitter randomizes each client's retry timing, spreading the load.

---

## Debug 1 — Stale Closure in useEffect (15 minutes)

### The Problem
This component has a stale closure bug. The count is always one render behind when logged. Find and fix it.

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      console.log('Current count:', count); // Always logs 0!
      setCount(count + 1); // Always increments from 0!
    }, 1000);
    return () => clearInterval(interval);
  }, []); // Bug: empty dependency array

  return <div>{count}</div>;
}
```

**The bug:** `useEffect` with `[]` runs once. The callback captures `count = 0` in its closure at mount time and never re-captures. Every tick logs 0 and sets count to 1.

**Fix 1:** Add `count` to the dependency array — but this recreates the interval on every count change.
**Fix 2 (correct):** Use the functional update form of `setCount`:
```jsx
setCount(prev => prev + 1); // No closure over stale count
```

---

## Debug 2 — N+1 With Raw SQL Fallback (10 minutes)

**Same as Interview 3 Round 4, Debug.** Fix the N+1 with eager loading, THEN rewrite with raw SQL JOIN. Under time pressure this round — you have 10 minutes, not 15.

**After fixing:** "What's the EXPLAIN plan difference between the ORM version and the raw SQL version? What would you look for in EXPLAIN output to confirm your JOIN is using an index?"

*Expected:* Look for `Seq Scan` vs `Index Scan`. An index scan on `worker_id` is O(log n). A seq scan is O(n). If the planner chooses seq scan even with an index, it might be because the table is small or the column's statistics are stale — run `ANALYZE`.
