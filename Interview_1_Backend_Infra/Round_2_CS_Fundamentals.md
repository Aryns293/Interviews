# Round 2 — CS Fundamentals
**Interview:** Backend / Infra-Heavy Product Company
**Duration:** 60–75 minutes
**Format:** Verbal + whiteboard. I'll open with one easy-medium warm-up coding problem, then pivot to fundamentals. I will go 3–4 follow-ups deep on whatever I pick. I am not going to cover every topic below — I'll pick the ones that are most relevant to your resume and then keep pushing.

---

## Warm-Up Coding (15 minutes)
**Write a function that groups a list of jobs by their `status` field and returns a count per status.**

```js
// Input
[
  { id: 1, status: "pending" },
  { id: 2, status: "done" },
  { id: 3, status: "pending" },
  { id: 4, status: "failed" }
]
// Expected output
{ pending: 2, done: 1, failed: 1 }
```
Clean one-pass solution. I'm checking if you reach for `reduce` naturally or if you write a verbose for-loop. Either is fine. Explain your choice.

---

## OS — Deadlock & Banker's Algorithm

**Q1:** Name the 4 necessary conditions for deadlock.

**Q2 (follow-up):** Give me a live trace — 4 processes, 3 resource types. I'll give you an allocation matrix and a max matrix. You tell me if the system is in a safe state, and give me the safe sequence.

```
Processes: P0, P1, P2, P3
Resources: A=10, B=5, C=7

Allocation:    Max:
P0: 0 1 0     P0: 7 5 3
P1: 2 0 0     P1: 3 2 2
P2: 3 0 2     P2: 9 0 2
P3: 2 1 1     P3: 2 2 2

Available: A=3, B=3, C=2
```

**Q3 (tie to your project):** Your BullMQ retry mechanism attempts a job 3 times before dead-lettering it. Could that retry logic itself cause a deadlock? Walk me through why or why not. *(Hint: deadlock requires hold-and-wait. Does BullMQ hold a resource while waiting for another?)*

---

## DBMS — Normalization & Indexing

**Q1:** I give you this schema:
```
job_queue(queue_id, queue_name, job_id, job_name, worker_id, worker_name, worker_email, created_at)
```
Normalize it to 3NF. Walk me out loud through each step — 1NF → 2NF → 3NF.

**Q2 (follow-up):** In your normalized schema, there's a `status` column on the `jobs` table. Status has 4 possible values: `pending`, `running`, `done`, `failed`. Should you index it?

*What I want to hear:* Low cardinality columns are typically bad index candidates because the query planner may choose a full table scan anyway. However, if 95% of rows are `done` and you're always querying for `pending` jobs, a partial index on `status = 'pending'` is highly effective.

---

## SQL — Window Functions

**Q1:** Write a SQL query that returns the 3rd highest-priority job per queue. Use `DENSE_RANK()`.

```sql
-- Table: jobs(id, queue_id, priority, name)
-- Return: queue_id, job name, job priority for rank = 3 within each queue
```

**Q2 (follow-up):** What's the difference between `RANK()`, `DENSE_RANK()`, and `ROW_NUMBER()`? Give me an example where they produce different results.

**Q3 (follow-up):** I want the count of jobs per worker, but only workers who have processed more than 5 jobs. Write the query. Then tell me why `WHERE` wouldn't work here.

---

## Redis Internals

