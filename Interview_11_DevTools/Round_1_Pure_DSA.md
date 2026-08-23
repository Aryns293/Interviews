# Round 1 — Pure DSA
**Interview:** Developer Productivity / Tooling Company
**Duration:** 60 minutes
**Interviewer's Note:** I want to see if you can manipulate strings and trees flawlessly, because developer tools are ultimately just massive text and AST processing engines.

---

## Problem 1 — Longest Absolute File Path
**Difficulty:** Medium
**Time Budget:** 25 minutes

### The Problem
Suppose we have a file system represented as a string: `"dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tfile2.ext"`.
Find the length of the longest absolute path to a file in the abstracted file system.

**Before you code:**
- How do you parse the depth? (Count the `\t` characters).
- What data structure makes this easy? (A Hash Map storing `depth -> current_path_length`, or a Stack storing the lengths).

**What I'm grading:**
| Criteria | Weight |
|---|---|
| Correctly splitting by `\n` and counting `\t` | High |
| Realizing you only update the max length if the string contains a `.` (it's a file, not a directory) | High |
| Avoiding building the actual string path, and just keeping integer lengths | Medium |

---

## Problem 2 — Implement a Basic Calculator (AST parsing light)
**Difficulty:** Hard
**Time Budget:** 35 minutes

### The Problem
Given a string `s` representing a valid expression, implement a basic calculator to evaluate it. The string may contain digits, `+`, `-`, `(`, `)`, and spaces.
Example: `"(1+(4+5+2)-3)+(6+8)"`

**The expected approach:**
- Use a Stack to keep track of intermediate results and the current sign.
- When you see a `(`, push the current result and the current sign onto the stack, then reset result and sign.
- When you see a `)`, pop the sign, multiply it by the current result, then pop the previous result and add it.

**Mid-Solve Twist:**
> "Now I want to support variables. E.g., `let x = 5; (x + 3)`. How does your architecture change?"

*Expected insight:* "The stack-based one-pass evaluator breaks down here. I need to formally separate parsing from evaluation. I would tokenize the string, build an Abstract Syntax Tree (AST), and then write an `evaluate(node, environment)` function where `environment` is a hash map of variable names to values."
