# Round 3 — Past Experience & Project Grilling
**Interview:** Remote-First / Fully Distributed Team Company
**Duration:** 45 minutes

> **Interviewer's mindset:** I want to know if you can function independently. I don't care how smart you are if I have to babysit you over Slack for 4 hours a day.

---

## Q1 — The Rolewize Remote Reality

> "Rolewize was fully remote. How did you actually get context on what to build? Walk me through onboarding into a codebase you'd never seen, with a team you'd never met in person."

*What I want to hear:*
- "I read the documentation." (If there was any).
- "I set up local environments by reading the docker-compose or package.json."
- "I batched my questions. Instead of pinging my manager 10 times a day, I spent 2 hours trying to unblock myself, wrote down 3 specific questions, and asked them async on Slack with context." (This is the golden answer for remote work).

---

## Q2 — Cross-Team Assumptions

> "Your webhook idempotency work touches other services' behavior (e.g., retries). In a remote team, how did you make sure your assumptions about their retry behavior were correct, without a hallway conversation?"

*Expected:* "I read their code." Or, "I wrote a design doc proposing the idempotency contract, tagged the engineers who owned the upstream services, and asked for async review before I wrote any code."

---

## Q3 — The Solo Developer Trap

> "All three of your major projects — CodeSync AI, QueueFlow, gitlight — look like solo work. Is that accurate? If so, how do you know your code would actually be reviewable and understandable by someone who joins cold, with no verbal walkthrough from you?"

*Expected:*
- Acknowledge the risk of solo coding (you can get away with bad naming, no comments, spaghetti architecture because it all fits in your own head).
- Counter it: "I forced myself to write proper READMEs for all of them. I use standard patterns (like MVC, or specific design patterns) so the structure is predictable. I write self-documenting code rather than clever code."
