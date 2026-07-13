---
name: implementation-plan-knowledge
description: Guidance for producing PlanBoard tracer-bullet implementation plans.
---

# Implementation Plan Knowledge

What makes a good ImplementationPlan.md. The implementation-plan-agent loads this before writing.

---

## What ImplementationPlan.md is

A tracer-bullet build roadmap. Breaks the full project into ordered, vertically-sliced pieces, each ending in a working, independently actionable deliverable that a coding agent can pick up and implement without additional context.

Not a Gantt chart. Not a ticket list. Not a phase table with time estimates.

---

## The tracer-bullet slice format

Each slice is a markdown heading block. This is the only format allowed:

```
## Slice N: [Short descriptive name]

### What this delivers
[One sentence. A complete, independently runnable, user-observable piece of functionality.
Not "the database layer" — that's horizontal. "Users can register and log in via email/password" — that's vertical.]

### Depends on
[Slice numbers this slice requires to be complete first, or "none"]

### Touches
[Which files, modules, endpoints, DB tables, or UI components this slice creates or modifies.
Be specific: "POST /auth/register endpoint, users table, auth middleware"]
```

The file starts with `# Implementation Plan` and then slice blocks, nothing else.

---

## Slicing rules

**Vertical, not horizontal:**
- Wrong: "Slice 1: Set up all DB tables" → horizontal, no observable behavior
- Right: "Slice 1: User can register with email and password, record stored in users table" → vertical

**Each slice must:**
- Produce something runnable and observable when complete
- Be completable in isolation — a coding agent picks it up with no other context
- Reference entity names from Schema.md exactly as written

**First slice rule:**
Slice 1 must always be the minimal runnable foundation — something that works end-to-end even if it does almost nothing (e.g. "API server starts and returns 200 on GET /health").

**Coverage rule:**
Every feature from PRD.md's Core Features must appear in at least one slice.
Nothing from PRD.md's Out of Scope may appear in any slice.

---

## What is explicitly NOT in this format

- No time estimates
- No MVP cut-line marker
- No phase labels (V1, V2, nice-to-have, post-MVP)
- No "Phase N —" headings — use "Slice N:" only
- No checklist sub-tasks within slices
- No dependency graphs or tables

These were in the old Python version. They are not in the plugin version.

---

## Dos

- Keep slices small enough to implement in a focused session
- Dependencies should be sequential: Slice 2 depends on Slice 1, not Slice 7
- "Touches" field should name real files and endpoints — not just "the backend"
- If DesignDecisions.md and AppFlow.md exist, reference them to ensure UI flows have corresponding slices
- The last slice should wire everything together into the complete working system

## Don'ts

- Don't make a slice that ends in "code written" with no testable behavior
- Don't make Slice 1 a pure infrastructure slice (no observable output)
- Don't let any PRD Core Feature remain uncovered by any slice
- Don't reference entities, endpoints, or modules not defined in Schema.md or TRD.md

## Anti-patterns

- Slice that delivers "all DB tables created" → horizontal layer, no observable behavior
- Slice with "Depends on: none" when it clearly needs a prior API or DB table → breaks implementation order
- 20+ slices where each is one function → too granular, defeats the purpose
- 3 slices where each is a full project phase → too coarse, no actionable guidance
