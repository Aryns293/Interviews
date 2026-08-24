# Round 1 — Pure DSA
**Interview:** E-commerce / Marketplace Company
**Duration:** 60 minutes

---

## Problem 1 — Product of Array Except Self
**Difficulty:** Medium

**The Problem:**
Reframed as computing a product's final price after all other active discounts stack, without division.

**What I'm grading:**
- Do you use prefix and suffix arrays to compute the products before and after the current index?
- Can you optimize it to O(1) space (excluding the output array) by calculating the suffix product on the fly?

---

## Problem 2 — Longest Consecutive Sequence
**Difficulty:** Hard

**The Problem:**
Given a stream of scanned warehouse SKU IDs, find the longest consecutive run currently in stock, **no sorting allowed** (O(n) required).

**Mid-solve twist:** 
> "SKUs are being added and removed from stock in real time while you're computing this."

*Forces incremental tracking instead of one static pass.*

**Scored on:**
- Recognizing why sorting breaks the O(n) requirement.
- Brute force stated first.
- Using a HashSet to find the start of a sequence (where `num - 1` is not in the set), then counting up.
- Edge cases: empty stock, all consecutive.
