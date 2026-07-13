---
name: planboard
description: Codex entry point for the standard PlanBoard rough-idea workflow.
---

# PlanBoard

Use this skill as the Codex-compatible entry point for PlanBoard.

First read and follow `skills/planboard-orchestration/SKILL.md` exactly.

Then apply the rough-idea flow it defines:

1. Start by structuring the user's idea into `PLANNER/StructuredPlan.md`.
2. Run the grilling workflow one question at a time until the user confirms shared understanding.
3. Continue through the specialist planning documents in the established PlanBoard sequence.
4. Do not use Claude Code slash commands here; this skill is the Codex surface.

If `PLANNER/` already exists, resume from the tracker instead of starting over.
