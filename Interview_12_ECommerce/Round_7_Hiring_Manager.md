# Round 7 — Hiring Manager / Bar Raiser
**Interview:** E-Commerce / High-Traffic Retail

---

## Resume Defense — Throughput Claims

> "You claim your systems are highly performant. If I put QueueFlow under a load of 10,000 jobs per second, where exactly will it break first?"

*What I'm testing:* Can you find the bottleneck in your own design?
- **Bad answer:** "It won't break, Redis is fast."
- **Good answer:** "Redis can handle 10k ops/sec easily, but the bottleneck will be the Node.js workers. Node is single-threaded. If the job payload is heavy, CPU serialization (JSON.parse/stringify) will block the event loop. If the job requires a network call, we will exhaust the OS connection pool or run out of ephemeral ports on the worker machines long before Redis dies."

---

## Judgment Scenario — The Bad Data Migration

> "We are migrating user cart data to a new database schema. You wrote the script. It ran last night. Today, 5% of users are complaining that items they added to their cart yesterday are missing. We are losing revenue by the minute. What do you do?"

*Expected:*
1. **Stop the bleeding:** If the old database is still up and receiving writes, do not touch it. If the new database is live, you have a split-brain problem.
2. **The Immediate Fix:** If the 5% data loss is isolated (the script just missed some rows), write a patch script to sync the missing 5% from Old DB to New DB immediately, while keeping the New DB live.
3. **If it's corrupted (worse):** Roll traffic back to the old database. The data written in the last 12 hours to the new DB will need to be manually merged back later. Revenue loss *right now* is worse than the pain of a manual merge tomorrow.

---

## Close

**Q1:** "E-commerce is often about small optimizations (shaving 100ms off load time). Do you enjoy that kind of microscopic performance tuning, or do you prefer greenfield architecture?"

**Q2:** "Questions for me?"
