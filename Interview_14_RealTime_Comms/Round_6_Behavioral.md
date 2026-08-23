# Round 6 — Behavioral (STAR)
**Interview:** Real-Time Communications / Streaming

---

## Q1 — Managing Latency Expectations
> "Tell me about a time you optimized a piece of code for latency (e.g., CodeSync AI). Did you measure the baseline first, or did you just guess what was slow?"

*Expected:* "Always measure first." Describe using Chrome DevTools Performance tab, or Node.js profiling, to prove that the JSON serialization was taking 20ms before optimizing it. Guessing is an anti-pattern.

---

## Q2 — Handling Connection Drops
> "Real-time systems fail ungracefully. Tell me about a time in your internship or projects where a silent failure caused a terrible user experience, and how you fixed it."

*Expected:* WebSockets dropping without throwing an error, leaving the user staring at a frozen screen thinking their partner is just typing slowly. The fix: implementing a visible "Reconnecting..." banner based on heartbeat timeouts, prioritizing clear UI state over hoping the network fixes itself.

---

## Q3 — Dealing with High-Velocity Data
> "QueueFlow processes data in the background. CodeSync AI processes it instantly. Tell me about a time you had to decide whether a feature should be synchronous (real-time) or asynchronous."

*Expected:* "In CodeSync, executing the code was slow (Docker startup). Initially I tried to block the socket and wait for it. The UX was terrible. I refactored it: the click to 'Run' is sync, but the execution is async, returning a Job ID. The frontend polls or listens on the socket for the result. Never block the main real-time pipe with a heavy compute task."

---

## Q4 — The "Noisy Neighbor" Problem
> "Have you ever dealt with a situation where one user/process monopolized resources and ruined the experience for everyone else?"

*Expected:* Mention Docker cgroups limiting CPU in CodeSync AI, or rate-limiting webhooks per-merchant in Rolewize so one bad merchant doesn't flood the queue.
