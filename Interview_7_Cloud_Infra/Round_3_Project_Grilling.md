# Round 3 — Past Experience & Project Grilling
**Interview:** Cloud Infrastructure / Platform Engineering Company
**Project in focus:** CodeSync AI — Docker execution sandbox, Judge0 fallback, Render hosting, SELF_PING workaround.

> **Interviewer's mindset:** You built a container-based code execution system. That's a genuinely hard problem — sandboxing untrusted code is what companies like Replit and Leetcode spend years getting right. I'm checking whether you understand what you built, where it's weak, and how you'd harden it at scale.

---

## Q1 — Docker Image Architecture

> "Walk me through your Dockerfile for the execution image. You included g++, python3, and JDK all in one image. What's the actual image size cost of that decision, and would you change the architecture at scale?"

*What I want:*
- Rough awareness of image sizes: `python3` slim ≈ 120MB, `openjdk` slim ≈ 200MB, `g++` ≈ 200MB. Combined in one image: 400–500MB+.
- At scale, a 500MB image pulled to every execution node is a significant cold-start latency and storage cost.
- **Better architecture:** Per-language images — `exec-python:latest` (120MB), `exec-cpp:latest` (150MB), `exec-java:latest` (200MB). Pre-warm the most commonly used ones. Route each execution request to the appropriate image.
- Trade-off: more images to maintain and version-bump, but much smaller per pull and faster spin-up.

---

## Q2 — Resource Limits: Empirical or Estimated?

> "What are your actual CPU, memory, and process-count limits on the sandbox container? Did you run load tests to arrive at those numbers, or did you estimate them?"

*I'm looking for honesty above all.* Options:
- **Empirical:** "I ran stress tests with intentional memory bombs and fork bombs, measured the point at which the host became unstable, and set limits to 50% of that threshold."
- **Estimated:** "I used common defaults — 256MB memory, 1 CPU, 50 processes — based on what similar open-source sandboxes use. I haven't formally tested the upper bounds."
- **Neither:** If you don't remember your exact values, say so. Don't invent a number.

---

## Q3 — The SELF_PING Workaround

> "You built a SELF_PING_URL mechanism to stop your free-tier hosting from sleeping. That's a workaround for a platform limitation, not a real solution. If this were a production deployment serving real users, what would you replace it with, and what would that cost?"

*Expected:*
- **Render free tier** sleeps instances after 15 minutes of inactivity. SELF_PING is a cron job that pings itself to prevent sleep.
- **Production alternative:** A paid always-on instance (Render Starter plan ≈ $7/month), AWS EC2 t3.micro + Auto Scaling Group, or Kubernetes with a minimum replica count of 1.
- **The real cost:** SELF_PING adds unnecessary traffic and burns resources keeping a server warm even when no users are active. At scale, you'd use auto-scaling with pre-warming: scale to 0 at night, scale up proactively at predicted peak hours.
- **What I want to hear:** Awareness that SELF_PING is a hack, a clear alternative, and a rough cost estimate.

---

## Q4 — Docker Fallback Trigger Logic

> "When does execution fall back from Docker to Judge0? Is that check done once at server startup, or per-request? What if Docker is healthy but a specific container is slow — do you have a timeout-based per-request fallback?"

*Expected:*
- Health-check at startup: simplest. Mark Docker as `available` or `unavailable` at boot. Doesn't handle mid-session Docker degradation.
- Per-request timeout: more robust. Each execution request has a `dockerTimeoutMs`. If Docker doesn't respond within that window, fall back to Judge0 for that specific request.
- Hybrid: health-check every 30 seconds (background), plus per-request 8-second timeout.
- Which did you actually build? Be precise.

---

## Q5 — Render vs Other Hosting

> "Why Render specifically for CodeSync AI, over Fly.io, Railway, Vercel, or a simple EC2 instance?"

*What I want:* A real answer, not "I just knew Render." Options:
- Render's free tier supports persistent services (unlike Vercel which only supports serverless functions — you can't run a long-lived Socket.IO connection on Vercel)
- Docker support out of the box (Railway and Fly.io also do this)
- Simple GitHub integration and auto-deploy
- Honest acknowledgment: if you chose Render because of familiarity or a tutorial, say so — that's fine, but own it
