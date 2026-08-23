# Round 2 — CS Fundamentals
**Interview:** HealthTech / Compliance-Heavy Company
**Duration:** 45 minutes

---

## Security (Heavy)
- **Encryption at rest:** Was your Rolewize S3 bucket encryption default or explicitly configured, and do you know the difference in who controls the keys?
- Field-level vs whole-record encryption when some fields need to stay searchable.

---

## DBMS
- **Audit trail design:** Every read of sensitive data logged, not just writes.
- ACID with a concrete partial-write-leaves-inconsistent-record example.

---

## Computer Networks
- Why HTTPS alone isn't sufficient for regulatory compliance around PII in transit.

---

## Operating Systems
- `chmod` on a server storing sensitive intake documents — least-privilege reasoning.

---

## Access Control
- Role-based field-level access (a nurse sees vitals, a billing clerk sees insurance info) — how is this actually enforced, not just documented as policy?
