# Round 0 — Online Assessment
**Interview:** Developer Productivity / Tooling Company (e.g., Atlassian, GitHub, GitLab, Vercel)
**Format:** Auto-graded — 90 minutes. Emphasizes string manipulation, parsing, and deep Git/OS concepts.

---

## DSA Problems

### Problem 1 — Evaluate Reverse Polish Notation
**Difficulty:** Medium

Evaluate the value of an arithmetic expression in Reverse Polish Notation (e.g., `["2","1","+","3","*"]` -> 9).

**Tooling framing:** This is the core logic behind AST (Abstract Syntax Tree) traversal and compiler design, which is highly relevant to CodeSync AI's real-time execution environment.

**What I'm testing:**
- Do you immediately use a Stack?
- Proper handling of integer division (truncating toward zero).
- Code cleanliness: a simple switch statement vs messy if/else chains.

---

### Problem 2 — Implement Trie (Prefix Tree)
**Difficulty:** Medium

Implement a Trie with `insert`, `search`, and `startsWith` methods.

**Tooling framing:** Autocomplete for an IDE (CodeSync AI) or fast path-matching for a Git clone (gitlight).

**What I'm testing:**
- Clean Node definition (using a Map or fixed size array of 26 characters).
- Efficient iteration over the string.
- Marking the `isEndOfWord` flag correctly.

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
