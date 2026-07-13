---
name: structure-ps-solution
description: Create a validated PlanBoard structured plan from a problem statement and proposed solution.
---

# Structure PS + Solution

Use this skill only for a new `/planboard-ps` session. It is separate from the rough-idea intake flow.

## Step 1 — Collect the Problem Statement

Ask exactly:

```text
Paste or describe the Problem Statement (PS).
This is the problem you're solving — not your solution.
Say "done" when finished.
```

Wait for the user's response, then capture it as the PS text.

## Step 2 — Collect the proposed solution

Ask exactly:

```text
Now describe your proposed solution to this PS.
What will you build? How does it address the problem?
Say "done" when finished.
```

Wait for the user's response, then capture it as the Solution text.

## Step 3 — Write `PLANNER/StructuredPlan.md`

Clean the PS and proposed solution for clarity without changing their meaning. Write `PLANNER/StructuredPlan.md` with exactly these sections:

```md
## Problem Statement (Structured)
[Cleaned, specific version of the PS — who is affected, what the pain is, why it matters, scope of the problem]

## Solution Overview
[Cleaned, specific version of the proposed solution — what it builds, how it addresses the PS, what it explicitly does not do]

## Fit Analysis
### Gaps
[Aspects of the PS the solution doesn't address]
### Assumptions
[Things the solution assumes that the PS doesn't guarantee]
### Risks
[Where the solution may fail to solve the problem in edge cases]

## Validated Scope
[The intersection of what the PS requires and what the solution proposes — this is what the PRD agent builds against]
```

The Fit Analysis must be honest. State real gaps plainly; never soften a mismatch to make the solution sound more complete than it is.

## Step 4 — Revision loop

Show the user the complete `## Fit Analysis` section, then ask exactly:

```text
Here's the fit analysis between your PS and proposed solution.
[Gaps / Assumptions / Risks shown]

Proceed with current scope, or revise your solution first?
[proceed / revise]
```

- If the user chooses `revise`, return to Step 2, collect the updated solution, rewrite `PLANNER/StructuredPlan.md`, and repeat this step.
- Count each `revise` response. After the fifth revision request, do not collect another solution. Output exactly:
  ```text
  Max revisions reached. Proceeding with current scope — noted gaps remain in StructuredPlan.md for reference.
  ```
  Then continue to the Grill Session.
- If the user chooses `proceed`, continue to the Grill Session.

## Rules

- Do not use this skill when `PLANNER/` already exists; resume from `Tracker.md` instead.
- Do not modify `skills/structure-idea/SKILL.md` or invoke it from this flow.
- Do not start specialist planning until the user proceeds or the revision cap is reached.
