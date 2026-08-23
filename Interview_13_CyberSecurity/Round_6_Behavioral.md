# Round 6 — Behavioral (STAR)
**Interview:** Cyber Security / Compliance-Heavy Company

---

## Q1 — Security vs Usability
> "Security often degrades user experience (e.g., forcing 2FA, short session timeouts, complex passwords). Tell me about a time you had to balance a strict security requirement with keeping a tool actually usable."

*Expected:* Discussing Rolewize webhooks. You forced merchants to verify HMAC signatures (annoying UX for them), but you provided a clean SDK/documentation snippet to make it easier, balancing the strict security requirement with developer experience.

---

## Q2 — The Security Incident (or Near Miss)
> "Tell me about a time you accidentally pushed a secret, introduced a vulnerability, or nearly caused a security incident. How did you handle the fallout?"

*Expected:*
- Honesty. Everyone has pushed an AWS key or a DB password to GitHub at some point.
- The correct response: You didn't just delete the commit (which leaves it in the Git history/reflog). You immediately went to the AWS console/DB and rotated the compromised credentials before scrubbing the repo.

---

## Q3 — Pushback on Bad Practices
> "You're at an internship and a senior engineer tells you to just store passwords in plain text for the MVP to save time. How do you respond?"

*Expected:* 
- A hard line. Some things can be compromised for MVP speed (UI polish, scaling architecture). Basic security hygiene (bcrypt hashing) cannot. 
- "I would tell them that implementing `bcrypt.hash()` takes 2 lines of code and 5 minutes, but a plaintext password leak will bankrupt the startup in legal fees. I'd offer to write the auth module myself to unblock them."

---

## Q4 — Keeping Up with Threats
> "How do you stay updated on security vulnerabilities? Where do you read about them?"

*Expected:* Mention specific sources. "I read the OWASP Top 10. I follow HackerOne disclosures. I read the post-mortems of major breaches (like the Cloudflare or GitHub incident reports) because they usually reveal fascinating architectural flaws rather than just simple code bugs."
