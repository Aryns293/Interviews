# Round 3 — Past Experience & Project Grilling
**Interview:** General Product-Based Company
**Duration:** 45–60 minutes
**Style:** All three projects get touched. Tight walkthroughs, real tradeoffs, hindsight.

---

## One End-to-End Walkthrough Per Project (10 min each)

### CodeSync AI
> "Trace a user clicking 'Run Code' in CodeSync AI to the output appearing in their terminal panel. Every step."

*Expected:* Browser click → Socket.IO event emitted → Server receives `run_code` event → Creates ExecutionRequest → Spins up Docker container (or falls back to Judge0) → Runs code with timeout → Streams/sends output back via Socket.IO → Client receives `execution_result` event → Renders in terminal panel.

*I'll stop you if you skip the fallback mechanism or the timeout handling.*

---

### QueueFlow
> "Trace a job being enqueued to it either completing successfully or landing in the dead-letter queue. Every step."

*Expected:* API call → idempotency check (Redis) → idempotency check (Postgres) → LPUSH to priority Redis queue → BullMQ worker picks up via BRPOP → Executes job → On success: mark done in Postgres → On failure: retry with exponential backoff (up to 3 attempts) → After 3 failures: move to dead-letter queue.

---

### gitlight
> "Trace `gitlight add file.txt` to `gitlight commit -m 'msg'`. What exists on disk after each command?"

*Expected add:* Read file → build blob object (header + content) → SHA-1 hash → zlib compress → write to `.git/objects/` → update `.git/index` (staging area) with file path → blob hash mapping.

*Expected commit:* Read index → build tree object from index entries → hash and store tree → build commit object (tree hash, parent hash, author, message) → hash and store commit → update `.git/refs/heads/main` (or current branch) to new commit hash.

---

## Cross-Project Tradeoff Questions

**Q1:**
> "Why Redis over Kafka/RabbitMQ for QueueFlow? Why MongoDB over Postgres for CodeSync AI — when you used Postgres at Rolewize? Defend BOTH choices with real tradeoffs."

*Redis vs Kafka:* Redis was the right choice for a project at this scale — simpler ops, built-in sorted sets for priority queues, fast. Kafka is better for extremely high throughput, durable replay, and multi-consumer fan-out. Acknowledging this is honest.

*MongoDB vs Postgres for CodeSync AI:* MongoDB's document model fits collaborative sessions (variable-structure code snapshots, arbitrary metadata). Postgres would give better relational guarantees but requires more upfront schema design. Honest answer: for a prototype, MongoDB was faster to iterate. For production, Postgres is probably right.

---

**Q2:**
> "What breaks in QueueFlow when traffic goes from 1,000 concurrent jobs to 100,000? Where's the first bottleneck?"

*Expected:*
1. **Redis memory** — each job in the queue takes memory. At 100k jobs, you need to calculate average job payload size × 100,000.
2. **Postgres write throughput** — each job completion writes to Postgres. A single Postgres instance handles ~10,000 writes/sec with proper indexing and connection pooling.
3. **Worker count** — each BullMQ worker processes one job at a time. You need horizontal scaling of workers.
4. First bottleneck is likely worker count — add more worker processes before touching Redis or Postgres.

---

**Q3 — Hindsight:**
> "If you could rewrite just ONE of your three projects from scratch, which one and what's the first thing you'd change architecturally?"

*This is genuinely open-ended — I want to hear what you regret, not what you'd add.*
