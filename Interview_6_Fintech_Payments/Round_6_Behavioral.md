# Round 6 — Behavioral (STAR)
**Interview:** Fintech / Payments Company
**Duration:** 45 minutes
**Note:** Every answer must be tied to your Rolewize internship or your projects. In fintech, security and financial-consistency behavioral questions carry real weight — a vague answer signals you didn't actually think about the implications of what you built.

---

## Q1 — Conflict / Security Tradeoff Disagreement
> "Tell me about a disagreement you had at Rolewize about a security or design decision — for example, around the TTL for presigned URLs, how strict MIME validation should be, or any other security-adjacent choice."

*What I'm listening for:*
- Did you have an opinion backed by a specific threat model, or did you just go with whatever was decided?
- Did you articulate the risk clearly to whoever you disagreed with?
- If you were overruled — did you document your concern?

---

## Q2 — Failure / Idempotency Edge Case
> "Tell me about an edge case in your idempotency logic that you didn't catch until later. What was the gap, and how did you find it?"

*If no specific edge case comes to mind:* "Walk me through a scenario where your current idempotency implementation would silently fail to prevent a duplicate. What's the smallest change that would expose that gap?"

---

## Q3 — Pressure / PII in Production
> "You were shipping production webhook infrastructure handling real resume PII on a 2-month internship clock. Walk me through how you actually thought about the security posture — was it 'ship fast and fix later,' or did you treat it as if it were a fintech product from day one?"

*What I'm listening for:*
- Did you actively think about PII, or was security an afterthought?
- Did you push back on any deadlines because of security concerns?
- What would you do differently if you had 6 months instead of 2?

---

## Q4 — Ambiguity / Choosing Security Thresholds
> "No spec told you to use a 15-minute TTL or to implement timing-safe HMAC comparison. How did you actually land on those decisions?"

*What I want:* Evidence that you researched these decisions — AWS docs on presigned URL best practices, Node.js `crypto.timingSafeEqual` docs, OWASP webhook security guidelines. Or honest acknowledgment that you made a judgment call and here's your reasoning.

---

## Q5 — Initiative / MIME Validation
> "MIME type validation on resume uploads — was that something Rolewize asked for, or did you add it because you saw the security gap yourself?"

*What I want:* If you added it yourself — walk me through the moment you realized it was needed. If it was asked for — what would you have added on your own if given more time?

---

## Scoring Rubric
| Dimension | Strong | Weak |
|---|---|---|
| Security mindset | Thinks in threat models, not just feature delivery | "I followed best practices" with no specifics |
| Ownership | Made independent security decisions and can defend them | "I implemented what I was told" |
| Honesty | Knows where gaps exist in their own implementation | Claims perfect security without nuance |
| Communication | Can explain a technical security tradeoff to a non-engineer | Uses jargon without explanation |
