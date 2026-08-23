# Round 3 — Past Experience & Project Grilling
**Interview:** Site Reliability Engineering (SRE) / DevOps-Focused Company
**Duration:** 45 minutes

---

## Q1
"QueueFlow claims '100% fault tolerance against worker node failures' — if you got paged at 3am because a worker died mid-job, walk through exactly what happens to that in-flight job. Is 100% literally defensible, or is there a real loss window?"

## Q2
"Your BullMQ 3-attempt retry — what alert would you configure around attempt exhaustion, and what's your actual runbook when it fires?"

## Q3
"CodeSync AI's Judge0 fallback — is the Docker health check itself monitored? If Docker silently degrades (not down, just slow), does anything page you, or does it fail silently until a user complains?"

## Q4
"No CI on any of your projects. How would you retroactively add basic health/readiness checks and a smoke-test pipeline to QueueFlow without a full rewrite — what's the minimum first step?"
