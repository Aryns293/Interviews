# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** Remote-First / Fully Distributed Team Company
**Duration:** 60 minutes

---

## Build 1 — Async-Aware Webhook Consumer (30 minutes)

### The Problem
Build a webhook retry consumer that assumes the sender is in a different timezone/region and may retry hours later.

**What I'm watching for:**
- Correct implementation of idempotency.
- **The "Async" test:** I want to see you write a code comment explaining your assumption about the retry window. "In an async environment, the person reading this code might be asleep. Leave a trail."
- Example: `// Assumes upstream retries up to 24h. We keep idempotency keys for 48h to be safe.`

---

## Debug 1 — React Stale Closure (Solo Debugging) (15 minutes)

### The Problem
```jsx
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1); // BUG: count is always 0 here
    }, 1000);
    return () => clearInterval(id);
  }, []);

  return <div>{count}</div>;
}
```

**The Twist:** "You are debugging this alone, no one else is online. Write down your reasoning as you go, out loud, as if leaving a trail in a GitHub issue."

**Expected "Trail":**
1. "The interval is firing, but `count` isn't incrementing."
2. "Oh, the dependency array is `[]`. The closure captures `count` from the initial render, which is `0`."
3. "Fix options: add `count` to the dependency array (but that resets the interval every render), OR use the functional updater: `setCount(prev => prev + 1)`."

---

## Discussion — Promise.all vs Promise.allSettled (10 minutes)

> "You're kicking off calls to 3 microservices owned by teams in different timezones who might be mid-deploy. Do you use `Promise.all` or `Promise.allSettled`?"

*Expected:* `Promise.allSettled`. If Team B is mid-deploy and returns a 503, `Promise.all` fails immediately and drops the successful responses from Team A and Team C. `Promise.allSettled` lets you gracefully degrade: use the data from A and C, and show a fallback UI for B, rather than blowing up the whole page.
