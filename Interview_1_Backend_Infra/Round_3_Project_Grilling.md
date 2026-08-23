# Round 3 — Past Experience & Project Grilling
**Interview:** Backend / Infra-Heavy Product Company
**Duration:** 45–60 minutes
**Format:** Conversational, but I will push hard. There are no trick questions — only your projects. If you built it, you should be able to answer any question about it. If you can't, that's the red flag.

> **Interviewer's mindset:** I've read your resume. I'm not testing whether you know textbook concepts — I'm testing whether the things on your resume are real. Every "claim" is a contract. I will cash every one of them.

---

## Section 1 — Rolewize Internship: Webhook Pipeline

**Q1 (opening, broad):**
> "Walk me through your Rolewize webhook endpoint end-to-end. Start from the moment an external service sends an HTTP POST to your URL, and end when the job is safely sitting in BullMQ. Don't skip anything."

*I'm listening for:* Signature verification → raw body extraction → idempotency check → job enqueue. Missing any step is a flag.

---

**Q2 (follow-up — the dual-layer idempotency):**
> "You used both Redis and Postgres for idempotency. Why both? What problem does each layer solve that the other doesn't?"

*Expected answer:*
- **Redis layer:** Fast in-memory dedup on the hot path — blocks duplicate requests before they touch the DB. TTL-based (e.g., 24h), so it's not permanent.
- **Postgres layer:** Durable, permanent record. Even after Redis TTL expires or Redis restarts, you can still detect a late-arriving duplicate from months ago.
- **What breaks with Redis only:** Redis is volatile. A crash or TTL expiry loses the dedup record. A replay attack after 24h slips through.
- **What breaks with Postgres only:** Every webhook call hits the DB for a uniqueness check — becomes your bottleneck at volume.

---

**Q3 (follow-up — the LLM cache):**
> "Your Redis cache uses SHA-256 hashing of resume content with a 24-hour TTL to eliminate duplicate LLM API calls. What is the exact cache key? What happens if the same resume content is uploaded, but the LLM prompt template or model version has changed — do you serve the stale cached result?"

*What I want to hear:* The cache key must include BOTH the content hash AND the prompt/model version identifier. If it doesn't, a prompt update silently serves stale results. This is a real production bug in naive caching implementations.

---

## Section 2 — QueueFlow: The Job Queue System

**Q4:**
> "You claim O(1) enqueue and dequeue using LPUSH and BRPOP. But you have 3-tier priority logic on top. Is that claim still true? Walk me through what actually happens across all 3 tiers for a single dequeue call."

*What I want to hear:* With 3 priority queues (`high`, `medium`, `low`), a single dequeue call is actually 3 `BRPOP` calls in priority order in the worst case (if high and medium are empty and you drain to low). So it's O(1) per tier, but O(k) where k = number of priority levels. That's fine, but the candidate should be honest about it — claiming flat O(1) is imprecise.

---

**Q5:**
> "Why did you choose Redis over Kafka or RabbitMQ for a queue that claims zero data loss? Defend that choice knowing that Redis AOF persistence can still have up to 1 second of data loss by default."

*What I want to hear:*
- Redis with `appendfsync always` gives true durability but at performance cost
- Kafka is the industry standard for truly durable, exactly-once delivery — Redis is the pragmatic choice for a project at this scale
- Honest acknowledgment: "zero data loss" is aspirational unless `appendfsync always` is configured. If it's not configured that way, the claim should be softened.

---

**Q6 (hindsight):**
> "If you rebuilt QueueFlow today from scratch, what's the first thing you'd change? Not add — change."

*What I'm testing:* Do you have genuine retrospective depth on your own work, or do you give a rehearsed "I'd add monitoring" answer? I want to hear a real architectural regret.

---

## What Gets You Through This Round

| Signal | Pass | Fail |
|---|---|---|
| End-to-end walkthrough | Covers every layer unprompted | Skips signature verification or idempotency |
| Dual-layer idempotency | Explains the failure mode of each layer in isolation | "Both are for safety" — no real tradeoff articulation |
| LLM cache key | Includes prompt/model version in the key | Content hash only |
| O(1) claim on priority queue | Accurately qualifies it as O(k) tiers | Defends flat O(1) without nuance |
| Redis vs Kafka choice | Acknowledges durability tradeoff honestly | "Redis is faster" — no real tradeoff |
| Hindsight | Specific architectural regret with reasoning | "I'd add more features" |
