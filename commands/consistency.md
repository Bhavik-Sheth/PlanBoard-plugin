---
description: Run a cross-file consistency check across all PLANNER/ documents
---
Invoke consistency-agent. Pass it the contents or paths of all currently approved files in PLANNER/ (read Tracker.md first to determine which files are ✅ Approved).

Do not run this check on files that are still pending, in progress, or blocked — only approved files are in scope.

If fewer than two files are approved, tell the user a consistency check requires at least two approved files and list what is currently approved.
