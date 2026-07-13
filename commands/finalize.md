---
description: Compile all approved PLANNER/ files into CLAUDE.md
---
Read skills/finalize-claude-md/SKILL.md and follow its instructions exactly.

Before compiling:
- Read PLANNER/Tracker.md and check for any files that are not yet ✅ Approved.
- If any required files (PRD.md, TRD.md, Schema.md, Rules.md, ImplementationPlan.md) are not approved, warn the user by listing them and ask whether to proceed anyway or finish the sequence first.
- Files skipped as backend-only (marked ✅ Skipped in Tracker.md) are not required — do not block finalization on them.

On user confirmation (or if all required files are approved), compile CLAUDE.md at the project root per the finalize skill.
