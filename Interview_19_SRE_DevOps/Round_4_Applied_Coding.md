# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** Site Reliability Engineering (SRE) / DevOps-Focused Company
**Duration:** 60 minutes

---

## Build 1 — Health & Readiness Probes
**Live build:** `/health` and `/ready` endpoints with a real distinction between them (ready = can serve traffic now, health = process alive but maybe not ready).

---

## Build 2 — Circuit Breaker
**Live build:** A circuit breaker wrapping a flaky downstream call (like Judge0 fallback) — open after N consecutive failures, half-open retry after cooldown.

---

## Debug 1 — Cascading Failure Root Cause
**Live debug:** Given a dependency graph and a change that caused a cascading failure, identify the likely root-cause service from timing data.

---

## Discussion — Error Budgets
**Discussion:** What SLO would you set for QueueFlow's job-processing latency, and how would you calculate an error budget from it?
