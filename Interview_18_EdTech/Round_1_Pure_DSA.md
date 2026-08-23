# Round 1 — Pure DSA
**Interview:** EdTech / Learning Platform Company
**Duration:** 60 minutes

---

## Problem 1 — Task Scheduler (Practice Scheduling)
**Difficulty:** Medium

**The Problem:**
Task Scheduler — reframed as scheduling a student's practice problems with a cooldown so the same topic doesn't repeat too soon.

---

## Problem 2 — Design Twitter (Student Feed)
**Difficulty:** Hard

**The Problem:**
Design Twitter (post, follow, top-10 recent-post feed) — reframed as a student's feed of the 10 most recent updates from courses/mentors they follow, heap-merge of per-source sorted lists.

**Mid-solve twist:**
> "A mentor can post to 100,000 followers — does your per-user feed generation still work, or do you need fan-out-on-write vs fan-out-on-read?" 

*Directly tests the classic feed-scaling tradeoff.*

**Scored on:**
- Recognizing the fan-out problem unprompted.
- Brute force stated first.
- Edge case of a user following no one.
