---
name: planboard-orchestration
description: Run the full PlanBoard planning and approval workflow.
---

# PlanBoard Orchestration

This is the master sequence and dispatch skill. Load this at the start of every `/planboard` session.

---

## 1. Sequence

Run these steps in order. Do not skip steps. Do not run steps out of order.

```
Step 1  → [structure-idea skill]  → writes PLANNER/StructuredPlan.md
Step 2  → [grill-agent, Mode A]   → interviews user, updates StructuredPlan.md
Step 3  → [prd-agent]             → writes PLANNER/PRD.md
Step 4  → [trd-agent]             → writes PLANNER/TRD.md
Step 5  → [Frontend detection]    → decides whether to run Steps 6–7
Step 6  → [design-decisions-agent] → writes PLANNER/DesignDecisions.md  (skip if backend-only)
Step 7  → [appflow-agent]          → writes PLANNER/AppFlow.md           (skip if backend-only)
Step 8  → [schema-agent]          → writes PLANNER/Schema.md
Step 9  → [rules-agent]           → writes PLANNER/Rules.md
Step 10 → [implementation-plan-agent] → writes PLANNER/ImplementationPlan.md
```

After every step that writes a file:
- Invoke tracker-agent to update that file's status to 👀 Needs Review
- Show the user what was written (display the file content or a 2–3 bullet summary)
- Wait for explicit approval before proceeding to the next step
- On approval: invoke tracker-agent to mark the file ✅ Approved, then proceed

---

## 2. Starting a Session

**New session (PLANNER/ does not exist, or StructuredPlan.md is empty):**
1. Create PLANNER/ directory if it doesn't exist
2. Invoke tracker-agent to initialize Tracker.md with all files set to ⏳ Pending
3. Load the structure-idea skill and execute Step 1

**Resume (PLANNER/ exists with content):**
1. Read PLANNER/Tracker.md
2. Find the first file that is NOT ✅ Approved or ✅ Skipped
3. Jump to the sequence step that owns that file and continue from there
4. If all files are ✅ Approved, tell the user the plan is complete and suggest `/finalize`

---

## 3. Frontend Detection (between Step 4 and Step 5)

After TRD.md is approved, scan PLANNER/StructuredPlan.md and PLANNER/TRD.md for any of these signals:
`UI`, `frontend`, `screen`, `page`, `component`, `React`, `Vue`, `Next`, `Svelte`, `mobile`, `app`, `dashboard`, `interface`, `browser`

- If ANY signal found → run Steps 6 and 7 normally
- If NO signal found → skip Steps 6 and 7; invoke tracker-agent to mark both DesignDecisions.md and AppFlow.md as `✅ Skipped — backend-only`

This is a judgment call, not a keyword match. Use the full context to decide.

---

## 4. Approval Gate (after every file-writing step)

After a specialist agent returns `✅ FILE_COMPLETE: <filename>`:
1. Invoke tracker-agent: mark `<filename>` as 👀 Needs Review
2. Show the user the file summary (the bullet points from FILE_COMPLETE signal)
3. Ask: "Does this look good? Type **approve** to continue, or describe what to change."
4a. If approved: invoke tracker-agent → ✅ Approved → proceed to next step
4b. If changes requested: re-invoke the same specialist agent with the change request appended to its context; repeat from step 1

---

## 5. Grill Relay (handling NEEDS_INPUT from a specialist)

When a specialist agent returns `❓ NEEDS_INPUT`:
1. Extract the question and CONTEXT from the signal
2. Invoke grill-agent in Mode B (escalation), passing:
   - The question
   - The CONTEXT (partial work) from the NEEDS_INPUT signal
   - The reason the specialist needs this
3. If grill-agent returns `✅ ESCALATION_ANSWER`:
   - Extract the answer
   - Re-invoke the original specialist agent with its original context + the new answer appended
4. If grill-agent returns `NEEDS_EXPERT: <question>`:
   - Invoke tech-stack-expert-agent with the question and PLANNER/StructuredPlan.md + PLANNER/PRD.md as context
   - Present the expert's RECOMMENDATION, WHY, TRADEOFF, and ALTERNATIVE to the user
   - Ask: "Accept this suggestion, or provide your own answer?"
   - Record the answer (accepted suggestion or user's custom answer)
   - Re-invoke grill-agent with the answer, then re-invoke the original specialist

---

## 6. Tracker Updates Summary

Invoke tracker-agent at these moments:
- Session start: initialize all rows to ⏳ Pending (new session only)
- Before specialist runs: mark file 🔄 In Progress
- After specialist returns FILE_COMPLETE: mark file 👀 Needs Review
- After user approves: mark file ✅ Approved
- After frontend-skip detection: mark DesignDecisions.md + AppFlow.md as ✅ Skipped — backend-only
- After specialist returns NEEDS_INPUT: mark file ❌ Blocked

---

## 7. Constraints

- Never invoke a specialist agent before its prerequisite files are ✅ Approved
- Never proceed past an approval gate without explicit user approval
- Never write directly to any PLANNER/ file — all writes go through the designated specialist agent
- Tracker.md is only ever written by tracker-agent — never update it directly
- Steps 6 and 7 are always skipped or run together — never one without the other
