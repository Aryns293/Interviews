# Round 0 — Online Assessment
**Interview:** Cyber Security / Compliance-Heavy Company (e.g., Okta, CrowdStrike, Banks)
**Format:** 90 minutes. Emphasizes string hashing, cryptography, and secure coding practices.

---

## DSA Problems

### Problem 1 — String Hashing / Rabin-Karp
**Difficulty:** Medium-Hard

Implement the Rabin-Karp string matching algorithm to find all occurrences of a pattern in a text.

**Security framing:** High-speed malware signature scanning (finding a specific byte sequence in a large binary).

**What I'm testing:**
- Do you understand rolling hashes?
- Math: `hash(s[i+1...j+1]) = (hash(s[i...j]) - s[i] * p^(L-1)) * p + s[j+1]`.
- Handling modulo arithmetic to prevent integer overflow.
- Handling hash collisions (you must do a full string comparison if hashes match).

---

### Problem 2 — Valid Parentheses
**Difficulty:** Easy (but with a twist)

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

**Security framing:** Parsing user-submitted JSON to ensure there are no malformed nesting attacks (XML/JSON bombs) before passing to the real parser.

**What I'm testing:**
- Flawless stack implementation.
- *The Security Twist:* What if the string is 10 GB long? (Stream processing, bounded stack size to prevent OOM/DoS attacks).

---

## MCQs

### Cryptography
**Q:** What is the difference between symmetric and asymmetric encryption? Where did you use them in your projects?
**A:** Symmetric uses the same key for encryption and decryption (AES). Asymmetric uses a public/private key pair (RSA/ECC). Rolewize HMAC is symmetric (shared secret). HTTPS/TLS uses asymmetric to exchange a symmetric session key.

### Web Security (OWASP)
**Q:** What is CSRF (Cross-Site Request Forgery) and how do you prevent it?
**A:** An attacker tricks a user's browser into executing an unwanted action on a trusted site where they are authenticated. Prevented by using anti-CSRF tokens (unique, unpredictable tokens validated on the server) and `SameSite` cookie attributes.
