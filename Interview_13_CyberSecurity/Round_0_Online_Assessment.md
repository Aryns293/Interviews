# Round 0 — Online Assessment
**Interview:** Cyber Security / Compliance-Heavy Company (e.g., Okta, CrowdStrike, Banks)
**Format:** 90 minutes. Emphasizes string hashing, cryptography, and secure coding practices.

---

## DSA Problems

### Problem 1 — Distinct Subsequences (DP)
**Difficulty:** Hard

Given two strings s and t, return the number of distinct subsequences of s which equals t.

---

### Problem 2 — Cracking the Safe (Eulerian Path)
**Difficulty:** Hard

There is a safe protected by a password. The password is a sequence of n digits where each digit can be in the range [0, k - 1]. The safe has a peculiar way of checking the password. When you enter a sequence, it checks the most recent n digits that were entered each time you type a digit. Return any string of minimum length that will unlock the safe guaranteed.

---

## MCQs

### Cryptography
**Q:** What is the difference between symmetric and asymmetric encryption? Where did you use them in your projects?
**A:** Symmetric uses the same key for encryption and decryption (AES). Asymmetric uses a public/private key pair (RSA/ECC). Rolewize HMAC is symmetric (shared secret). HTTPS/TLS uses asymmetric to exchange a symmetric session key.

### Web Security (OWASP)
**Q:** What is CSRF (Cross-Site Request Forgery) and how do you prevent it?
**A:** An attacker tricks a user's browser into executing an unwanted action on a trusted site where they are authenticated. Prevented by using anti-CSRF tokens (unique, unpredictable tokens validated on the server) and `SameSite` cookie attributes.
