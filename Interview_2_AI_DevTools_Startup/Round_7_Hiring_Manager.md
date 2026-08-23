# Round 7 — Hiring Manager / Bar Raiser
**Interview:** AI / Dev-Tools Startup
**Duration:** 45–60 minutes

---

## Resume Defense

**I will read this line from your resume:**
> *"AI-powered code reviews... detecting bugs and generating precise code fixes"*

**Q1:** "Precise. How do you know the fixes are precise? What happens when Gemini generates a fix that compiles but is logically wrong? Do you validate AI output before showing it to the user?"

*What I want:* Honest acknowledgment that LLM output is not validated programmatically — it's shown as a suggestion, not a guaranteed correction. "Precise" is aspirational language. A strong answer proposes how you'd add validation: unit-test generation, static analysis on the suggested fix, or a disclaimer in the UI.

**Q2:** "You say you 'detect bugs.' Have you tested CodeSync AI's AI reviewer against code with known bugs? What's the false-positive rate?"

*What I want:* Either real testing data, or an honest "I haven't formally measured it" with a proposal for how you would.

---

## Judgment Scenario 1 — Feature vs Stability

> "The PM wants to ship voice chat to CodeSync AI rooms next sprint. You've thought about it and you're not confident it won't destabilize the real-time sync layer — WebRTC signaling and Socket.IO event ordering can interact in unpredictable ways. What do you do?"

**Strong answer framework:**
1. Make your concern explicit and technical — not "it feels risky" but "here's the specific interaction I'm worried about"
2. Propose a spike: 2–3 days of prototyping to validate the concern or disprove it, before committing to the sprint
3. If the PM still wants to ship: ask for a feature flag and limited rollout, so you can observe behavior on 5% of users before going wide
4. Document your concern in the sprint planning notes

---

## Judgment Scenario 2 — Data Loss

> "A user reports that their code disappeared from a CodeSync AI session. You check your logs and can't immediately reproduce it. You also know — because it's on your own 'Future Improvements' list — that you haven't built version history yet. What do you say to the user? And what does this incident change on your roadmap?"

**To the user:**
- Acknowledge the issue, apologize, don't make promises you can't keep
- Ask for reproduction details: browser, session ID, approximate time
- Don't say "it won't happen again" until you know why it happened

**To your roadmap:**
- This is a data integrity issue — it moves version history from "future improvement" to "urgent"
- Immediate quick win: log every ChangeEvent to a DB table with a timestamp — this gives you a replay log even without a full version control UI
- Longer term: snapshot the document state every N changes

---

## Close

**Q1:** "Why do you want to work at a dev-tools / AI startup specifically, versus a larger product company?"

**Q2:** "What would make you turn down our offer, assuming compensation is fair?"

**Q3:** "What's a feature you'd want to build in your first 3 months here, based on what you know about our product?"
