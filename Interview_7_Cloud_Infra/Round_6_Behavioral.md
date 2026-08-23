# Round 6 — Behavioral (STAR)
**Interview:** Cloud Infrastructure / Platform Engineering Company

---

## Q1 — Conflict / Infrastructure Choice Disagreement
> "Tell me about a time you disagreed with someone about an infrastructure or hosting choice — whether that's Render vs another platform, a Docker configuration decision, or any infra-adjacent call."

---

## Q2 — Failure / Sandbox Instability
> "Was there a Docker sandbox instability or resource-limit misconfiguration in CodeSync AI that you discovered later than you'd have liked? Walk me through what happened."

*If no Docker instability:* "Walk me through the moment you realized the SELF_PING workaround was necessary — what broke, how did you find out, and what would a real fix have looked like?"

---

## Q3 — Pressure / Execution Image Stability
> "Getting the execution image working reliably before a deadline — whether a hackathon demo, GRiD submission, or just your own ship date. Walk me through that experience."

---

## Q4 — Ambiguity / Choosing Resource Limits
> "No one gave you exact CPU or memory numbers for your sandbox container. How did you actually arrive at the limits you used?"

---

## Q5 — Initiative / Judge0 Fallback and SELF_PING
> "The Judge0 fallback and the SELF_PING keep-alive — were those in any spec, or did you add them because you saw the gap yourself? Walk me through the decision."

*What I'm listening for:* Proactive reliability thinking. Did you think about "what happens when Docker goes down?" before it happened, or after?

---

## Scoring Rubric
| Dimension | Strong | Weak |
|---|---|---|
| Reliability thinking | Built fallbacks and self-healing proactively | Only fixed things after they broke |
| Infrastructure awareness | Knows WHY infrastructure decisions were made | "I just used what was in the tutorial" |
| Honesty | Admits the SELF_PING is a workaround | Claims it's a "feature" |
| Growth | Has a clear picture of what a production-grade version would look like | No post-project reflection |
