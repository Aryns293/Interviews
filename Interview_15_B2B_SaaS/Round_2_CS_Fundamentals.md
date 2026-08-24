# Round 2 — CS Fundamentals
**Interview:** B2B SaaS / Enterprise Software
**Duration:** 45 minutes
**Theme:** Multi-tenancy, APIs, Idempotency, and Webhooks.

---

## Databases — Multi-Tenancy Architecture

**Q:** "Rolewize is a SaaS. How do you isolate data between Tenant A and Tenant B at the database layer?"
*Expected:*
1. **Shared DB, Shared Schema (The most common MVP):** Every table has a `tenant_id` column. High risk of data leakage if a developer forgets the `WHERE` clause.
2. **Shared DB, Isolated Schema:** One Postgres database, but a different `schema` (namespace) for every tenant. Better isolation, harder to run global migrations.
3. **Isolated DB:** A completely separate database instance per tenant. Maximum security (Enterprise tier), maximum cost/maintenance.

---

## System Design — Webhook Delivery Reliability

**Q:** "You built the webhook system for Rolewize. Our SaaS sends 10 million webhooks a day. If our customer's server is down, we must retry. How do you implement exponential backoff without overloading our own queues?"

*Expected:*
- Do not keep failing jobs in the "active" queue looping endlessly.
- Use BullMQ / Redis ZSETs (which you did in QueueFlow).
- If a delivery fails, calculate `next_retry = now + (2 ^ attempt_count) * base_delay`.
- Put the job back into the ZSET with a score of `next_retry`.
- A separate poller picks it up only when the time has come, keeping the active worker pool free for fresh webhooks.

---

## APIs — Pagination Strategies

**Q:** "A customer wants to export their 5 million transaction logs via our API. `?page=500000` is timing out. Why, and what are the alternatives?"
*Expected:*
- Offset pagination is O(N).
- **Alternative 1: Cursor-based pagination.** `?starting_after=txn_9876`. Fast O(log N) DB lookups using the index.
- **Alternative 2: Async Export.** Don't do this via a synchronous API. Provide an endpoint `POST /exports`. Return a `202 Accepted` with a Job ID. Generate a CSV in the background (QueueFlow), upload to S3, and email them a presigned URL.


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### OS - File Systems
**File Descriptors:** What happens internally when you open a file in an application?

### CN - Web Arch
**HTTP Versions:** What are the key architectural differences between HTTP/1.1 (Keep-Alive), HTTP/2 (Multiplexing, Binary Framing), and HTTP/3 (QUIC/UDP)?

### SQL - Query 5
**Finding Orphan Records (Anti-Joins):** Find all customers who have *never* placed an order using `LEFT JOIN ... IS NULL` or `NOT EXISTS`.



---

## Master Question Bank — Assigned Slice (Round 15)
*These questions are your assigned coverage from the 275-question Master Bank. Every question appears exactly once across all 20 interviews.*

### Operating Systems (OS)
- Priority inversion — how is it solved (priority inheritance)?
- Preemptive vs non-preemptive scheduling

### Database Management Systems (DBMS)
- Logical schema vs physical schema
- NoSQL types (document, key-value, column-family, graph) — when to choose over SQL

### SQL — Practical Query Problems
- How do indexes affect JOIN performance?

### Computer Networks (CN)
- Man-in-the-middle attack — how TLS prevents it
- VPN — conceptual mechanism

### Object-Oriented Programming (OOP)
- Object slicing in C++

### Linux
- Environment variables — temporary vs permanent

### Security
- Fork bomb — how do process limits mitigate it?

### Git
- origin vs upstream in a fork workflow

### Language Internals — Java
- synchronized methods vs synchronized blocks

### Language Internals — C++
- Move semantics (std::move, rvalue references)

