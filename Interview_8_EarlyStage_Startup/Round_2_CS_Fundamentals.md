# Round 2 — CS Fundamentals
**Interview:** Early-Stage Seed Startup
**Duration:** 45 minutes
**Style:** Faster pace, lighter depth than enterprise loops. Startups care more about "can you reason correctly under pressure" than "can you recite the textbook."

---

## DBMS — ACID, Fast and Practical

**Q1:** ACID — give me one concrete example of each property breaking, in the context of your own projects.

**Q2 (pragmatic twist):**
> "When would you deliberately NOT use a database transaction for speed — and what specific risk are you accepting when you make that choice?"

*Expected:*
- High-write throughput logs, analytics events, or audit trails where eventual consistency is acceptable
- Risk: if the process crashes between two writes, the data is partially written with no rollback
- In fintech or user-facing state changes: never skip the transaction. In append-only analytics: often fine.

**Q3:**
> "Is your `POST /signup` endpoint idempotent? Does it need to be for an MVP?"

*Expected:*
- `POST /signup` is NOT idempotent by default — submitting the form twice creates two accounts (or throws a uniqueness error)
- For an MVP: a uniqueness constraint on `email` in the DB is sufficient — the second signup attempt returns a 409, not a duplicate account. That's good enough.
- For a production system handling mobile clients with unreliable connections that may retry: you'd add a client-generated idempotency key.

---

## Security — Non-Negotiable Even at MVP Stage

**Q1:** Show me a SQL injection vulnerability and fix it with a prepared statement.

```js
// VULNERABLE
const result = await db.query(`SELECT * FROM users WHERE email = '${req.body.email}'`);

// SAFE
const result = await db.query('SELECT * FROM users WHERE email = $1', [req.body.email]);
```

**Q2:** "You're building an MVP signup form. You have 2 hours. Do you salt and hash passwords, or store them in plaintext for now with a 'fix later' note?"

*Expected:* Salt and hash. Always. Using bcrypt takes 3 lines of code. If this MVP leaks, plaintext passwords can be used to compromise your users' accounts on every other site they use the same password. The risk-to-effort ratio makes this non-negotiable even at day 1.

---

## Git — Real-World Workflow on a 2-Person Team

**Q:** "You and a co-founder are the only two engineers. No CI, no PR requirements, no code review process enforced. What's your actual Git workflow — merge or rebase — and why?"

*Expected:* Honest, practical answer. Options:
- **Feature branches + squash merge:** Clean main history, each feature is one commit. Good for looking back at "when did we ship X."
- **Trunk-based:** Both push directly to main, communicate via Slack when pushing. Fast, chaotic, works at 2 people.
- **Rebase before merge:** Linear history. Harder for beginners, cleaner for `git log`.

*What I'm listening for:* Do you have an actual opinion, or do you give a textbook non-answer? At a startup, "it depends" without a recommendation signals indecision.

---

## CN — REST Idempotency on Flaky Mobile Networks

**Q:**
> "A user on a 3G connection submits a form. The server processes it and returns 200, but the response is lost in transit. The app retries the request. What actually happens, and how do you prevent a duplicate?"

*Expected:* Without idempotency handling:
- POST creates a second resource → duplicate order, duplicate signup, double charge
- Fix: client generates a UUID for the request and sends it as `Idempotency-Key`. Server stores the key + response. On retry, the same response is returned without re-processing.
- For MVP: at minimum, uniqueness constraints in the DB as a last line of defense. Add a proper idempotency-key header handler when you see retry-induced duplicates in production.


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### OS - Deadlocks
**Prevention vs Avoidance:** What is the difference? Explain Banker's Algorithm and how it ensures the system remains in a 'safe state.'

### DBMS - Indexing
**Hash Indexes:** When would you use a Hash Index over a B-Tree Index?

### OOP - Patterns
**Observer:** Explain the Pub/Sub architecture. How is it implemented in event-driven systems?



---

## Master Question Bank — Assigned Slice (Round 8)
*These questions are your assigned coverage from the 275-question Master Bank. Every question appears exactly once across all 20 interviews.*

### Operating Systems (OS)
- Page fault — what happens when one occurs?
- CPU scheduling algorithms: FCFS, SJF, SRTF, Round Robin, Priority

### Database Management Systems (DBMS)
- Covering index
- OLTP vs OLAP

### SQL — Practical Query Problems
- NULL handling in comparisons, aggregates, JOINs
- EXISTS vs IN — when to prefer which

### Computer Networks (CN)
- CDN — how does it improve latency?
- NAT (Network Address Translation)

### Object-Oriented Programming (OOP)
- Constructor vs copy constructor
- Static vs dynamic binding (early vs late)

### Linux
- chown

### Security
- Refresh token — why longer-lived than an access token?

### Git
- Interactive rebase (rebase -i)

### Language Internals — Java
- G1 garbage collector

### Language Internals — C++
- Dangling pointer