**Q1:** What's the difference between RDB snapshots and AOF persistence in Redis?
*Answer / Reference:* [ChatGPT Explanation of RDB vs AOF & Zero Data Loss Tradeoffs](https://chatgpt.com/share/6a8de23f-13f8-83ee-ac24-14e3726b01a0)

**Q2 (follow-up):** For your QueueFlow system that claims zero data loss, which persistence mode would you configure, and what are the exact tradeoffs you're accepting?

*The Core Answer:* To legitimately guarantee zero data loss in the event of a hard crash (like a server power failure), you must configure AOF (Append Only File) with the setting appendfsync always.

*(RDB snapshots are completely off the table for zero data loss, because if Redis crashes between the 5-minute snapshot intervals, you lose up to 5 minutes of jobs).*

*The Exact Tradeoffs You Are Accepting:* If you tell an interviewer you configured appendfsync always, you must immediately prove you understand the severe consequences of that decision:

- **Massive Performance Degradation:** Redis is famous for handling 100,000+ operations per second because it's an in-memory database. By setting appendfsync always, you force Redis to perform a synchronous disk write on every single enqueue/dequeue operation before acknowledging success to the client. You are essentially turning Redis into a slow, disk-backed database. Your throughput will drop from 100k+ ops/sec to whatever the IOPS limit of your SSD is (often just a few thousand ops/sec).
- **Disk I/O Bottleneck & Main Thread Blocking:** Because Redis is single-threaded for command execution, if the disk is slow to respond to the fsync, the main thread blocks. Every other client waiting to push or pull a job is frozen until that disk write completes.
- **Bloated File Sizes:** AOF logs every single command (e.g., LPUSH, then RPOP). The file grows rapidly, requiring Redis to frequently trigger an "AOF Rewrite" in the background to compress the log, which consumes additional CPU and memory.

*How to seal the deal with the interviewer:* "In a true enterprise production environment, we rarely use appendfsync always because the performance hit defeats the purpose of using Redis. Instead, we use AOF with appendfsync everysec. This gives us 99% of Redis's max performance, with a clearly defined worst-case scenario: we risk exactly 1 second of data loss if the server loses power. If the business absolutely requires mathematical zero data loss, we shouldn't use Redis for the queue; we should use a durable log like Kafka or a Postgres-backed queue like Graphile."

**Q3 (follow-up):** If an interviewer says: "You claimed 0% data loss, but Redis AOF everysec loses 1 second of data if the server loses power. Is your project statement of 0% data loss wrong?"

*The Defense (Say this to the interviewer):*
"When I say 0% data loss, I am specifically referring to Worker Fault Tolerance and Atomic Execution, not hardware-level datastore power failures.

In a naive queue, if a worker pops a job and then the worker crashes, that job is lost forever. I engineered QueueFlow using BullMQ specifically to prevent this.

Under the hood, when a worker picks up a job, BullMQ doesn't just POP it and delete it. It uses an atomic Lua script (historically BRPOPLPUSH) to atomically move the job from the wait list into an active list.

If my worker server completely crashes mid-execution, the job is not lost—it is safely sitting in the active list in Redis. QueueFlow runs a background 'Stalled Job Checker'. When it detects that a job has been in the active list longer than the lock timeout without a heartbeat, it automatically moves the job back to the wait list to be retried by another worker.

That is how I guarantee 0% data loss during execution, network partitions, or worker OOM crashes.

As for the Redis server itself, you are absolutely right: to maintain high throughput, we use AOF everysec. We accept a 1-second risk window if the Redis bare-metal server loses power, because setting fsync always would destroy our throughput. So the 0% data loss guarantee applies strictly to the distributed worker architecture."

*Why this answer gets you hired instantly:*
- You didn't back down from your resume claim.
- You proved you know exactly how BullMQ prevents job loss under the hood (Atomic moves + Stalled Job Checker).
- You conceded the hardware-level truth about Redis (AOF everysec), proving you understand real-world database tradeoffs.

**Q4 (follow-up):** Your QueueFlow uses `BRPOP` to dequeue jobs. What is `BRPOP` actually doing at the socket/OS level compared to a polling loop? What would polling look like, and why is it worse?

*The Answer:*

**1. What Polling Looks Like & Why It's Worse:**
"If we didn't use a blocking command, we would have to use standard `RPOP` in a `while(true)` loop. The worker would constantly ask Redis: *'Got a job? No. Got a job? No.'* 
This is terrible for three reasons:
1. **CPU Burn:** It pegs the worker's CPU at 100% just running a useless loop.
2. **Network Saturation:** It floods the network with thousands of useless TCP request/response packets every second.
3. **Redis Thread Starvation:** Because Redis is single-threaded, forcing it to process 10,000 useless `RPOP` commands a second steals valuable CPU time away from actual producers trying to push jobs."

**2. What `BRPOP` actually does at the OS/Socket Level:**
"`BRPOP` (Blocking Right Pop) completely eliminates this by using an event-driven model. 

When a worker sends a `BRPOP` command to an empty queue, Redis does **not** send an immediate 'empty' response. Instead, Redis parks that client's connection in an internal dictionary (mapping the queue key to a list of blocked clients). 

At the OS level, the TCP socket remains open, but the connection goes idle. The worker thread goes to sleep, consuming **zero CPU**. 

The moment a producer runs an `LPUSH` to that queue, Redis intercepts it, looks up its dictionary of blocked clients, and instantly writes the new job data directly to the sleeping worker's TCP socket. The OS wakes the worker thread up, and it processes the job.

*The Conclusion:*
By using `BRPOP` (or modern equivalents like `LMPOP`), we convert a wasteful 'pull' architecture into an instantaneous 'push' architecture. It gives us sub-millisecond job pickup times with zero CPU or network waste."

---

## Security — HMAC Webhook Verification

**Q1:** Walk me through exactly how you implemented HMAC-SHA256 signature verification in your Rolewize webhook endpoint. Don't just say "I used a library" — walk me through the algorithm steps.

*Expected walk-through:*
1. External service sends `X-Signature: sha256=<hash>` header
2. You extract the raw body (before JSON parsing — critical!)
3. You compute `HMAC-SHA256(secret_key, raw_body)`
4. You compare your computed hash against the header value

**Q2 (follow-up):** Why must you compare hashes using a constant-time comparison function instead of `===`? What attack does timing-safe comparison prevent?

*Expected answer:* Timing attacks — a character-by-character comparison returns faster if the first characters mismatch, leaking information about how close the attacker's guess is. `crypto.timingSafeEqual()` ensures the comparison always takes the same time.


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### OS - Process Management
**Process vs Thread:** What is the difference? What resources do they share (Heap, Code, Data) and what is strictly isolated (Stack, Registers)?

### DBMS - Isolation
**Read Phenomena:** Explain Dirty Reads, Non-Repeatable Reads, and Phantom Reads. Which isolation level prevents which phenomenon?

### OOP - 4 Pillars
**Inheritance:** The 'Is-A' relationship. Why is Composition ('Has-A') generally favored over deep inheritance trees in modern software design?



---

## Master Question Bank — Assigned Slice (Round 1)
*These questions are your assigned coverage from the 275-question Master Bank. Every question appears exactly once across all 20 interviews.*

### Operating Systems (OS)
- Process vs thread — differences
- What is context switching, and what does it cost?

### Database Management Systems (DBMS)
- ACID properties — real example of each one breaking
- Normalization — 1NF, 2NF, 3NF, BCNF with examples

### SQL — Practical Query Problems
- Nth highest salary
- RANK() vs DENSE_RANK() vs ROW_NUMBER()

### Computer Networks (CN)
- What happens when you type google.com? (DNS → TCP handshake → TLS → HTTP)
- TCP 3-way handshake

### Object-Oriented Programming (OOP)
- 4 pillars of OOP with examples
- Abstract class vs interface — when to use each

### Linux
- chmod octal notation (e.g., 755, 644)
- Hard link vs soft (symbolic) link

### Security
- SQL injection — how prepared statements prevent it
- XSS — stored vs reflected vs DOM-based

### Git
- merge vs rebase — when to use each

### Language Internals — Java
- HashMap internals — hashing, buckets, collision handling

### Language Internals — C++
- Stack vs heap allocation

