# Round 7 — Hiring Manager / Bar Raiser
**Interview:** Developer Productivity / Tooling Company

---

## Resume Defense — The "Why did you build it this way?"

> "CodeSync AI uses MongoDB. gitlight is built in Node.js. For tooling companies, performance is a feature, not a nice-to-have. MongoDB is rarely used for high-frequency collaborative text editing. Node.js is rarely used for CLI tools that need to start up in 10ms. Defend these choices."

*What I'm testing:* 
- **The wrong answer:** Arguing that Node.js is "fast enough" for a CLI or that Mongo is "the best" for text editing.
- **The right answer:** "I chose them because I wanted to optimize for shipping speed to prove out the core logic (AST traversal, DAGs, WebSockets). If I were building `gitlight` for production, I'd rewrite it in Rust or Go for the 5ms startup time and zero-dependency binary distribution. If I were scaling CodeSync AI, I'd use Redis for the ephemeral collaborative state and only flush to Postgres/Mongo periodically. But picking Node/Mongo allowed me to build the whole system in weeks instead of months."

---

## Judgment Scenario — The Broken Release

> "You just merged a PR that upgraded the core parsing library in our main product. Tests passed. But 10 minutes after deployment, the community slack channel blows up — the new parser is crashing on a very specific, undocumented syntax used by 5% of our enterprise users. What do you do?"

*Expected:*
1. **Revert first, investigate second.** Developer trust is fragile. Do not try to "roll forward" with a hotfix while users are broken.
2. Acknowledge that the test suite has a blind spot.
3. Write a regression test capturing that specific weird syntax.
4. Fix the parser locally, verify against the new test, and re-release.

---

## Close

**Q1:** "What is the worst developer experience (DX) you've ever had with a tool, and how would you fix it if you owned that tool?"
*(Have a good rant ready. Webpack config? AWS IAM roles? Make it specific.)*

**Q2:** "Questions for me?"
