# Round 1 — Pure DSA
**Interview:** B2B SaaS / Enterprise Software
**Duration:** 60 minutes
**Interviewer's Note:** SaaS code needs to be readable and bug-free. I care less about you knowing Fenwick trees, and more about how cleanly you write business-logic-heavy algorithms.

---

## Problem 1 — Design Hit Counter (Rate Limiting)
**Difficulty:** Medium
**Time Budget:** 25 minutes

### The Problem
Design a hit counter. `hit(timestamp)` records a hit. `getHits(timestamp)` returns the number of hits in the last 300 seconds.

**What I want to see:**
- You should quickly arrive at the Array of Buckets approach.
- Two arrays: `times[300]` and `hits[300]`.
- Clean modulo arithmetic: `index = timestamp % 300`.
- Handling the time difference correctly in `getHits` (only sum the buckets where `timestamp - times[i] < 300`).

---

## Problem 2 — Basic Calculator II (Parsing API Filters)
**Difficulty:** Medium
**Time Budget:** 35 minutes

### The Problem
Given a string `s` which represents an expression, evaluate this expression and return its value. The integer division should truncate toward zero. Contains `+`, `-`, `*`, `/`. No parentheses.

**SaaS framing:** "Users send us custom billing filter expressions as strings. We need to evaluate them safely without using `eval()`."

**What I want to see:**
- Use a Stack.
- Keep track of the *previous* operator (initialized to `+`).
- Iterate through string. Build up the current number.
- When you hit an operator (or end of string):
  - If prev op was `+`, push `num`.
  - If prev op was `-`, push `-num`.
  - If prev op was `*`, pop, multiply by `num`, push.
  - If prev op was `/`, pop, divide by `num` (using `Math.trunc`), push.
- Finally, sum the stack.
