# Round 3 — Past Experience & Project Grilling
**Interview:** HealthTech / Compliance-Heavy Company
**Duration:** 45 minutes

---

## Q1
"Your Rolewize pipeline does AI resume parsing with PII extraction — if this were patient intake data instead, what changes about your MIME validation and pre-signed URL approach? Is 15 minutes still the right TTL for far more sensitive files?"

## Q2
"Redis caching with SHA-256 hashing eliminates duplicate LLM calls — is caching any derived output of PII a retention risk, even with a hashed cache key? What would you need to know before answering confidently?"

## Q3
"If a webhook event contains PII and gets retried 3 times, does your system log the payload each time, tripling the sensitive-data footprint in logs? Is that something you'd need to explicitly design around?"

## Q4
"What's the actual difference between designing this for 'move fast' versus 'must survive a compliance audit' — name three concrete changes."
