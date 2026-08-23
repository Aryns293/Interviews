# Round 6 — Behavioral (STAR)
**Interview:** Developer Productivity / Tooling Company

---

## Q1 — Empathy for the Developer
> "Tell me about a time you built a tool or a script specifically to make another developer's life easier, not just to solve a business requirement."

*What I'm listening for:* DevTools companies hire people who love building tools. Did you write a custom bash script for your hackathon team to spin up the local environment? Did you automate a tedious deployment step at Rolewize?

---

## Q2 — Dealing with Edge Cases
> "Developer tools often fail on weird edge cases (e.g., a file with zero bytes, or a project with 10,000 dependencies). Tell me about an edge case in `gitlight` or `CodeSync AI` that completely broke your initial design."

*Expected:*
- `gitlight`: Handling binary files vs text files in the diff algorithm.
- `CodeSync AI`: A user pasting a 10MB text file into the Monaco editor, freezing the WebSocket stream.

---

## Q3 — Open Source & Collaboration
> "Have you ever tried to read the source code of a major open-source project to figure out how it works? Walk me through that experience."

*Expected:* Since you built `gitlight`, I expect you to have looked at either the Git C source code, or the documentation/specifications for Git's internal plumbing. Walk me through the intimidation factor and how you parsed it.

---

## Q4 — The "Dogfooding" Principle
> "Did you ever use `gitlight` or `CodeSync AI` to actually build something else, or were they just portfolio projects? How did using your own tool change your perspective on its bugs?"

*Expected:* "Dogfooding" (using your own product) is a core cultural tenet at companies like Atlassian and GitHub. Honest answers about realizing your own UI was clunky or your own error messages were unhelpful are highly valued here.
