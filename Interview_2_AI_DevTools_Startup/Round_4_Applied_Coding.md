# Round 4 — Applied Coding: Backend + Full-Stack
**Interview:** AI / Dev-Tools Startup
**Duration:** 60–90 minutes
**Theme:** Your stack is React + Node.js + Socket.IO + Docker. Problems here are chosen to test real frontend and backend skills, not LeetCode patterns.

---

## Build 1 — Debounced Search Bar with Cancellation (25 minutes)

### The Problem
Build a React search component that:
1. Calls a mock API as the user types, but **debounced** — waits 400ms after the last keystroke
2. Cancels in-flight requests if a new one is triggered before the old one completes (no stale results)
3. Shows a loading state while fetching, and displays results

```jsx
// Mock API (already provided — don't change this)
const searchAPI = (query) =>
  new Promise((resolve) => setTimeout(() => resolve([`Result for: ${query}`]), 600));

// Build this component
function SearchBar() {
  // TODO
}
```

**What I'm watching for:**
- Do you use `AbortController` + `fetch` signal to cancel stale requests? (Or `useRef` to track and cancel a previous promise)
- Is the debounce implemented with `useRef` + `clearTimeout`, or do you try to use `useEffect` cleanup incorrectly?
- Do you show a loading spinner between keystrokes?
- Is there a race condition where a slow request from 3 keystrokes ago overwrites a faster response from the current keystroke?

---

## Debug 1 — Re-render on Every Keystroke (15 minutes)

### The Problem
This component re-renders on every keystroke, even when `searchQuery` hasn't changed. Find the bug and fix it.

```jsx
function Dashboard({ onSearch }) {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');

  const handleSearch = (query) => {
    onSearch(query);
  };

  return (
    <div>
      <input value={name} onChange={(e) => setName(e.target.value)} />
      <button onClick={() => setCount(c => c + 1)}>Increment: {count}</button>
      <SearchResults onSearch={handleSearch} />
    </div>
  );
}
```

**The bug:** `handleSearch` is recreated on every render. `SearchResults` receives a new function reference every time `name` or `count` changes, causing it to re-render even if neither `query` nor the search logic changed.

**Fix:** Wrap `handleSearch` in `useCallback` with `[onSearch]` as the dependency. Then ensure `SearchResults` is wrapped in `React.memo`.

**Follow-up:** When does `useCallback` actually help, and when does it add overhead for no benefit?

---

## Build 2 — JWT Auth Middleware with Refresh Token Rotation (25 minutes)

### The Problem
Build a Node.js Express middleware that:
1. Validates an access token (JWT, short-lived — 15 minutes)
2. If the access token is expired, checks for a refresh token in an HTTP-only cookie
3. If the refresh token is valid, issues a new access token AND rotates the refresh token (old one is invalidated)
4. Attaches the decoded user to `req.user`

```js
// Constants (already defined)
const ACCESS_SECRET = process.env.ACCESS_SECRET;
const REFRESH_SECRET = process.env.REFRESH_SECRET;
const REFRESH_TOKEN_STORE = new Set(); // In real life: Redis SET

// Build this middleware
const authMiddleware = async (req, res, next) => {
  // TODO
};
```

**What I'm watching for:**
- Do you invalidate the old refresh token before issuing the new one? (Rotation, not just generation)
- Do you set the new refresh token in an HTTP-only cookie (not in JSON response body)?
- Do you handle the case where the refresh token itself is expired?
- Token reuse detection: if a refresh token is used twice, does your system detect the replay and revoke the session?

---

## Discussion — Promise.all vs Promise.allSettled (10 minutes)

**Q:** In CodeSync AI, when a user runs code, you try Docker first and fall back to Judge0. You're considering kicking off both in parallel. Which would you use — `Promise.all` or `Promise.allSettled`, and why?

*Expected reasoning:*
- `Promise.all`: rejects immediately if ANY promise rejects. If Docker throws, Judge0's result is discarded even if it succeeded.
- `Promise.allSettled`: waits for ALL promises, returns each result with a status (`"fulfilled"` or `"rejected"`). Lets you check which one succeeded.
- **For a primary/fallback pattern:** Neither is ideal. The correct approach is a sequential try-catch: try Docker with a timeout, if it fails/times out, try Judge0. Racing them in parallel wastes resources on the fallback if the primary succeeds.
- **Nuance worth noting:** `Promise.race()` lets you take whichever resolves first — but if Docker is slow and Judge0 is fast, you'd use Judge0's result while a Docker container is still running (resource leak).
