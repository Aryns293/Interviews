# Round 7 — Hiring Manager / Bar Raiser
**Interview:** Fintech / Payments Company
**Duration:** 45–60 minutes

> **Fintech bar:** This round is harder than a general product company because the stakes of a wrong answer in production are higher. A security gap in fintech is not a UX bug — it's a regulatory incident, a customer harm event, or a fraud vector.

---

## Resume Defense — The Presigned URL Number, Pushed Harder

> "You said 15 minutes for the presigned URL TTL. I don't believe your first answer was fully thought through. Let me push: what actually goes wrong at **5 minutes**? What actually goes wrong at **60 minutes**? And what would you choose if this were a PCI-DSS compliant fintech system?"

*Expected:*
- **5 minutes:** On a slow mobile connection, a large resume file upload could take 3–4 minutes. Add network hiccups and a slow API call to generate the URL, and 5 minutes is genuinely too short. Users on 3G or with large files would get `RequestExpired` errors from S3, creating a poor UX and extra retries.
- **60 minutes:** An intercepted URL (via a log aggregation system, a browser extension, or an analytics script that captures URL parameters) gives an attacker a full hour to upload arbitrary content to your S3 key. In fintech, logs are often retained — a URL in a log file is effectively as long-lived as the log retention policy.
- **PCI-DSS context:** PCI-DSS doesn't specify a presigned URL TTL, but the principle of least privilege applies — use the shortest TTL that doesn't break the user flow. 15 minutes is a defensible industry standard.

---

## Judgment Scenario — Data Exposure Race Condition

> "You're reviewing your codebase and you find a race condition. Under a specific timing window — two concurrent requests hitting the same endpoint within ~50ms — one user could briefly see another user's cached resume analysis result. It hasn't been exploited as far as you can tell. What do you do in the **next hour**, the **next day**, and **who do you tell**?"

**Next hour:**
- Do NOT downplay or ignore it because "it probably hasn't been exploited"
- Assess exploitability: what are the exact conditions? Is this a theoretical race or can you reproduce it?
- If reproducible: immediately deploy a hotfix or roll back the affected feature if a clean fix isn't ready in 30 minutes
- Tell your manager NOW — even if you're not sure how bad it is. The worst outcome in fintech is discovering you knew about a vulnerability and didn't escalate.

**Next day:**
- Root cause analysis: why did the cache key not include user context?
- Fix: ensure every cache key is scoped to the user (include `user_id` or a user-derived hash in the key)
- Write a regression test that would catch this class of bug
- Check logs for any evidence of exploitation (though absence of evidence ≠ evidence of absence)

**Who you tell:**
- Your manager immediately
- Security team if one exists
- If user PII was potentially exposed even briefly: legal and compliance must be informed — depending on jurisdiction (GDPR Article 33: 72-hour breach notification requirement)

---

## Close — Fintech-Specific Interest

**Q1:** "What draws you to fintech and security-adjacent engineering specifically, over a general product company?"

**Q2:** "If you joined this team, what's the first security-adjacent thing you'd audit in our codebase — not because we've asked, but because your instincts from Rolewize would make you nervous until you'd checked it?"

**Q3:** "What would make you turn down an offer from us, assuming compensation is fair?"
