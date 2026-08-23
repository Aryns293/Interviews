# Round 2 — CS Fundamentals
**Interview:** Site Reliability Engineering (SRE) / DevOps-Focused Company
**Duration:** 45 minutes

---

## Operating Systems
- Load average vs CPU utilization — which would you actually page on-call for?
- Diagnosing thrashing from metrics alone, no shell access.

---

## DBMS
- Connection pool exhaustion — symptoms, root causes, diagnosing live from a dashboard vs needing to SSH in.

---

## Computer Networks
- What makes a good health-check endpoint (tie to your Docker/Judge0 fallback) vs a fake "always 200 OK" one.
- Timeout tuning — too aggressive vs too lax, failure mode of each.

---

## Linux
- `kill -9` vs `SIGTERM` during an incident — when is a hard kill actually the right call despite data-loss risk?

---

## Observability
- Logs vs metrics vs traces — which would you reach for first for "why is p99 latency spiking right now"?


---

## Exhaustive CS Fundamentals (Theoretical & SQL)
*These are additional deep-dive topics specifically assigned to this interview loop to ensure 100% breadth coverage across all 20 interviews.*

### DBMS - Concurrency
**Isolation:** Explain the difference between Pessimistic Locking and Optimistic Locking (MVCC - Multi-Version Concurrency Control).

### CN - Security
**Certificates:** What is a Certificate Authority (CA), and how does it prevent Man-in-the-Middle (MITM) attacks?

