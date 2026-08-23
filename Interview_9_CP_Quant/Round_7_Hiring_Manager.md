# Round 7 — Hiring Manager / Bar Raiser
**Interview:** Competitive-Programming-Heavy / Quant-Adjacent Company

---

## Resume Defense — The Rating Divergence

> "You list LeetCode Knight (1924, top 3%) and Codeforces Specialist (1411). These are different platforms with different rating scales. Codeforces is mathematically heavier. Which one do you think reflects your actual strength better, and why aren't they more aligned?"

*What I'm testing:* Self-awareness.
- Do not say "LeetCode is just as hard." It isn't, and a quant interviewer knows it.
- **Strong answer:** "Codeforces 1411 reflects my mathematical and combinatorial ceiling better. LeetCode 1924 reflects my speed at recognizing standard patterns. Codeforces tests novel insights; LeetCode tests how fast you can implement a known pattern. I'm faster at pattern matching than I am at deriving novel number theory proofs under pressure, which is why there's a divergence."

---

## Judgment Scenario — The Production Math Bug

> "Your matching engine has been using floating-point for price comparisons in production. Someone just found a rounding edge case that let one order execute at a slightly wrong price (e.g., $100.00001 instead of $100.00). It's already shipped and running. What do you do — right now, and structurally?"

**Right now:**
- In quant/fintech, bad trades cost real money immediately.
- **Halt trading / kill switch:** If the system supports pausing the matching engine, do it immediately.
- Roll back to the previous stable version if the floating-point logic was recently introduced.
- If it's been there forever, you must assess the blast radius: how many trades were affected?

**Structurally:**
- Never use floating point for currency or exact matching.
- Migrate the entire engine to use integer arithmetic (store prices in ticks/cents, e.g., $100.00 = 10000).
- Write a migration script to fix the affected records in the ledger.
- Inform compliance/legal.

---

## Close

**Q1:** "Why algorithmic/low-latency engineering specifically, over general product work?"
*(Say something about enjoying the constraint of extreme performance, not just 'I like CP').*

**Q2:** "Do you actually want to write C++/Rust in a low-latency environment, or are you happier in Node.js/TypeScript building full systems?"
*(Honest answer required — quant firms hire for the former).*

**Q3:** "Questions for me?"
