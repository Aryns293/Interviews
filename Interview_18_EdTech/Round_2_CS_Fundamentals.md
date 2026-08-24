# Round 2 — CS Fundamentals
**Interview:** EdTech / Learning Platform Company
**Duration:** 45 minutes

---

## Computer Networks
- Your CodeSync AI Socket.IO model applied to a live virtual classroom — one mentor broadcasting to many students vs. many-to-many editing. 
- Does your current architecture handle "one mentor, 300 watching students" well?

---

## Operating Systems
- Process/thread basics applied to running many students' code submissions concurrently for auto-grading.
- Isolation matters here just like your Docker sandbox.

---

## DBMS
- Schema for student progress across courses/modules/lessons — normalize it, then say what you'd denormalize back for a fast progress-dashboard read.

---

## Security
- Your Docker sandbox model applied directly to auto-grading untrusted student code.
- Exact threat model for a fork bomb or a student trying to read another's submission on the same host.

---

## Object-Oriented Programming (OOP)
- Course → Module → Lesson → Assignment as a class hierarchy.
- Composition or inheritance, and where do quiz/coding-assignment/video-lesson fit as assignment types?


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### DBMS - ACID
**Consistency & Durability:** What does Consistency mean in the context of ACID versus the CAP Theorem? How does the DB guarantee a transaction survives a power failure immediately after the commit succeeds?

### CN - Security
**TLS/SSL Handshake:** Explain Symmetric vs Asymmetric encryption. How are both used during a TLS handshake?



---

## Master Question Bank — Assigned Slice (Round 18)
*These questions are your assigned coverage from the 275-question Master Bank. Every question appears exactly once across all 20 interviews.*

### Operating Systems (OS)
- Why does Belady's Anomaly specifically show up under FIFO?
- What is swapping?

### Database Management Systems (DBMS)
- Deadlock timeout — how is it configured?
- Database partitioning: range, list, hash

### SQL — Practical Query Problems
- Most recent record per group (e.g., latest login per user)

### Computer Networks (CN)
- HTTP caching — Cache-Control, ETag, Last-Modified

### Object-Oriented Programming (OOP)
- Singleton pattern vs using it as a global-state escape hatch

### Linux
- SIGTERM vs SIGKILL vs SIGINT

### Security
- MIME-type validation — why trusting client-declared MIME is risky

### Git
- Tag vs branch

