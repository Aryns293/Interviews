# Round 1 — Pure DSA
**Interview:** Cyber Security / Compliance-Heavy Company
**Duration:** 60 minutes
**Interviewer's Note:** Security engineering DSA focuses heavily on correctness, bounds checking, and handling malicious inputs safely.

---

## Problem 1 — Implement `strstr` (Needle in Haystack)
**Difficulty:** Medium
**Time Budget:** 25 minutes

### The Problem
Implement `strstr()`. Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

**The Security Twist:** 
> "The haystack is a 2GB network packet stream. The needle is a 10-byte malware signature. O(N*M) brute force is too slow and will cause a Denial of Service (DoS) timeout. Implement it in O(N)."

**What I want to see:**
- You MUST implement either KMP (Knuth-Morris-Pratt) or Rabin-Karp.
- KMP is preferred because it avoids hash collisions entirely. You need to build the Longest Prefix Suffix (LPS) array (`pi` array).
- If you write O(N*M) naive string matching, you fail this round.

---

## Problem 2 — Integer to English Words
**Difficulty:** Hard
**Time Budget:** 35 minutes

### The Problem
Convert a non-negative integer `num` to its English words representation. (e.g., `123` -> "One Hundred Twenty Three").

**Security framing:** Parsing numbers safely without integer overflow or injection vulnerabilities.

**What I want to see:**
- Impeccable handling of edge cases (0, millions, billions).
- Clean code architecture (arrays for `LESS_THAN_20`, `TENS`, `THOUSANDS`).
- *The real test:* If I pass you a string instead of an int (e.g., `"12345678901234567890"`), do you blindly parse it using `parseInt()` and suffer precision loss/overflow, or do you validate the input boundaries first? You must check boundaries before parsing.
