# Round 3 — Past Experience & Project Grilling
**Interview:** Early-Stage Seed Startup
**Duration:** 45–60 minutes
**Style:** Candid. I'm not grading against a perfection bar — I'm grading for self-awareness, pragmatism, and whether you can ship in a resource-constrained environment.

> **Interviewer's mindset:** I need someone who can work fast, make judgment calls independently, and know when to cut corners vs when NOT to. Your 3 projects tell me you can build. This conversation tells me how you think about what you build.

---

## Q1 — Honest Self-Assessment: Polish vs Ship

> "You built CodeSync AI, QueueFlow, and gitlight — three full systems — on top of an internship and a CGPA of 8.11, in under two years. Walk me through each project and tell me honestly: what was 'good enough to ship' vs 'polished,' and where did you deliberately cut corners?"

*What I'm listening for:*
- **CodeSync AI:** Was OT/CRDT implemented? No → that's a known cut corner. Was version history implemented? Probably not → another cut.
- **QueueFlow:** Is "zero data loss" actually guaranteed with AOF config? Probably not by default. Is the UI production-grade? Probably not.
- **gitlight:** Which Git commands aren't implemented? (cherry-pick, stash, worktree?) What's the coverage of edge cases in the LCS diff?

*The candidate who says "everything is fully polished" is less credible than the one who says "here's exactly where I cut corners and why."*

---

## Q2 — 48-Hour QueueFlow MVP

> "If I gave you 48 hours to turn QueueFlow into something a paying customer could use tomorrow, what's the absolute minimum you'd add, and what would you explicitly leave broken?"

*Structuring this answer:*

**Must add in 48 hours:**
- Authentication (JWT — users can't manage other users' queues)
- API documentation (Swagger/Postman collection)
- Error messages that a non-engineer can read
- A simple dashboard showing queue depth and job status (even if it's just a JSON endpoint)
- Rate limiting on the API (prevent abuse on day 1)

**Explicitly leaving broken:**
- Exactly-once delivery guarantee (at-least-once is fine for a first customer)
- The UI — a JSON API is enough to validate the product
- Multi-tenant data isolation beyond basic auth (assuming the first customer is internal)
- Observability/alerting (you'll add when the first thing breaks)

---

## Q3 — Rolewize: Structured or Self-Directed?

> "How much of the webhook/idempotency design at Rolewize was specified in a ticket vs your own call? Was the internship structured with clear deliverables, or were you figuring out scope yourself?"

*What I want:*
- Honest account of the internship structure
- Specifically: did someone hand you the HMAC verification approach, or did you research and propose it?
- Did you receive code reviews, or were you largely unsupervised?
- This tells me how you'll operate in an early-stage startup where nobody will specify your tickets for you

---

## Q4 — Speed vs Polish: One Right, One Wrong

> "Give me a concrete example from your projects where you consciously chose speed over polish — and one where, in retrospect, you think you chose wrong and should have taken more time."

*Strong examples:*

**Correctly chose speed:** Using MongoDB for CodeSync AI instead of spending days on a Postgres schema — the document model fit the prototype well and let you ship faster.

**Wrongly chose speed (possible examples):**
- Not implementing magic-byte MIME validation at Rolewize — relied on Content-Type header, which is spoofable
- Leaving the QueueFlow UI as a barebones HTML form instead of investing a day in a proper React UI — made demos harder
- Not adding structured logging from day 1 — debugging issues became much harder without log context
