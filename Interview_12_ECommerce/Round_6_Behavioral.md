# Round 6 — Behavioral (STAR)
**Interview:** E-Commerce / High-Traffic Retail

---

## Q1 — Customer Obsession
> "Tell me about a time you noticed a bug or a poor UX in one of your projects (CodeSync AI or QueueFlow) and went out of your way to fix it even though it wasn't strictly required."

*Expected:* E-commerce companies (especially Amazon/Flipkart) obsess over customer experience. Did you add a loading spinner? Did you improve the error message when a webhook failed? Give a specific example.

---

## Q2 — Bias for Action vs Dive Deep
> "Tell me about a time you had to make a decision quickly with incomplete data, and later realized you were wrong once you had the full picture."

*Expected:* Discuss a technical choice. E.g., "I chose MongoDB for CodeSync AI because I needed to move fast (Bias for Action). But when I started thinking about the collaborative editing conflict resolution, I realized a document store was the wrong shape for sequential operational transforms (Dive Deep), and I should have used an event-sourced SQL schema."

---

## Q3 — Ownership / The Missing Logs
> "You ship the QueueFlow backend. A user complains their job didn't run. You check the system and there is no trace of the job ever existing. Walk me through your immediate emotional and technical response."

*Expected:* 
- Emotional: Assume the system is broken, not the user. Ownership means taking the bug report seriously.
- Technical: "I add request logging at the very edge (the API Gateway/Nginx) before it even hits Node.js. If it's not in the Node logs, I need to know if the network request ever reached my server."

---

## Q4 — Invent and Simplify
> "QueueFlow has a lot of features (retries, rate limiting, pub-sub). If you had to delete 50% of the codebase to make it easier to maintain, what would you cut, and why would the remaining 50% still be valuable?"

*Expected:* Simplicity scales. "I would cut rate-limiting and custom retry strategies. I would keep the core atomic job popping and the delay logic. It's better to do one thing (reliable background execution) perfectly than 5 things poorly."
