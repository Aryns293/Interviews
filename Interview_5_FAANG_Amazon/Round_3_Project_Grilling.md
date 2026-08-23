# Round 3 — Past Experience & Project Grilling
**Interview:** FAANG-Style / Amazon Bar Raiser Loop
**Duration:** 45–60 minutes
**Style:** All 3 projects + the internship, in one sitting. I will defend every absolute claim on your resume. No softballs.

> **Interviewer's mindset:** Every bullet on your resume is a contract. I am here to audit the contract. If you've rounded up, I'll find it. If you've been precise, we'll have a great conversation. If you've been imprecise, I'll note it in my hire/no-hire scorecard.

---

## Claim 1 — "Zero Data Loss" (QueueFlow)

> "Is that literally true? Walk me through the exact window between a job being LPUSH'd to Redis and being durably persisted to Postgres. If the server process crashes in that window, what happens to the job?"

**The precise answer:**
- After `LPUSH`, the job is in Redis
- If Redis is configured with `appendfsync everysec` (default AOF), there's up to 1 second of potential data loss
- The Postgres write happens AFTER the worker picks up the job via `BRPOP` and processes it
- The crash window: LPUSH → BRPOP → crash before Postgres write = job is neither in Redis (BRPOP consumed it) nor in Postgres = **the job is lost**
- BullMQ uses a "lock" pattern (job is moved to an `active` set, not deleted on BRPOP) specifically to handle this case — if the lock expires without completion, the job is re-queued

**I will ask:** "Did you implement this lock-and-redeliver pattern, or did you rely on BullMQ's default behavior?"

---

## Claim 2 — "100% Fault Tolerance Against Worker Node Failures" (QueueFlow)

> "100%? What if the failure mode is a network partition — the worker process is alive and thinks it's processing, but it can't reach Redis or Postgres? Does your system distinguish 'dead' from 'partitioned'?"

**The precise answer:**
- A network partition is NOT the same as a worker crash
- A crashed worker: the Redis lock TTL expires, job is re-queued automatically
- A partitioned worker: the worker is still alive, still holds the lock (if it can periodically renew it), but can't complete the job
- If the worker keeps renewing the lock from its side, the job is "stuck" — it won't be re-queued even though it's never completing
- "100% fault tolerance" should be "fault-tolerant against worker crashes" — network partitions are a different failure mode

---

## Claim 3 — "3-Attempt Retry" (Rolewize BullMQ)

> "What happens on attempt 4? Is it a silent drop, a dead-letter queue write, or an alert? If you don't remember exactly, tell me that — don't guess."

**Expected:** Be precise. BullMQ moves jobs to a `failed` set after `maxAttempts` is exceeded. Whether you added alerting, a dead-letter queue consumer, or just let them sit in the failed set depends on your implementation. If you don't remember the exact behavior, saying "I'd need to check the BullMQ docs on `removeOnFail` configuration" is more honest than guessing.

---

## Claim 4 — "Eliminating Duplicate LLM API Calls" (Rolewize)

> "Eliminating, or reducing? Give me the exact mechanism through which a duplicate could still get through."

**The precise answer:**
- First call for any content always hits the LLM — it's not in cache yet
- Race condition: two identical resumes uploaded within milliseconds, both cache-miss at the same time, both trigger LLM calls before either writes to cache
- Fix: use `SET NX` (set if not exists) as a distributed lock before the LLM call, so only one call goes through and the other waits for the cached result
- TTL expiry: after 24 hours, the cache entry is gone. The same resume uploaded on day 2 makes a new LLM call.
- "Reduces" is accurate. "Eliminates" is aspirational without a distributed lock.

---

## Claim 5 — "Fully Compatible With Real Git" (gitlight)

> "Compatible how — did you systematically test your generated objects against `git cat-file` output on the same content? Or is that an aspirational claim?"

**What I accept:**
- "I tested blob and commit object generation — the SHA-1 hashes matched `git hash-object` for the same content" → credible
- "I didn't do systematic cross-testing, but the SHA-1 algorithm is deterministic, so if my header format matches Git's spec, the hashes will match" → honest and technically sound
- "It's fully compatible" with no testing methodology → I flag it

---

## What Gets You Through This Round

| Claim | Pass | Fail |
|---|---|---|
| Zero data loss | Acknowledges the crash window, knows BullMQ's lock-redeliver mechanism | "Redis handles it" |
| 100% fault tolerance | Distinguishes crash vs network partition | "Workers auto-restart" |
| 3-attempt retry | Knows exactly what happens on attempt 4, or explicitly says "I'd need to verify" | Guesses |
| Eliminating duplicates | Identifies the race condition and TTL expiry as mechanisms that could let duplicates through | "The cache handles it" |
| Git compatibility | Has a testing methodology | Aspirational claim with no verification |
