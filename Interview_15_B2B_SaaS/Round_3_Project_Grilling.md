# Round 3 — Past Experience & Project Grilling
**Interview:** B2B SaaS / Enterprise Software
**Duration:** 45 minutes
**Project in focus:** Rolewize and QueueFlow.

---

## Q1 — QueueFlow: The "Stuck Job" Problem

> "In QueueFlow, you have workers pulling from a Redis queue. If a worker pulls a job, starts executing it, and then the Node.js process is OOM killed halfway through, what happens to that job?"

*What I want to hear:*
- If you just pop the job, it's lost forever.
- SaaS requires "Exactly-Once" or "At-Least-Once" delivery guarantees.
- You must explain the concept of a `visibility timeout` (like AWS SQS) or the `processing` set pattern (BRPOPLPUSH). The job is moved to a 'processing' list and timestamped. A separate watcher process re-queues jobs that have been in the 'processing' list longer than 5 minutes.
- *Crucial follow-up:* Because the job was re-queued, it might run twice. Therefore, the worker logic MUST be idempotent.

---

## Q2 — Rolewize: Webhook Signature Rotation

> "You implemented HMAC signatures for Rolewize webhooks. If a merchant's secret key is compromised, how do you rotate the key without dropping webhooks while they update their servers?"

*Expected:*
- Zero-downtime key rotation.
- You generate a `New Key` while keeping the `Old Key` active.
- For a transition period (e.g., 24 hours), the webhook payload includes *two* signatures in the header: `Stripe-Signature: t=1611867133,v1=old_hash,v1=new_hash`.
- The merchant can verify against whichever key they currently have loaded. After 24 hours, you deprecate the old key.

---

## Q3 — CodeSync AI: Multi-Tenant Sandbox Security

> "SaaS companies isolate tenants ruthlessly. In CodeSync AI, you execute code in Docker. Did you run User A and User B's code in the same container? If not, how did you manage the lifecycle of ephemeral containers?"

*Expected:*
- "No, they must be in separate containers."
- Elaborate on container lifecycle: pre-warming a pool of generic Node.js containers, injecting the user code on demand, executing, capturing stdout, and immediately destroying the container to prevent lingering state or cross-tenant contamination.
