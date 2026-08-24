# Round 2 — CS Fundamentals
**Interview:** Fintech / Payments Company
**Duration:** 60–75 minutes
**Theme:** This round is the heaviest DBMS and security round across all 10 interviews. Every topic maps to a real risk in a payments system.

---

## DBMS — Isolation Levels & Double-Spend

**Q1 (concrete double-spend scenario):**
> "User A checks their balance: ₹10,000. User A initiates a ₹9,000 transfer. Simultaneously, User B (a different session, same account — family account) initiates a ₹8,000 transfer. Both reads see ₹10,000. Both transfers proceed. Final balance: ₹10,000 - ₹9,000 - ₹8,000 = -₹7,000. What anomaly is this, which isolation level caused it, and which isolation level prevents it?"

*Expected:*
- **Anomaly:** Lost Update (or dirty read in some implementations)
- **Caused by:** `READ UNCOMMITTED` or even `READ COMMITTED` without row-level locking
- **Prevented by:** `REPEATABLE READ` with `SELECT FOR UPDATE`, or `SERIALIZABLE`
- **Practical fix:** `BEGIN; SELECT balance FROM accounts WHERE id=? FOR UPDATE; UPDATE...`

**Q2 (2PL and deadlocks in payments):**
> "Two transactions: T1 transfers from Account A to Account B. T2 transfers from Account B to Account A. Both start at the same time. Walk me through how a deadlock forms, and how you break it."

*Expected:*
- T1: Lock A → tries to lock B (B is locked by T2)
- T2: Lock B → tries to lock A (A is locked by T1)
- Circular wait → deadlock
- Break it: always acquire locks in a canonical order (e.g., ascending account ID). T1 and T2 both lock the lower-ID account first. No circular wait possible.

**Q3 (CAP applied to a ledger):**
> "Your payment ledger must be strongly consistent — no stale reads. Your session cache (user login state, JWT blacklist) can tolerate slight staleness. How do you split these across CP vs AP stores?"

*Expected:* Ledger → Postgres (CP: consistency over availability). Session cache → Redis (AP: available even under partition, eventual consistency acceptable for 30-second JWT blacklist lag). Explain the tradeoff explicitly.

---

## Security — HMAC, Timing Attacks, and Encryption at Rest

**Q1:**
> "A payment processor sends you webhooks. The request arrives over valid HTTPS. Why isn't TLS alone sufficient to guarantee the webhook is authentic?"

*Expected:* TLS guarantees transport security — the message wasn't tampered with in transit. But it doesn't verify that the *sender* is who they claim to be. Anyone with a valid TLS certificate can send you an HTTPS request. HMAC-SHA256 uses a shared secret that only you and the legitimate processor know — it proves the message was constructed by someone with that secret.

**Q2:**
> "Walk through the HMAC-SHA256 verification flow for a payment webhook. Now: the signature comparison uses JavaScript's `===` instead of `crypto.timingSafeEqual`. Walk me through the exact attack."

*Expected:* Timing oracle attack — character-by-character comparison leaks timing information. With enough samples (~10,000 requests), an attacker can reconstruct the expected HMAC one character at a time without knowing the secret key. `crypto.timingSafeEqual()` ensures constant-time comparison regardless of where the mismatch occurs.

**Q3 (direct to your Rolewize experience):**
> "You stored resume files (PII) on S3. Was the bucket encrypted at rest? Did you control the key, or was it AWS-managed default encryption?"

*Expected:* Honest answer. AWS S3 enables server-side encryption by default (SSE-S3) on all new buckets since 2023. If you used S3 without explicitly configuring it, you likely got SSE-S3 (AWS-managed keys) by default. Customer-managed keys (SSE-KMS) give you key rotation control and audit logs — required for PCI-DSS compliance. Know the difference.

---

## OS — Deadlock Proof on Your Own System

**Q:**
> "Could your dual-layer Redis/Postgres idempotency check in Rolewize ever deadlock? Prove it either way using the 4 necessary conditions for deadlock."

*Expected proof:*
- **Mutual exclusion:** Redis SET NX holds a logical lock. ✓ (exists)
- **Hold-and-wait:** After acquiring the Redis lock, does the process wait for another resource while holding it? Only if it then tries to acquire a Postgres row lock. In your implementation, does the Redis lock remain held during the Postgres write? If yes — potential hold-and-wait.
- **No preemption:** Redis TTL IS a form of preemption — the lock releases automatically after TTL. This breaks deadlock condition 3.
- **Circular wait:** Would require Process A holding Redis waiting for Postgres while Process B holds Postgres waiting for Redis. Unlikely in practice, but theoretically possible if transactions interleave wrong.
- **Conclusion:** TTL-based expiry on the Redis lock breaks condition 3, making deadlock practically impossible. But you should know the analysis, not just assert "Redis handles it."

---

## CN — TLS vs HMAC (Webhook Authenticity Layer)

**Q:** Walk me through the TLS handshake. Then explain: if TLS already encrypts and integrity-checks the payload, what additional security property does HMAC provide that TLS doesn't?

*Expected:* TLS provides: encryption (confidentiality), MAC (integrity), and certificate-based server authentication. TLS does NOT verify the *application-level identity* of the sender. A malicious actor can set up a legitimate TLS server and send you a perfectly valid HTTPS webhook. HMAC adds **application-level authentication** — only a party with the pre-shared secret can produce a valid HMAC signature.


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### OS - CPU Scheduling
**CPU Scheduling:** Explain Round Robin, Shortest Job First (SJF), and Multilevel Feedback Queues.

### DBMS - Indexing
**Clustered vs Non-Clustered Indexes:** What is the difference? How many clustered indexes can a table have?

### OOP - Patterns
**Singleton:** How do you implement a strictly thread-safe Singleton? Why is it often considered an anti-pattern?



---

## Master Question Bank — Assigned Slice (Round 6)
*These questions are your assigned coverage from the 275-question Master Bank. Every question appears exactly once across all 20 interviews.*

### Operating Systems (OS)
- Thrashing — what causes it, how do you prevent it?
- Belady's Anomaly

### Database Management Systems (DBMS)
- B-Tree vs B+ Tree — why do DBs prefer B+ Trees for indexing?
- Indexing — clustered vs non-clustered

### SQL — Practical Query Problems
- Second highest salary without LIMIT/OFFSET
- GROUP BY with multiple aggregates + HAVING

### Computer Networks (CN)
- Is POST idempotent? PUT? DELETE? GET? Why?
- Cookies vs sessions vs JWTs

### Object-Oriented Programming (OOP)
- Liskov Substitution Principle — classic violation
- Interface Segregation Principle

### Linux
- PID vs PPID

### Security
- JWT — header, payload, signature

### Git
- reset --soft vs --mixed vs --hard

### Language Internals — Java
- Checked vs unchecked exceptions

### Language Internals — C++
- Shallow copy vs deep copy (copy constructor)

