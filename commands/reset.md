---
description: Reset a PLANNER/ file back to pending so it can be rewritten
---
The user wants to reset the following file: $ARGUMENTS

Steps:
1. Confirm with the user before proceeding — resetting will discard the current version of `$ARGUMENTS` and mark it as ⏳ Pending in Tracker.md.
2. On confirmation:
   a. Delete PLANNER/$ARGUMENTS (or clear its contents if deletion is not possible).
   b. Invoke tracker-agent with instruction to mark `$ARGUMENTS` as ⏳ Pending in Tracker.md.
   c. Also mark any files that depend on `$ARGUMENTS` as ❌ Blocked in Tracker.md, since their upstream input has changed. List them for the user.
3. Tell the user to run /planboard to resume the sequence — the orchestration will pick up from the now-pending file.

If $ARGUMENTS is not a recognized PLANNER/ file name, tell the user and list valid file names from Tracker.md.

Dependency order for cascade-blocking:
- PRD.md reset → block TRD, Schema, DesignDecisions, AppFlow, Rules, ImplementationPlan
- TRD.md reset → block Schema, DesignDecisions, AppFlow, Rules, ImplementationPlan
- Schema.md reset → block ImplementationPlan
- DesignDecisions.md reset → block ImplementationPlan
- AppFlow.md reset → block ImplementationPlan
- Rules.md reset → block ImplementationPlan
- ImplementationPlan.md reset → no downstream dependents
