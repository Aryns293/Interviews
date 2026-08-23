# Round 2 — CS Fundamentals
**Interview:** Remote-First / Fully Distributed Team Company
**Duration:** 45 minutes
**Theme:** Everything is viewed through the lens of timezone independence and asynchronous communication.

---

## Computer Networks — Async Comm Patterns

**Q1:** "In a fully async team building an app, does a stateful long-lived connection like Socket.IO make sense? Does it fight against the timezone-independent model?"
*Expected:* WebSockets require both parties (client and server) to be alive and connected concurrently. If the app is a real-time chat, yes. If the app is a task tracker for an async team, long-polling or even simple REST with aggressive client-side caching (SWR) is often better because stateful connections scale poorly and don't fit the "offline-first" or "async-first" mental model.

**Q2:** "REST Idempotency — retries over unreliable links."
*Expected:* Mention idempotency keys. (Covered heavily in other loops, but crucial here because distributed teams often deal with distributed infrastructure where network partitions are common).

---

## Git — The Remote Worker's Lifeline

**Q1:** "Merge vs Rebase on a distributed team with contributors in 6 timezones. What's your policy?"
*Expected:* 
- Rebase local feature branches against `main` before pushing (keeps history linear).
- Squash and Merge PRs (keeps `main` clean, one commit per feature).
- NEVER rebase a shared remote branch (causes chaos for teammates in other timezones who pulled it while you were sleeping).

**Q2:** "Conflict resolution when nobody's online to pair on it."
*Expected:* Git reflog. When a merge goes horribly wrong at 2am, `git reflog` lets you undo the botched merge and reset HEAD to the exact commit before you started.

---

## OOP / Docs-as-Code

**Q:** "Why do interface/abstract-class contracts matter more when the person implementing them won't talk to the person who wrote them for 12 hours?"
*Expected:* Interfaces act as the ultimate truth. If the frontend and backend agree on an API schema (OpenAPI/Swagger) or a TypeScript interface, they can work independently in different timezones. If the contract is just a Slack message, someone is going to get blocked for 12 hours waiting for clarification.

---

## Security — Stateless Auth

**Q:** "JWT — why does stateless auth matter for a distributed, possibly multi-region backend?"
*Expected:* With session cookies (stateful), the session ID must be looked up in a central database or Redis. If you have servers in US-East and EU-West, checking a central Redis adds latency. JWTs contain the user payload and are cryptographically signed — any region can verify the signature locally without a database lookup.
