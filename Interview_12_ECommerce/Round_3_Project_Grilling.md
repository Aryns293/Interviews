# Round 3 — Past Experience & Project Grilling
**Interview:** E-Commerce / High-Traffic Retail
**Duration:** 45 minutes
**Project in focus:** QueueFlow (Message queues are the backbone of e-commerce order processing).

---

## Q1 — QueueFlow: Durability vs Performance

> "In QueueFlow, you use Redis lists and ZSETs to manage job state. Redis is primarily an in-memory store. If the Redis process crashes while a job is in the 'processing' state, what happens to that job? Is it lost?"

*What I'm listening for:*
- You need to explain the "Reliable Queue" pattern (RPOPLPUSH or BRPOPLPUSH, or in modern Redis, LMOVE).
- If you just pop an item from the pending list and crash before pushing it to the complete list, the job is lost.
- A reliable queue atomically pops from the `pending` list and pushes it to a `processing` list. 
- If the worker crashes, a separate "sweeper" process checks the `processing` list for jobs that have been there too long, and moves them back to `pending`.
- Does QueueFlow implement this? (BullMQ does this via locks).

---

## Q2 — QueueFlow: The ZSET Scheduler

> "You schedule delayed jobs using Redis ZSETs (Sorted Sets). Walk me through exactly how a delayed job gets moved from the ZSET to the active List when its time has come."

*Expected:*
- The ZSET stores `job_id` with a score of `execution_timestamp`.
- A polling process runs every X milliseconds: `ZRANGEBYSCORE delayed -inf <current_timestamp>`.
- For each job returned, atomically remove it from the ZSET (`ZREM`) and push it to the active List (`LPUSH`).
- Crucial detail: It must be atomic (using Lua scripting in Redis) so that if you have multiple workers polling the ZSET, they don't both grab the same delayed job.

---

## Q3 — Webhook Deliverability at Scale (Rolewize)

> "Rolewize webhook deliveries. If a merchant's server goes down during a Black Friday sale, your webhook system will start accumulating thousands of failed webhooks that need to be retried. What happens to your system's memory/queue depth? Does it choke the delivery of webhooks to *other* healthy merchants?"

*Expected:* Head-of-line blocking.
- If all webhooks go into one giant queue, the failing merchant will trigger constant retries, filling the queue and slowing down deliveries to healthy merchants.
- Fix: Use separate queues per merchant (or per tenant), or implement exponential backoff + a dead-letter queue (DLQ) so failing messages quickly move out of the fast lane.
