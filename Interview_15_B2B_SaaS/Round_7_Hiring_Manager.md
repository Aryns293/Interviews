# Round 7 — Hiring Manager / Bar Raiser
**Interview:** B2B SaaS / Enterprise Software

---

## Resume Defense — The "Not Invented Here" Syndrome

> "You built your own message queue (QueueFlow). You built your own Git plumbing (gitlight). In a B2B startup, our goal is to ship value to customers, not rebuild infrastructure. If you joined us, would you try to build your own tools, or are you comfortable using AWS SQS and GitHub out of the box?"

*What I'm testing:* Can you differentiate between a learning project and production pragmatism?
*Strong answer:* "I build tools to understand them, so I know how to use the enterprise versions effectively. Building QueueFlow taught me exactly *why* AWS SQS has visibility timeouts and dead-letter queues. Because I understand the internals, I can debug SQS issues faster. In a professional setting, I will always advocate for managed services (SQS, Kafka, standard Git) because rebuilding infrastructure offers zero competitive advantage to our core product."

---

## Judgment Scenario — The Deprecation Nightmare

> "We are shutting down the `v1` API next month. 100 enterprise customers are still using it, despite us warning them for a year. If we turn it off, their internal tools will break, and they will call our CEO furious. If we leave it on, it blocks our massive database migration. What is your plan?"

*Expected:*
1. **Never just turn it off.** B2B relies on trust.
2. **The "Brownout" Strategy:** You don't turn it off permanently. You turn it off for 5 minutes during business hours. Then 1 hour the next day. The sudden HTTP 500/410 errors trigger their PagerDuty alarms, forcing their engineers to realize the deadline is real, but allows them to recover when the brownout ends.
3. **Targeted Outreach:** Look at the DB. See exactly *which* endpoints the 100 users are hitting. Have the Account Managers call them with a specific payload: "You are still hitting `/v1/users`, please change it to `/v2/users`."

---

## Close

**Q1:** "What is the most complex system you've built that nobody ever saw the code for, but it ran perfectly in the background?"

**Q2:** "Questions for me?"
