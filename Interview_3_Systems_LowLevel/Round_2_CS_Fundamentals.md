# Round 2 — CS Fundamentals
**Interview:** Systems / Low-Level Deep-Dive Company
**Duration:** 60–75 minutes
**Theme:** OS internals, hashing, compression, Linux, and Git internals — all tied directly to gitlight.

---

## OS — Paging, Segmentation, and Your Object Store

**Q1:** What's the difference between paging and segmentation? What problem does each solve?

**Q2 (project tie-in):**
> "Your gitlight object files are zlib-compressed and stored at content-addressed paths (`.git/objects/ab/cdef...`). When your program reads one of those object files, at what layer does the OS's page cache interact with that operation — if at all?"

*Expected answer:*
- When your Node.js code calls `fs.readFile()`, the OS checks its **page cache** first — a kernel-level cache of recently accessed disk pages
- If the object file is in the page cache, the read is served from RAM — no disk I/O
- If not, the OS reads from disk, populates the page cache, and returns the data
- The zlib decompression happens in your userspace code AFTER the OS has fetched the raw bytes — the OS page cache stores the compressed bytes, not the decompressed content

---

## Hashing — SHA-1 From First Principles

**Q1:**
> "Explain SHA-1 at a level where I believe you actually understand it, not just that you called `crypto.createHash('sha1')`."

*Expected explanation:*
- SHA-1 takes arbitrary-length input, produces a fixed 160-bit (40 hex character) hash
- It processes input in 512-bit blocks, using a series of bitwise operations, modular additions, and non-linear functions across 80 rounds
- The output is deterministic and exhibits the avalanche effect — flipping 1 input bit changes ~50% of output bits

**Q2:**
> "What's a hash collision? SHA-1 has known collision vulnerabilities (SHAttered attack, 2017). Why doesn't a SHA-1 collision in your gitlight object store immediately corrupt your data?"

*Expected:*
- A collision = two different inputs producing the same hash
- In theory: two different file contents could have the same SHA-1 hash → same object store path → one would overwrite the other
- In practice: finding a SHA-1 collision for arbitrary meaningful content is still computationally infeasible for your use case
- Real Git has migrated to SHA-256 for this exact reason. Acknowledging this is a plus.

---

## Compression — zlib Internals

**Q:**
> "Why zlib specifically for object compression? What's the tradeoff between compression ratio and CPU time, and did you tune the compression level?"

*Expected:*
- zlib uses DEFLATE (LZ77 + Huffman coding) — good balance of compression ratio and speed
- Compression level 0–9: 0 = no compression, 9 = maximum compression but slow
- Real Git uses level 1 (fast, decent compression) for performance
- For a personal project, default level (6) is fine, but acknowledging the tradeoff shows depth

---

## Linux — Hard Links vs Content-Addressed Storage

**Q:**
> "What's the difference between a hard link and a soft (symbolic) link? And how does that differ from how Git's object model stores file content — where the filename is completely absent from the storage layer?"

*Expected:*
- Hard link: two directory entries pointing to the same inode (same physical data). Deleting one doesn't delete the data — the inode's reference count drops to 0 only when all hard links are removed.
- Soft link: a pointer to another file path. If the target is deleted, the symlink is broken.
- Git objects: content-addressed — the SHA-1 hash of the content IS the filename. The original filename is stored in the tree object, not the blob. Two files with identical content share one blob object.

---

## Git Internals (Real Git, Not Yours)

**Q1:** What does `git gc` do, and when would it run automatically?

*Expected:* Compacts loose object files into pack files, removes unreachable objects (orphaned commits/blobs), repacks references. Runs automatically when loose objects exceed ~6,700 or when you explicitly call it.

**Q2:** What's the difference between `git merge` and `git rebase`, and what does the commit DAG look like after each?

**Q3:** What is `git reflog`, and how does it let you recover from a `git reset --hard`?

*Expected:* reflog tracks every movement of HEAD, including resets. Even after `reset --hard`, the old commit SHA is in reflog for 90 days — you can `git checkout <sha>` to recover it.
