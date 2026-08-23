# Round 2 — CS Fundamentals
**Interview:** FAANG-Style / Amazon Bar Raiser Loop
**Duration:** 60–75 minutes
**Style:** Rapid-fire. 3–4 follow-ups deep on each topic. No time to think out loud slowly. I move to the next topic if you stall.

---

## OS — Banker's Algorithm + Your System

**Q1:** Live numerical trace — 4 processes, 3 resource types. Is the system in a safe state? Give the safe sequence. *(Same matrix as Interview 1, Round 2 — if you know it cold, you answer in 3 minutes)*

**Q2 (your system):**
> "Does your BullMQ retry logic have any theoretical deadlock risk? Prove it either way using the 4 conditions."

*Expected proof:* BullMQ workers acquire a job lock (Redis SET NX) and then process it. There's no "hold-and-wait" — a worker doesn't hold the job lock while waiting for another resource. Even if two workers try to acquire the same job lock, one gets it and the other fails-and-moves-on. No circular wait. No deadlock.

---

## DBMS — 2PL, CAP, Consistency

**Q1:** What is 2-Phase Locking? What problem does it prevent, and what problem does it introduce?

*Expected:* 2PL = acquire all locks in a growing phase, release in a shrinking phase. Prevents: certain serializability violations. Introduces: potential for deadlock (two transactions each holding a lock the other needs).

**Q2 (your project):**
> "QueueFlow uses Redis (AP-leaning under CAP) and Postgres (CP-leaning). What consistency model does the combined system actually give you?"

*Expected:* The weakest link determines the overall guarantee. Redis with default config is eventually consistent under partition — you can write to a replica and not see it on the primary immediately. The combined system is AP for the fast path (Redis) and CP for the durable path (Postgres). If Redis and Postgres diverge during a partition (e.g., a job is BRPOP'd from Redis but not yet written to Postgres, and Redis crashes), you lose the job. The system is not strictly CP as a whole.

---

## Computer Networks — Socket.IO Internals

**Q:**
> "Socket.IO falls back through multiple transports. Which transport is closest to UDP in behavior — lowest latency, no guaranteed delivery? And why doesn't Socket.IO just use raw UDP for a collaborative code editor?"

*Expected:*
- WebSocket is closest — it's a persistent, full-duplex TCP connection, but once established, overhead per message is minimal (just a 2-byte framing header)
- Actually UDP is UDP — Socket.IO doesn't support raw UDP
- Why not UDP for a code editor? UDP doesn't guarantee delivery or order. If a character insertion event is lost or reordered, the collaborative document becomes corrupted. You need ordered, reliable delivery — which means TCP (and therefore WebSocket over TCP).
- WebRTC's data channel uses SCTP over UDP and can be configured for reliability — but that's not what Socket.IO does

---

## Language Internals — HashMap + Timing Attack

**Q1:** Walk me through HashMap internals in Java 8+. What is treeification, and why was it added?

*Expected:* HashMap uses an array of buckets. Keys with the same hash go into the same bucket as a linked list. If a bucket has ≥ 8 entries, it converts to a Red-Black Tree (treeification). Why: a malicious actor can force many collisions into one bucket, making lookups O(n). With treeification, worst case becomes O(log n).

**Q2:**
> "You implemented HMAC-SHA256 webhook verification at Rolewize. I know you used `crypto.timingSafeEqual`. Now break it — how would you attack a webhook endpoint that does HMAC-SHA256 verification with a regular `===` string comparison?"

*Expected:* Timing attack. JavaScript's `===` returns `false` as soon as it finds the first mismatching character. If I measure how long each verification attempt takes, I can determine character-by-character how close my forged signature is to the real one. Characters that match take slightly longer. Repeat 10,000 times to average out noise, and you can eventually reconstruct the expected HMAC value without knowing the secret key.

`crypto.timingSafeEqual()` always compares every byte regardless of the first mismatch — constant time, no information leaked.
