---
description: Approve a PLANNER/ file and continue the planning sequence
---
The user has approved the following file: $ARGUMENTS

Steps:
1. Invoke tracker-agent with instruction to mark `$ARGUMENTS` as ✅ Approved in Tracker.md.
2. Once tracker-agent confirms the update, read skills/planboard-orchestration/SKILL.md.
3. Resume the planning sequence from the next pending item in Tracker.md — dispatch the appropriate specialist subagent as if the user had approved inline during a /planboard session.

Important:
- Do not restart the sequence from the beginning.
- If $ARGUMENTS is not a recognized PLANNER/ file name, tell the user and list valid file names based on Tracker.md.
- If there are no remaining pending files after this approval, inform the user the plan is complete and suggest running /finalize.
