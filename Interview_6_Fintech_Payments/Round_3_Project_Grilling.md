# Round 3 — Past Experience & Project Grilling
**Interview:** Fintech / Payments Company
**Duration:** 45–60 minutes
**Project in focus:** Rolewize internship — HMAC webhook verification, dual-layer idempotency, presigned S3 URLs, MIME validation, BullMQ retry.

> **Interviewer's mindset:** You built production infrastructure handling PII and external webhook delivery at a company with real users. I'm going to audit every security claim. In fintech, "good enough" and "probably secure" are not acceptable answers.

---

## Q1 — Cross-Instance Race in Idempotency

> "You deployed the webhook handler. Two copies of the same webhook event arrive at the same millisecond — but they land on two **different server instances** (different Node.js processes, different machines). Does your idempotency layer catch that race, or only races within a single process?"

*What I want:*
- In-memory dedup (e.g., a JavaScript `Set`) only works within one process — it catches nothing across instances
- Redis SET NX IS cross-process safe — the atomic `SET key NX PX ttl` is a single Redis command that cannot race across multiple callers
- So: if your Redis layer is the first dedup gate, yes — it catches cross-instance races
- If your first gate is in-memory, you have a cross-instance gap and need to answer honestly about it

**Follow-up:** "What's the window between two instances both doing a Redis GET before either does a SET NX?" → This is why you must use `SET NX` (atomic) not `GET then SET` (race condition between the two operations).

---

## Q2 — Presigned URL TTL Threat Model

> "You used 15-minute presigned S3 URLs for resume uploads. Why 15 minutes specifically? What's the actual threat model — what breaks if the TTL is too long, and what breaks if it's too short?"

*Expected:*
- **Too long (e.g., 60 min):** If a URL is intercepted (via browser history, logs, a third-party analytics script reading the URL), the attacker has a full hour to upload malicious content to your S3 bucket under a legitimate key
- **Too short (e.g., 2 min):** Users on slow connections or who navigate away and return find their URL has expired — bad UX, forces re-generation which adds server load and latency
- **15 minutes:** Industry standard for upload URLs — enough for a normal user upload flow, short enough to limit interception window
- **Additional control:** Presigned upload URLs should also restrict the `Content-Type` and maximum `Content-Length` at the S3 policy level — not just the TTL

---

## Q3 — MIME Validation Depth

> "You validate MIME types on resume uploads. Does your validation check the client-declared `Content-Type` header, or does it actually sniff the file's magic bytes?"

*Expected:*
- **Client-declared Content-Type:** Trivially spoofable. `curl -H "Content-Type: application/pdf" --data-binary @malware.exe` — attacker sends an executable claiming to be a PDF
- **Magic byte sniffing:** Read the first few bytes of the file and compare against known file signatures. PDF starts with `%PDF`. PNG starts with `\x89PNG`. This cannot be spoofed without corrupting the file itself.
- **Correct implementation:** Use a library like `file-type` (Node.js) that reads magic bytes — do NOT trust the Content-Type header for security decisions
- **What I want to know:** Which one did you actually implement at Rolewize?

---

## Q4 — BullMQ Retry and Financial Consistency

> "Your BullMQ webhook handler at Rolewize retries up to 3 times. If attempt 2 **partially succeeds** — the job side-effects fire (e.g., data is written to the DB) but the acknowledgment back to BullMQ is lost due to a process crash — does attempt 3 re-fire the side-effects? Does anything get double-processed?"

*This is the core idempotency question. It exposes whether you actually thought about exactly-once processing.*

*Expected:*
- Yes — if the job function is not idempotent, attempt 3 will re-execute and double-process
- The fix: your job handler must be idempotent. Before executing side-effects, check whether they've already been applied (e.g., check the DB for the event ID before writing)
- BullMQ's `removeOnComplete`/`removeOnFail` options affect job retention, not idempotency
- In a fintech context, double-processing a webhook could mean a double charge, double credit, or duplicate ledger entry — none of which are acceptable

---

## Q5 — PII Data Handling

> "Resume files are PII. Walk me through every point in your Rolewize system where that PII touches a surface — storage, transit, processing — and tell me what protection was in place at each point."

*Expected touch points:*
1. **Upload:** Presigned HTTPS URL (in-transit encryption via TLS)
2. **S3 storage:** SSE-S3 or SSE-KMS at rest
3. **Lambda/worker processing:** Pulled from S3 over HTTPS, processed in memory — was the memory wiped after processing?
4. **LLM API call:** Resume content sent to an external LLM API — was it transmitted over HTTPS? Did you check the LLM provider's data retention policy?
5. **Redis cache:** SHA-256 hash of the resume content is the key, but is the cached LLM *response* (which may contain extracted PII) stored in Redis? For how long? Is Redis encrypted at rest?

*I'm not expecting perfection — I'm checking whether you've thought about every surface.*
