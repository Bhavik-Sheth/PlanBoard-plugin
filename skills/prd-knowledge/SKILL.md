---
name: prd-knowledge
description: Guidance for producing a PlanBoard product requirements document.
---

# PRD Knowledge

What makes a good PRD.md. The prd-agent loads this before writing.

---

## What PRD.md is

A decision-grade product spec. The layer between "why we're building this" and "what engineering will build."
Not a vision doc. Not a ticket board. An alignment tool with measurable, verifiable requirements.

---

## Required sections and depth

| Section | What it must contain |
|---|---|
| Problem Statement | Specific pain, who has it, why current solutions fail. Not abstract. |
| Target Users | Named role-specific personas (e.g. "Raj, solo SaaS founder, no AWS experience") — never just "developers" or "users" |
| Core Features | MoSCoW split: Must Have / Should Have / Could Have / Won't Have. One sentence per feature max. |
| Out of Scope | Explicit list of what this version will NOT do. Prevents scope creep. |
| User Stories | Format: `As a [user], I want [action] so that [outcome]`. 3–8 stories covering must-have features. |
| Acceptance Criteria | Per story: measurable, testable. "System responds in < 2s" not "it feels fast." |
| Success Metrics | Quantified: completion rate, error rate, latency targets. No vanity metrics. No "user satisfaction." |
| Edge Cases | Offline behavior, empty states, invalid input, concurrent actions, auth failure, rate limits. |
| Constraints | Hard limits only. Written in absolute terms. Pulled from Grill Session answers. |

---

## Dos

- Before writing `PRD.md`, check `PLANNER/StructuredPlan.md` for a `## Fit Analysis` section. If it contains Gaps that were not resolved during the Grill Session—for example, the user proceeded without addressing them or the five-revision cap was reached—include this section in `PRD.md`:
  ```md
  ## Known Gaps (carried from Fit Analysis)
  - [Gap]: not addressed in current scope — flagged for future consideration
  ```
  A flagged gap must never silently disappear between planning and implementation.
- Start with the problem, not the solution
- Link every Core Feature back to at least one User Story
- Use MoSCoW explicitly — forces prioritization decisions
- Keep acceptance criteria testable: a QA engineer must be able to run a pass/fail check
- Match depth to project scope — small project = 1–2 pages; multi-module system = 5+
- Show > tell: prefer concrete examples and user stories over abstract prose

## Don'ts

- Don't use vague language: "user-friendly", "scalable", "modern", "intuitive" — unverifiable
- Don't include implementation details (that's TRD's job)
- Don't skip Out of Scope — its absence causes scope creep every time
- Don't make success metrics qualitative
- Don't write a user story for every edge case — PRD is not a ticket board
- Don't use LLM padding: no "this document outlines...", no filler intros

## Anti-patterns

- Generic persona: "users who want to be productive" → useless
- Missing acceptance criteria → majority of engineering re-requests trace back to this
- PRD that reads like a vision deck (too high level) or a JIRA board (too granular)
- Success metrics that say "increase user satisfaction" without a number
