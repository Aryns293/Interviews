# Round 7 — Hiring Manager / Bar Raiser
**Interview:** FAANG-Style / Amazon Bar Raiser Loop
**Duration:** 45–60 minutes
**Style:** This is the hardest final round. I pick TWO resume lines (not one), I stack judgment scenarios with follow-ups, and I close with questions that have no "right" answer.

> **Bar raiser role:** I am not the hiring manager. My job is to ensure this hire raises the average bar of the team. I can veto a hire even if every other interviewer voted yes.

---

## Resume Defense — Two Lines, Back-to-Back

**Line 1 — I'll pick this:**
> *"Solved 1,500+ DSA and CP problems"*

**My questions:**
- "What fraction were Medium/Hard vs Easy? Give me a rough split."
- "Tell me about a problem that actually changed how you think about a class of problems — not the hardest, but the most instructive."
- "You hold a Knight rating on LeetCode. What does Knight mean to you — a floor you're comfortable at, or a ceiling you're pushing against?"

**Line 2 — I'll pick one of your project bullets at random:**

*Possible picks:*
- "Reduced LLM costs by X%" → "How did you measure the reduction? What was the baseline, and what methodology did you use?"
- "Sub-second enqueue latency" → "Under what load profile? What's the p99 at 1,000 concurrent enqueues?"
- "End-to-end encrypted" (if listed) → "Encrypted at rest, in transit, or both? Who holds the encryption keys?"

---

## Judgment Scenario 1 — Stacked (Two Layers)

**Layer 1:**
> "Your manager asks you to ship something you believe is fundamentally broken — a merge algorithm that you know produces wrong results on 5% of inputs. The deadline is tomorrow. What do you do?"

*(Wait for your full answer.)*

**Layer 2 (immediately after):**
> "Your manager hears you out. They say: 'I understand, but we're shipping it — it's my call, not yours.' What do you do now?"

*What I'm testing with Layer 2:* **Disagree and Commit** vs **Ownership**. The right answer is: you've made your concerns explicit (in writing, if possible), you've been overruled, you commit to the decision and execute — but you also ensure the known defect is tracked and escalated via official bug-tracking channels. You do NOT silently comply and you do NOT go over your manager's head without a very serious reason.

**What fails:**
- "I'd comply completely" — no ownership, no accountability for the defect
- "I'd escalate over my manager's head" — first step is too aggressive; reserved for ethical violations, not technical disagreements
- "I'd refuse to ship it" — insubordination without very serious justification

---

## Judgment Scenario 2 — The Differentiator Question

> "Why should we hire you over another candidate who has a similar LeetCode rating, a similar CGPA, and projects of similar complexity? Be specific — not 'I work hard' or 'I'm passionate.'"

*What I'm testing:* Self-awareness of your actual differentiators. Generic answers fail this immediately.

**What a strong answer sounds like:**
- Ties to a specific, concrete thing that other candidates with similar surface-level metrics wouldn't have: "I've built systems that are running in production, not just tutorials — I can speak to the failure modes of idempotency at the boundary of Redis and Postgres because I've debugged that boundary." 
- Or: "I've reimplemented Git's plumbing layer — not because it's on a hiring checklist, but because I wanted to understand how content-addressing actually works at the byte level. That kind of self-directed depth is not something you can fake with LeetCode practice."

---

## Close — Harder Edge

**Q1:** "What would make you turn down an offer from us, assuming compensation is fair?"

*This is not a trap.* I genuinely want to know. Candidates who say "nothing" signal they haven't thought about their own career. Candidates with a thoughtful answer signal self-awareness.

**Q2:** "Where do you see yourself in 3 years — not title-wise, but in terms of the problems you want to be solving?"

**Q3:** "Any questions for me?"

*Questions that distinguish you:*
- "What's the biggest technical problem on this team that your last 3 hires couldn't crack?"
- "What does the internal bar-raiser process look like at this company — what would make you say no to a candidate who passed every other round?"
- "What's something about working here that isn't on the careers page?"
