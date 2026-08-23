# Round 3 — Past Experience & Project Grilling
**Interview:** AI / Dev-Tools Startup
**Duration:** 45–60 minutes
**Project in focus:** CodeSync AI — real-time collaborative code editor with sandboxed execution and AI-powered code review.

> **Interviewer's mindset:** You built a collaborative real-time editor. That's ambitious. I'm going to check if the details hold up, because the technical problems in this space — conflict resolution, sandboxing, fallback reliability — are genuinely hard. If you've thought deeply about them, we'll have a great conversation. If you haven't, we'll find out quickly.

---

## The Concurrency Gotcha — This Is Where Most Candidates Get Caught

**Q1:**
> "Two users are editing the same file in the same CodeSync AI room. They both type on the same line at the exact same millisecond. What actually happens to the final content of that file? Is it correct?"

*This is the most important question in this round. Think before you answer.*

**What I'm probing:**
- Raw Socket.IO broadcast is last-write-wins. If User A's change arrives at the server 10ms after User B's, User B's change is overwritten.
- This is a fundamental limitation of naive real-time sync. It doesn't cause crashes — it causes silent data corruption.
- The real solution is **Operational Transformation (OT)** or **CRDTs** (Conflict-free Replicated Data Types).

**Strong answer:**
> "Honest answer: in the current implementation, it's effectively last-write-wins. Both changes get broadcast, but the order they arrive at each client determines the final state. There's no conflict resolution. I'm aware this is a real limitation — for a production system, you'd need OT or CRDT. I understand the concept: OT transforms operations so they can be applied in any order and produce the same result. I haven't implemented it yet — it's in my 'Future Improvements' list and I'd tell you that straight."

**Weak answer (fails this round):**
> "Socket.IO handles it" — No, it doesn't. Socket.IO is just a transport layer.

---

## Docker Sandboxing — Attack Mitigations

**Q2:**
> "Your Docker sandbox disables networking, limits CPU and memory, limits the number of processes, runs as a non-root user, and enforces an execution timeout. For each of those restrictions, tell me what specific attack or failure mode it mitigates."

*Expected answers:*
| Restriction | What it mitigates |
|---|---|
| Disable networking | Code can't phone home, can't attack external services, can't exfiltrate data |
| Limit CPU | Prevents CPU exhaustion that would starve other containers/the host |
| Limit memory | Prevents memory bombs (`a = " " * 10**12`) from OOM-killing the host |
| Limit processes (`--pids-limit`) | Prevents fork bombs — `:(){ :|:& };:` |
| Non-root user | Limits blast radius if container is escaped — root in container = root on host without additional isolation |
| Execution timeout | Prevents infinite loops from holding a container indefinitely |

**Q3 (follow-up on fork bomb):**
> "Does your process limit actually stop a fork bomb, or does it just slow it down?"

*Expected:* With `--pids-limit=50`, after 50 processes are created, `fork()` calls return `EAGAIN` (resource unavailable). The bomb can't grow further. The existing 50 processes are still running and still consuming CPU — you need the CPU limit AND a timeout to fully neutralize it.

---

## Fallback Logic — Docker → Judge0

**Q4:**
> "When does execution fall back from Docker to Judge0? Is that decision made once at server startup, or per-request? And if Docker is healthy but just slow on a particular request — say, a container took 8 seconds to respond — do you fall back, or wait?"

*What I want:* A precise description of your actual fallback mechanism. Is it:
- Health-check based (ping Docker every N seconds, mark it down if it fails)
- Per-request with a timeout (if this request takes > X seconds, try Judge0)
- Or both?

If your fallback is purely health-check based and not timeout-based per request, a slow-but-alive Docker instance will never trigger the fallback. That's a valid design choice, but you should know it.

---

## AI Code Review — Gemini Integration

**Q5:**
> "Why does your Gemini code review prompt include the room's selected language? Can't Gemini detect the language from the code itself?"

*What I want:* A thoughtful answer. Yes, modern LLMs can detect language — but:
- Explicit language context reduces ambiguity (TypeScript vs JavaScript, C vs C++)
- You're paying per token — an explicit context prevents Gemini from wasting tokens on language detection
- It lets you tailor the review tone ("this is TypeScript — flag implicit `any` types")

---

## Database Choice — MongoDB vs Postgres

**Q6:**
> "You used MongoDB for user auth and codebase storage in CodeSync AI, but you used Postgres at Rolewize. What drove the different choice, and do you actually stand by it for CodeSync AI?"

*What I want:* An honest tradeoff, not a defensiveness spiral.
- MongoDB pros for CodeSync AI: flexible schema for storing code snapshots with arbitrary metadata, easy to store entire "sessions" as documents
- MongoDB cons: no transactions for multi-document consistency, joins are expensive, schema flexibility is also schema-chaos
- Honest acknowledgment: for a mature product, Postgres would likely be the right choice. MongoDB was faster to prototype with given the document-oriented nature of the data.
