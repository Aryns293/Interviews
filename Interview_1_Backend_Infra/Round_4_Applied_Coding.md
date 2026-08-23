# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** Backend / Infra-Heavy Product Company
**Duration:** 60–90 minutes
**Format:** Live coding. I'll give you a real-world problem and watch you build it. This isn't LeetCode — there's no single "correct" answer. I'm watching how you architect, how you handle edge cases, and whether your code is production-aware.

---

## Build 1 — Idempotent Webhook Receiver Middleware (30 minutes)

### The Problem
Build an Express.js middleware that makes any POST endpoint idempotent using Redis.

**Requirements:**
1. Extract a unique `Idempotency-Key` from the request header
2. On first request: process normally, store the response in Redis with a TTL of 24 hours
3. On replay (same key within TTL): return the cached response immediately without touching the database or executing any business logic
4. If the key is missing: return `400 Bad Request`

```js
// Starter scaffold — build from here
const express = require('express');
const redis = require('redis');

const app = express();
const client = redis.createClient();

// Your middleware goes here
const idempotencyMiddleware = async (req, res, next) => {
  // TODO
};

app.post('/webhook', idempotencyMiddleware, async (req, res) => {
  // Simulate processing
  await processWebhook(req.body);
  res.status(200).json({ received: true });
});
```

**What I'm watching for:**
- Do you intercept `res.json()` to capture the response before caching it?
- Do you handle the race condition where two identical requests arrive simultaneously before either is cached? (Redis `SET NX` pattern)
- Do you handle the case where Redis is down gracefully — fail open or fail closed, and can you justify your choice?
- Is the cache key namespaced (e.g., `idempotency:<key>`) or are you using the raw header value directly?

---

## Debug 1 — N+1 Query Problem (15 minutes)

### The Problem
This endpoint lists all jobs and fetches each job's worker details. It's causing high DB load. Find and fix it.

```js
// Broken implementation
app.get('/jobs', async (req, res) => {
  const jobs = await db.query('SELECT * FROM jobs');

  const result = await Promise.all(
    jobs.rows.map(async (job) => {
      const worker = await db.query(
        'SELECT * FROM workers WHERE id = $1',
        [job.worker_id]
      );
      return { ...job, worker: worker.rows[0] };
    })
  );

  res.json(result);
});
```

**Fix it with a JOIN. Then answer:**
- How many DB queries does the broken version make for 100 jobs?
- What's the query count after your fix?
- When would you still use the N+1 approach? (Hint: there are valid cases.)

---

## Discussion — Connection Pool Under Load (10 minutes)

**Q:** Your Postgres connection pool has a default size of 10. Your QueueFlow system starts receiving 1,000 concurrent job-completion writes. Walk me through exactly what happens.

*What I want to hear:*
- Requests queue up waiting for a free connection
- If the queue grows beyond the pool's `max` + `idleTimeoutMillis` threshold, requests start being rejected
- Solution: increase pool size (but Postgres has a hard `max_connections` limit, default 100)
- Better solution: PgBouncer as a connection pooler in front of Postgres, so your app sees unlimited connections but Postgres only sees a fixed pool

**Follow-up:** What's the difference between connection pooling at the application layer (pg-pool) vs at the infrastructure layer (PgBouncer)? When does it matter?

---

## Build 2 — Exponential Backoff Utility (15 minutes)

### The Problem
Build a reusable `retry(fn, options)` function with exponential backoff.

**Requirements:**
- `fn`: async function to retry
- `options.maxAttempts`: number (default 3)
- `options.baseDelayMs`: number (default 2000)
- Delay sequence: 2s → 4s → 8s (base * 2^attempt)
- Add jitter: randomize the delay by ±20% so retrying workers don't all hammer at the same time (thundering herd problem)
- Return the result on success, throw after all attempts exhausted

```js
// Implement this
async function retry(fn, options = {}) {
  // TODO
}

// Example usage
const result = await retry(() => callExternalAPI(), {
  maxAttempts: 3,
  baseDelayMs: 2000
});
```

**Follow-up:** In your actual QueueFlow system, you store retry state in a Redis ZSET rather than in memory. Why does in-memory retry state fail across worker restarts? Walk me through the exact ZSET data model you'd use to track: job ID, attempt count, and next retry timestamp.
