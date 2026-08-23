# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** Cloud Infrastructure / Platform Engineering Company
**Duration:** 60–90 minutes

---

## Build 1 — /health Endpoint + Graceful Shutdown Handler (25 minutes)

### The Problem
Build a `/health` endpoint that checks real dependencies, and a SIGTERM handler that finishes in-flight requests before the process exits.

```js
const express = require('express');
const app = express();

// Build this — check all real dependencies
app.get('/health', async (req, res) => {
  // TODO: check DB connection, Redis, Docker daemon availability
  // Return 200 with { status: 'ok', checks: {...} } if all healthy
  // Return 503 with failed checks if any dependency is unhealthy
});

// Build this — SIGTERM handler for graceful shutdown
process.on('SIGTERM', () => {
  console.log('SIGTERM received. Finishing in-flight requests...');
  server.close(() => {
    // TODO: close DB pool, Redis client, drain active connections
    process.exit(0);
  });
});

const server = app.listen(3000);
```

**What I'm watching for:**
- Does the health check actually test the dependency (e.g., `await db.query('SELECT 1')`) or just check if the client object exists?
- Does `server.close()` stop accepting new connections while finishing existing ones? (Yes — that's exactly what it does. If you didn't know this, look it up.)
- Is there a timeout on graceful shutdown? (If in-flight requests take more than 30 seconds, do you force-exit anyway?)

---

## Debug 1 — Memory Leak in a Long-Running Worker (20 minutes)

### The Problem
This Node.js worker process grows in memory over time until it OOMs. Find the leak.

```js
const EventEmitter = require('events');
const jobEmitter = new EventEmitter();

async function processJob(jobId) {
  return new Promise((resolve) => {
    const handler = (result) => {
      console.log(`Job ${jobId} result:`, result);
      resolve(result);
    };
    jobEmitter.on(`job:${jobId}:done`, handler);
    // BUG: handler is never removed after the job completes
    // Every job adds one listener — after 10,000 jobs, 10,000 listeners
  });
}

// Somewhere else in the codebase
jobEmitter.emit(`job:${jobId}:done`, { status: 'ok' });
```

**The leak:** Every call to `processJob` adds a new listener on `jobEmitter` but never removes it. After the Promise resolves, the listener stays registered forever. After 10,000 jobs, 10,000 orphaned listeners hold references to their closures, preventing GC.

**Fix:**
```js
const handler = (result) => {
  jobEmitter.off(`job:${jobId}:done`, handler); // Remove before resolving
  resolve(result);
};
```

**Follow-up:** How would you detect this in production before it causes an OOM? (Node.js `process.memoryUsage()` in a metrics endpoint, `EventEmitter.listenerCount()` monitoring, or a heap snapshot with `--inspect`.)

---

## Discussion — Adding a QueueFlow Worker Node With Zero Downtime (15 minutes)

> "Walk me through the exact sequence of steps to add a new QueueFlow worker node without dropping any jobs or causing any downtime."

*Expected sequence:*
1. Provision the new worker instance
2. Ensure it has access to the same Redis and Postgres credentials
3. Start the new worker process — it will immediately begin consuming jobs via `BRPOP`
4. No special coordination needed: BullMQ's job locking ensures only one worker processes each job
5. Monitor: check that the new worker is processing jobs (not silently failing)
6. Scale down old workers one at a time (send SIGTERM, wait for in-progress jobs to complete, then exit)

**What I'm listening for:**
- You understand that BullMQ's atomic job-locking prevents double-processing during the scale-up overlap
- You don't suggest "pause the queue" during the transition — that causes dropped jobs
- You mention monitoring after the new node comes up, not just deployment

---

## Write It Out — Multi-Stage Dockerfile (15 minutes)

### The Problem
Rewrite this bloated single-stage Dockerfile as a multi-stage build:

```dockerfile
# BLOATED - everything in one stage
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install           # installs devDependencies too
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

**Expected multi-stage:**
```dockerfile
# Stage 1: Build
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci                        # deterministic install
COPY . .
RUN npm run build

# Stage 2: Runtime
FROM node:18-alpine AS runtime
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
# OR: only copy production node_modules
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

**What I'm watching for:**
- `node:18-alpine` instead of `node:18` (alpine = ~50MB vs ~900MB base image)
- Production-only `node_modules` in the runtime stage (don't copy `devDependencies`)
- Layer caching order: `COPY package*.json` before `COPY .` so the npm install layer is cached when only source files change
- `npm ci` instead of `npm install` (deterministic, fails if `package-lock.json` is out of sync)
