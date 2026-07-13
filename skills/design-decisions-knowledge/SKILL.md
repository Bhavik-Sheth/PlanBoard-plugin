---
name: design-decisions-knowledge
description: Guidance for producing PlanBoard architecture decision records.
---

# Design Decisions Knowledge

What makes a good DesignDecisions.md. The design-decisions-agent loads this before writing.

---

## What DesignDecisions.md is

An Architecture Decision Record (ADR) log. Append-only.
Each entry captures one significant architectural choice, why it was made, what was rejected, and what trade-offs were accepted.
Not a planning doc — a history doc. It explains WHY the TRD looks the way it does.

---

## What qualifies as an ADR entry

Only decisions that are:
- Architecturally significant (affect structure or key quality attributes)
- Difficult or costly to reverse
- Non-obvious — if it's the only reasonable choice, it may not need an ADR

**Examples that always qualify:** DB choice, auth strategy, sync vs async, monolith vs microservice, ORM vs raw SQL, state management approach, LLM provider, hosting platform.

**Examples that do NOT qualify:** tabs vs spaces, variable naming conventions, file structure within a module.

---

## Required ADR entry format

```
## ADR-001: [Short decision title]
**Status:** Accepted
**Date:** YYYY-MM-DD
**Context:** Why this decision was needed. What problem, what constraints forced a choice.
**Decision:** What was chosen and the core reasoning in one sentence.
**Alternatives considered:**
  - [Option A]: [why rejected — specific, not "it wasn't as good"]
  - [Option B]: [why rejected]
**Consequences:** Trade-offs accepted. What gets harder, what gets easier. Any technical debt incurred.
```

Number ADRs sequentially starting from ADR-001.

---

## Decisions Log (required section)

An append-only table at the end of the file:

```
## Decisions Log

| Date | ADR | Decision | Rationale | Author |
|---|---|---|---|---|
| YYYY-MM-DD | ADR-001 | Chose PostgreSQL | Schema is fixed; joins required | PlanBoard |
```

Use today's date for all initial entries. Author is always "PlanBoard" for AI-generated decisions.

---

## Dos

- Always include context AND rejected alternatives — a decision without justification becomes meaningless when circumstances change
- Record TechStackExpert-accepted suggestions as formal ADRs — these are the most important entries
- Every major tech choice in TRD.md must have a corresponding ADR
- When a decision changes: write a new ADR that supersedes the old one — never edit accepted records
- Rationale must reference a specific NFR, constraint, or project characteristic — "X because it's popular" is not acceptable
- Short is fine — 5 sentences per section is enough if the decision is clear

## Don'ts

- Don't go back and edit accepted records — append new superseding entries only
- Don't hide consequences — all trade-offs must be explicit
- Don't record a decision without listing at least one alternative considered
- Don't document trivial decisions (it inflates the log and reduces trust in its importance signal)

## Anti-patterns

- Single entry covering multiple unrelated decisions — one ADR per decision
- "We chose X because it's better" — better at what? Name the NFR or constraint
- Decisions made in TRD but not recorded here → "decision amnesia" → repeated debates in later sessions
