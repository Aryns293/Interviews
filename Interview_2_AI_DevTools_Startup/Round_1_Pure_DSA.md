# Round 1 — Pure DSA
**Interview:** AI / Dev-Tools Startup
**Duration:** 60 minutes
**Format:** Live coding, shared editor. No autocomplete. I know your project (CodeSync AI) is about real-time systems. My problem choices are not random.

---

## Problem 1 — Group Anagrams
**Difficulty:** Medium
**Time Budget:** 15 minutes

### The Problem
Given an array of strings, group them so that all anagrams appear together.

**Before you code — tell me:**
- What makes two strings anagrams of each other?
- What's the simplest way to produce a canonical key for a group? (Sort the characters)
- What's a faster way? (Character frequency array → tuple)

**What I'm grading:**
- Clean hash map usage
- Correct handling of empty string `""` (it's its own group)
- Time complexity statement: O(n * k log k)

---

## Problem 2 — Word Break II
**Difficulty:** Hard
**Time Budget:** 30 minutes

### The Problem
Given a string `s` and a dictionary, return **all** valid segmentations of `s` using words from the dictionary.

```
Input:  s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
Output: ["cats and dog", "cat sand dog"]
```

**Before you code — walk me through your approach:**
- This isn't the same as Word Break I (yes/no). You need all paths.
- How do you avoid recomputing from the same index? (Memoization — cache `index → [list of valid sentences from that index]`)
- What's the worst case? (`"aaa...a"` with dictionary `["a","aa","aaa"...]`) — exponential output, unavoidable.

**What I'm grading:**
| Criteria | Weight |
|---|---|
| Recognized exponential blowup *before* being told | High |
| Added memoization correctly (caching list of sentences, not just bool) | High |
| Handled the empty string base case | Medium |
| Handled no-valid-segmentation case (return `[]`, not crash) | Medium |

---

## Mid-Solve Twist — The Differentiator

*I will say this after you've solved Problem 2:*

> "Good. Now the dictionary isn't static — it's being streamed in. New words are being added while queries for valid segmentations are arriving. How does your solution need to change?"

**What this tests:**
- Static solution: build a set from the dictionary, done.
- Streaming solution: the dictionary grows. Your memoization cache is now stale every time a word is added, because a previously invalid segmentation might now be valid.
- Optimal direction: **Trie** — insertions are O(k), lookups are O(k). The Trie itself acts as the dictionary, and you can incrementally add words without rebuilding.

**What I want to hear:**
1. Recognize that your memo cache must be invalidated (or you switch to a non-memoized approach with a live Trie)
2. Propose the Trie as the dictionary structure — even if you can't implement it fully in time
3. Articulate the tradeoff: Trie lookup is O(k) per prefix check, which is same asymptotic cost as hash set lookup but with better prefix-matching capabilities

**What fails:**
- "I'd just re-run the whole thing with the new dictionary" — O(n * k) per new word, ignores the stream nature
- Silence — if you don't know, say "I haven't thought about this variant, let me reason through it"
