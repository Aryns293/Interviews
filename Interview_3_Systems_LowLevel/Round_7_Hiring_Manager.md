# Round 7 — Hiring Manager / Bar Raiser
**Interview:** Systems / Low-Level Deep-Dive Company

---

## Resume Defense

**I'll pick this bullet:**
> *"Solved 1,500+ DSA and CP problems"*

**Q1:** "Roughly what fraction were Medium/Hard vs Easy?"

**Q2:** "Tell me about a specific problem — not the hardest you've solved, but one that actually changed how you think about a class of problems. What was it, and what changed?"

*What I want:* A real answer, not a rehearsed one. If you can't name a specific problem that changed your thinking, 1,500 problems is just a number.

**Q3:** "What's a problem you've gotten wrong in a contest, thought about for a week, and then finally understood? What was the gap in your mental model?"

---

## Judgment Scenario 1 — Shipping Broken Code

> "Your manager asks you to ship a merge algorithm you personally know produces wrong results on 5% of inputs. The deadline is tomorrow. What do you do?"

**Strong answer:**
1. Quantify: what is 5% wrong? Is it data corruption, wrong output, crash? The answer changes the severity.
2. Document it in writing — send a Slack message or email explicitly stating the known defect and your concern before shipping
3. Propose a middle path: ship with a kill switch / feature flag that limits rollout to internal users
4. If overruled and it ships anyway: ensure the defect is tracked as a P0 bug, not forgotten

**What fails:**
- "I'd just ship it, my manager said to" — no ownership
- "I'd refuse and escalate over my manager's head immediately" — disproportionate response for a first step

---

## Judgment Scenario 2 — Silent Correctness Bug

> "You find a subtle correctness issue in a system your whole team depends on. Something like a hashing bug that only manifests with Windows-style CRLF line endings on a Unix system. It's not on fire yet — no one's complained. Do you raise it immediately, or fix it quietly?"

**Strong answer:**
- Fix it quietly is tempting but wrong. Silent fixes are not changes anyone can reason about, and they create future debugging nightmares.
- The right move: raise it on Slack/team channel, create a tracked issue with severity, propose a fix in a PR with a clear description and a regression test.
- If it's urgent enough, fix it immediately AND raise it simultaneously — don't wait for approval to fix a correctness bug.

---

## Close

**Q1:** "Systems and infra roles specifically — what draws you to low-level work over general full-stack?"

**Q2:** "What would make you turn down an offer from this team?"

**Q3:** "Questions for me?"
