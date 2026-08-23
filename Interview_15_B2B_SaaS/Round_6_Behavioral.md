# Round 6 — Behavioral (STAR)
**Interview:** B2B SaaS / Enterprise Software

---

## Q1 — Writing for Other Developers
> "In B2B SaaS, your users are other developers. Tell me about a time you had to write documentation or design an API surface specifically to prevent another developer from shooting themselves in the foot."

*Expected:* Empathy for the developer experience (DevEx). "When building Rolewize, I noticed developers kept passing the raw JSON body to the HMAC verifier and failing because Express parsed it. I wrote a specific troubleshooting section in the README titled 'Why is my signature failing in Express?' and provided the exact 3 lines of code they needed to capture the raw body buffer."

---

## Q2 — The Production Outage
> "Tell me about a time a system you built (or worked on) went down. Not a bug, but a hard outage. What did you do during the outage, and what did you change the next week?"

*Expected:*
- **During:** Did not panic. Checked dashboards. Checked recent deployments. Rolled back the most recent deploy *before* trying to understand the root cause.
- **After:** Wrote a Post-Mortem. Did not blame a person ("John merged bad code"), blamed the system ("The CI pipeline lacked a test for this specific database constraint"). Added the missing test.

---

## Q3 — Prioritizing Tech Debt vs Features
> "Your startup CEO wants you to build 3 new integrations by Friday. You know the core webhook queuing system (QueueFlow) is currently built on a brittle architecture that might fail under load. How do you handle this conversation?"

*Expected:* B2B companies value stability over shiny features. 
"I translate the tech debt into business risk. I tell the CEO: 'If we build these 3 features, we will sign 3 new clients. But if QueueFlow fails on Friday because we didn't refactor it, we will lose the trust of our 50 existing clients, and they will churn. Let's build 1 integration this week, and spend the other half of the week stabilizing the queue.'"

---

## Q4 — The "Boring" Work
> "A lot of B2B SaaS engineering is 'boring' — parsing CSVs, mapping fields, writing database migrations. It's not always building distributed WebSockets. How do you stay motivated?"

*Expected:* "I find motivation in the scale and the reliability. Parsing a CSV is boring. Building a system that parses a 10GB CSV without OOMing the server, while providing real-time progress bars via WebSockets (CodeSync AI skills applied to B2B), is an exciting engineering challenge."
