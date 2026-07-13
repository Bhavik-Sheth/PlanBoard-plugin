---
name: structure-idea
description: Convert a rough idea into PlanBoard's structured project plan.
---

# Structure Idea Skill

Converts the user's raw idea into a clean, structured PLANNER/StructuredPlan.md.
This is always Step 1 in the sequence. Run in the main thread (no subagent needed).

---

## What to do

1. Take the raw idea from the user (passed as `$ARGUMENTS` to `/planboard`, or typed interactively)
2. If the raw idea is fewer than 2 sentences or is too vague to act on (e.g. "an app for productivity"), ask the user one clarifying question: "Can you tell me a bit more about the problem you're trying to solve and who would use it?"
3. Structure the idea into PLANNER/StructuredPlan.md using the format below
4. Show the user what was written and ask if it captures their idea correctly
5. Allow one round of corrections if the user wants to adjust anything
6. Once confirmed, mark as done and continue to the Grill Session

---

## StructuredPlan.md Format

```md
# Structured Plan

## Problem Statement
[Specific pain. Who has it. Why current solutions fail or don't exist.]

## Proposed Solution
[What this project builds. How it addresses the problem at a high level. No implementation detail.]

## Key Goals
[3–5 concrete, specific goals — not vague aspirations. "Users can X without Y" format.]

## Non-Goals
[What this project will explicitly NOT do in this version.]

## Target Users
[Named, role-specific personas. "Developers" is not a persona. "Solo SaaS founders with no DevOps background" is.]

---

## Grill Session Answers
[This section is appended by grill-agent after the interview. Leave blank here.]
```

---

## Rules

- Be faithful to the raw idea — do not invent features, goals, or scope the user didn't describe
- If something is ambiguous in the raw idea, represent it as ambiguous — the Grill Session will resolve it
- Problem Statement must name a real, specific problem — not a category ("I want to build a task manager")
- Target Users must be specific — reject "general users" or "everyone"
- Do not write implementation details (tech stack, architecture, DB) — that belongs in TRD
- Write clean Markdown directly — no surrounding code fences
- Keep it concise: StructuredPlan.md should be 1–2 pages maximum
