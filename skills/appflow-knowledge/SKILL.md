---
name: appflow-knowledge
description: Guidance for producing the PlanBoard application-flow document.
---

# AppFlow Knowledge

What makes a good AppFlow.md. The appflow-agent loads this before writing.
Only populated if the project has a frontend — backend-only projects skip this file.

---

## What AppFlow.md is

Maps the paths users take through the app to complete goals: screen-to-screen navigation, state transitions, decision points, entry and exit points.
Not a user emotional journey map. Not a wireframe. A state machine described in text + diagrams.

---

## Required sections and depth

| Section | What it must contain |
|---|---|
| Primary User Flows | One flow per major user goal. Each has: entry point, numbered steps, exit point. |
| Flow Diagrams | A Mermaid `flowchart TD` block for each primary flow. Nodes = screens/states, edges = transitions/triggers. |
| Edge Cases in Flow | For each flow: what happens on auth failure, network error, validation error, session expiry. |
| Navigation Map | Hierarchical list of all screens/views and how they connect. |

---

## Primary flow format

```
### Flow: [Goal name — e.g. "User Sign-Up"]
**Entry point:** [screen or trigger]
1. [Step — screen name + action]
2. [Step]
3. ...
**Exit point:** [terminal state — success screen, redirect, etc.]
```

---

## Mermaid diagram format

```
flowchart TD
  A([Entry: Landing Page]) --> B{Logged in?}
  B -- Yes --> C[Dashboard]
  B -- No --> D[Login Screen]
  D -- Success --> C
  D -- Forgot password --> E[Password Reset Flow]
  D -- Error --> F([Error: Invalid credentials])
```

Rules for diagrams:
- Diamond shapes `{}` for decision points only
- Circle caps `([...])` for entry and exit states
- Rectangle `[...]` for normal screens/states
- Every branch of a decision must be labeled
- Error and failure paths must be shown — not just happy path

---

## Dos

- One flow per user goal — don't combine checkout and onboarding in one diagram
- Single entry point per flow — if multiple entries exist, split into separate flows
- Map ALL decision points as diamonds — binary branches make ambiguity visible
- Include empty states and loading states — common design gaps that cause dev confusion
- Every screen in the Navigation Map must appear in at least one Primary User Flow
- Every Core Feature from PRD.md must have at least one corresponding flow
- Use real screen names derived from PRD.md feature list — not generic "Home Page"

## Don'ts

- Don't create one mega-diagram covering the whole app — break by feature/goal
- Don't show only the happy path — error and failure paths are required
- Don't leave decision branches without labels
- Don't describe emotional/motivational journey — that's a user journey map, not an app flow
- Don't assume users always arrive from one entry point — document all realistic entries

## Anti-patterns

- Happy-path-only flow → engineer builds happy path, ignores error handling
- Flow with 10+ sequential steps and no decision points → not a real flow, just a list
- Undocumented role-based branching → causes auth bugs
- Screens in the Navigation Map that don't appear in any flow → orphaned screens
