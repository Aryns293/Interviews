# Round 3 — Past Experience & Project Grilling
**Interview:** Competitive-Programming-Heavy / Quant-Adjacent Company
**Duration:** 45 minutes
**Style:** I am going to ask you to deconstruct your CP rankings and contest performances. I want to see if you actually understand the algorithms you apply, or if you just memorize templates.

---

## Q1 — LeetCode Weekly 504 (Global Rank 892)

> "You placed Global Rank 892 in LeetCode Weekly 504. Walk me through that contest. How many problems did you solve, how much time was left when you submitted your last one, and what almost went wrong?"

*What I'm listening for:*
- Genuine recall of the contest flow. A real competitive programmer remembers their best (and worst) contests vividly.
- Did you get stuck on a bug in Q3/Q4? How did you debug it under pressure? (Print statements, dry running a small test case, identifying uninitialized variables?)

---

## Q2 — Codeforces Specialist (1411)

> "Codeforces Specialist at 1411, Global Rank 1,484 in a Div 2 out of 30,000+. Codeforces is significantly harder than LeetCode. What's the hardest problem in that specific contest you actually solved, and what was your key insight?"

*What I want:*
- I want you to explain a CF Div 2 C or D problem to me.
- I want to hear the math or the combinatorial insight that made it solvable, not just the code implementation. CF problems rarely reduce to standard templates.

---

## Q3 — The 1,500+ Problem Breakdown

> "1,500+ problems solved across platforms. Roughly what fraction are medium/hard vs easy? What's one problem that genuinely changed how you approach a category of problems, not just one you happened to solve?"

*What fails:* "I mostly do mediums, and I liked Two Sum."
*What succeeds:* "About 70% mediums, 20% hards. The problem that changed my perspective was 'Find Minimum in Rotated Sorted Array II' because it forced me to realize that binary search isn't just about sorted arrays, it's about finding any boolean predicate `F, F, F, T, T, T` that splits the search space."

---

## Q4 — CP vs Real-World Engineering

> "Your Fenwick/Segment-tree instincts from CP — did any of that show up in your actual projects? Or is CP a fully separate skill from what you build?"

*What I want to hear:*
- Connections to real systems.
- For example: "In QueueFlow, I used Redis ZSETs for delayed job scheduling. A ZSET uses a Skip List under the hood, which provides O(log N) operations just like a balanced BST or a segment tree, allowing fast range queries by score (timestamp)."
- Or: "In gitlight, doing graph traversals (LCA for merges) felt exactly like CP graph problems."
