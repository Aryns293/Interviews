# Round 7 — Hiring Manager / Bar Raiser
**Interview:** Cloud Infrastructure / Platform Engineering Company

---

## Resume Defense — Exact Sandbox Limits, Pushed Hard

> "You've told me about your sandbox resource limits. Tell me: the CPU limit — what's the actual number, in what unit (shares, millicores, percentage?), and what happens to a process that hits it — is it throttled or killed?"

*Expected precision:*
- Docker CPU limits can be expressed as: `--cpus=0.5` (0.5 cores), `--cpu-shares=512` (relative weight), or `--cpu-quota + --cpu-period` (fine-grained)
- **Throttled, not killed:** CPU limits throttle the cgroup — the process doesn't die, it just gets less CPU time. Contrast with memory limits, where exceeding the limit triggers OOM Kill.
- If you set a memory limit and a process exceeds it: the OOM Killer sends SIGKILL. The container exits.

**Follow-up:** "What if the container is CPU-throttled but the user's code has a 5-second execution timeout? Does the timeout measure wall-clock time or CPU time? If it's wall-clock, a throttled container could legitimately take 10x longer than expected."

---

## Judgment Scenario — Live Demo Disaster

> "Your host is about to put your server to sleep in 5 minutes. There's a live client demo starting in exactly 5 minutes. SELF_PING is down. What do you do in those 5 minutes, and what do you fix the next morning?"

**Next 5 minutes:**
- Immediately trigger a warm-up request to the server to prevent sleep (manual ping)
- If the server is already asleep: log in to the host dashboard and restart the instance
- Open the demo in an incognito tab and verify the server is responding before the client joins
- Have the Judge0 fallback ready as backup for code execution if Docker is slow after cold start

**Next morning:**
- Diagnose why SELF_PING was down — was the cron job misconfigured, or did the free tier itself have an outage?
- Upgrade to a paid always-on tier for anything that has a real client or demo dependency
- Add a synthetic monitoring service (UptimeRobot free tier is sufficient) that pings your server every 5 minutes and alerts you if it goes down

---

## Close

**Q1:** "Platform and infra work — why does that appeal to you over building product features?"

**Q2:** "What's the first thing you'd want to learn or build in an infra/platform role that you haven't had a chance to do in your projects yet?"

**Q3:** "Questions for me?"

*Strong question:*
> "What does the on-call rotation look like for a new engineer joining this team — are you expected to be on-call in the first month, or is there a ramp-up period?"
