# Round 0 — Online Assessment
**Interview:** Developer Productivity / Tooling Company (e.g., Atlassian, GitHub, GitLab, Vercel)
**Format:** Auto-graded — 90 minutes. Emphasizes string manipulation, parsing, and deep Git/OS concepts.

---

## DSA Problems

### Problem 1 — Parse Lisp Expression (Recursion)
**Difficulty:** Hard

You are given a string expression representing a Lisp-like expression to return the integer value of. The syntax for expressions is given as follows: An integer, a let expression, an add expression, or a mult expression.

---

### Problem 2 — Basic Calculator (Stack)
**Difficulty:** Hard

Given a string s representing a valid expression, implement a basic calculator to evaluate it, and return the result of the evaluation. The expression may contain parentheses, plus, minus, and empty spaces.

---

## MCQs

### Git Internals
**Q:** In Git, what actually happens when you run `git add file.txt`?
**A:** Git compresses the file's contents, generates a SHA-1 hash of the content, stores it as a blob in the `.git/objects` directory, and updates the index (staging area) to point to this new blob. (This is exactly what your `gitlight` project does).

### Linux File Systems
**Q:** What is an inode?
**A:** A data structure that stores metadata about a file (permissions, owner, size, timestamps, disk block locations), but NOT the file's name or its actual data.

### AST / Parsing
**Q:** Which data structure is most commonly used to represent parsed source code before execution?
**A:** N-ary Tree (Abstract Syntax Tree).
