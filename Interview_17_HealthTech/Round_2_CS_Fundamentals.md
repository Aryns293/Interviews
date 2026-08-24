# Round 2 — CS Fundamentals
**Interview:** HealthTech / Compliance-Heavy Company
**Duration:** 45 minutes

---

## Security (Heavy)
- **Encryption at rest:** Was your Rolewize S3 bucket encryption default or explicitly configured, and do you know the difference in who controls the keys?
- Field-level vs whole-record encryption when some fields need to stay searchable.

---

## DBMS
- **Audit trail design:** Every read of sensitive data logged, not just writes.
- ACID with a concrete partial-write-leaves-inconsistent-record example.

---

## Computer Networks
- Why HTTPS alone isn't sufficient for regulatory compliance around PII in transit.

---

## Operating Systems
- `chmod` on a server storing sensitive intake documents — least-privilege reasoning.

---

## Access Control
- Role-based field-level access (a nurse sees vitals, a billing clerk sees insurance info) — how is this actually enforced, not just documented as policy?


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### DBMS - ACID
**Atomicity:** How is Atomicity actually implemented? (e.g., Write-Ahead Logging (WAL) or Shadow Paging).

### CN - APIs
**Idempotency:** What does it mean for an HTTP method to be idempotent? (e.g., `PUT` vs `POST`).

### SQL - Query 7
**Consecutive Occurrences:** Write a SQL query to find all numbers that appear at least three times consecutively using `LEAD()` or `LAG()` window functions.



---

## Master Question Bank — Assigned Slice (Round 17)
*These questions are your assigned coverage from the 275-question Master Bank. Every question appears exactly once across all 20 interviews.*

### Operating Systems (OS)
- Internal vs external fragmentation
- Page replacement algorithms: FIFO, LRU, Optimal — compare

### Database Management Systems (DBMS)
- How do you read an EXPLAIN / EXPLAIN ANALYZE plan?
- Full table scan — when does the optimizer choose it over an index scan?

### SQL — Practical Query Problems
- Natural join — why discouraged in production?

### Computer Networks (CN)
- Stateful vs stateless protocols

### Object-Oriented Programming (OOP)
- Interface default methods — why introduced (e.g., Java 8+)

### Linux
- df -h vs df -i

### Security
- Pre-signed URL — expiry-time tradeoff

### Git
- Shallow clone — why use one?

