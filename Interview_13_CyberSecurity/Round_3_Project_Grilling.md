# Round 3 — Past Experience & Project Grilling
**Interview:** Cyber Security / Compliance-Heavy Company
**Duration:** 45 minutes
**Project in focus:** Rolewize & S3 security.

---

## Q1 — S3 Resume Storage (PII)

> "You stored resume files (PII) on S3. If I compromise your AWS account and gain read access to that bucket, can I read the resumes?"

*What I'm listening for:*
- Did you use Server-Side Encryption (SSE-S3 or SSE-KMS)?
- If SSE-S3: Yes, the attacker can read them because AWS transparently decrypts the file if the IAM user has `s3:GetObject` permissions.
- If Client-Side Encryption (KMS): No, unless they also compromise the KMS key policy or extract the key from your application server's memory.
- You must know the difference between encryption at rest (protects against physical disk theft) and encryption in use (protects against IAM compromise).

---

## Q2 — Presigned URL Leakage

> "You generated 15-minute presigned URLs for uploading resumes. What happens if a user copies that URL and posts it on Reddit? Can anyone upload a file to your bucket?"

*Expected:* Yes, anyone with the URL can upload.
*How do you mitigate this?*
- The presigned URL must enforce a strict `Content-Length-Range` to prevent uploading a 10GB file and bankrupting your AWS account.
- The URL must enforce `Content-Type` matching.
- An async worker (e.g., using S3 Event Notifications -> Lambda or SQS) should scan the uploaded file with ClamAV before moving it to a "clean" bucket.

---

## Q3 — Logging and Secrets

> "In QueueFlow, you pass Redis and Postgres connection strings. If an error occurs in the BullMQ worker, does it log the error object? What if that error object contains the Redis connection string (which includes the password)?"

*Expected:*
- Recognizing that standard `console.log(error)` often dumps full connection strings or HTTP request headers (including Authorization tokens).
- Fix: Implement a log sanitizer or a custom logger wrapper (like Pino or Winston) that redacts known secret patterns (e.g., `redis://*:*@`, `Bearer [a-zA-Z0-9]+`) before writing to stdout.
