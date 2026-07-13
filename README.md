# PlanBoard

A Claude Code plugin that turns a rough idea into a complete, structured project specification — before any code gets written.

PlanBoard runs a sequence of specialized agents, each owning exactly one planning document, with a human approval checkpoint at every step. By the end, you have a full `PLANNER/` folder of markdown files and a `CLAUDE.md` ready to hand off to your coding agent.

---

## What it produces

```
PLANNER/
├── StructuredPlan.md       # Your idea, structured and aligned
├── PRD.md                  # Product Requirements (with Constraints merged in)
├── TRD.md                  # Technical Requirements and stack decisions
├── Schema.md               # Data model — entities, fields, relationships
├── DesignDecisions.md      # Architecture Decision Records (frontend projects only)
├── AppFlow.md              # User flows and navigation map (frontend projects only)
├── Rules.md                # Coding standards and AI agent behaviour rules
├── ImplementationPlan.md   # Tracer-bullet build slices
├── Tracker.md              # Kanban-style status board
└── ARCHITECTURE_DIAGRAMS/
    ├── SystemArchitecture.md
    ├── DataFlow.md
    └── ERDiagram.md

CLAUDE.md                   # Generated at project root by /finalize
```

---

## Installation

### Claude Code

**Step 1 — Add the GitHub repo as a marketplace:**
```
/plugin marketplace add Bhavik-Sheth/PlanBoard-plugin
```

**Step 2 — Install the plugin:**
```
/plugin install planboard@Bhavik-Sheth/PlanBoard-plugin
```

**To update after the author pushes changes:**
```
/plugin marketplace update Bhavik-Sheth/PlanBoard-plugin
/reload-plugins
```

### Codex

Anyone with access to this GitHub repository can add it as a Codex marketplace and install PlanBoard:

```bash
codex plugin marketplace add Bhavik-Sheth/PlanBoard-plugin --ref main
codex plugin add planboard@planboard
```

In Codex, use the skills directly instead of Claude Code slash commands:

```text
$planboard
$planboard-ps
```

To receive the latest published version later:

```bash
codex plugin marketplace upgrade planboard
```

---

## Ideal user journey

1. Start with a rough idea—not a finished specification:

   ```text
   /planboard "A shared household app for tracking chores and supplies"
   ```

2. PlanBoard turns the idea into an initial structured plan, then grills the unclear decisions one at a time. It researches facts available in the codebase, explains why each decision matters, and gives a recommended answer; you make the final decision.

3. Once you confirm a shared understanding, specialist agents create the product requirements, technical requirements, data schema, architecture decisions, application flow, coding rules, and implementation plan. You review and approve each document before the next one begins.

4. When the plan is complete, run `/finalize`. PlanBoard produces a `CLAUDE.md` and a `PLANNER/` folder containing the approved project context, ready to hand to a coding agent.


## How it works

1. `/planboard "<idea>"` structures your idea into `StructuredPlan.md`
2. A Grill Session interviews you to fill in what's missing (constraints, target users, platform, unknowns)
3. Specialist agents draft each document in sequence — each one reads only what it needs and writes exactly one file
4. You review and approve each file before the next one is drafted
5. When you're satisfied with everything, `/finalize` compiles `CLAUDE.md`
6. Hand `CLAUDE.md` to your coding agent (Claude Code or any other) — it now has the full project context

### On-demand features
- `/consistency` — run anytime to check for cross-file contradictions
- `/diagram` — run after Schema or TRD changes to refresh the ASCII diagrams
- Tech stack suggestions — if you answer "I don't know" during the Grill Session, an Opus-powered expert agent gives a specific, justified recommendation

---

## Commands

### `/planboard "<your idea>"`
Start a new planning session or resume an existing one.

Runs the full sequence: structures your idea → interviews you (Grill Session) → drafts each planning document in order → asks for your approval before moving to the next one.

If a `PLANNER/` folder already exists, automatically resumes from where you left off based on `Tracker.md`.

```
/planboard "a CLI tool that helps developers manage multiple .env files across projects"
```

---

### `/planboard-ps`
Start a new planning session from an existing Problem Statement and proposed solution.

PlanBoard collects each input, analyzes where the proposed solution does not fully meet the problem, and lets you revise the solution or proceed with the documented gaps before the normal Grill Session begins.

---

### `/approve <filename>`
Approve a specific file and continue the sequence.

Use this when you've reviewed a file outside the main session and want to signal approval explicitly.

```
/approve PRD.md
/approve TRD.md
```

---

### `/status`
Show the current Kanban-style status of all planning files.

```
/status
```

Status symbols:
| Symbol | Meaning |
|--------|---------|
| ⏳ | Pending |
| 🔄 | In Progress |
| 👀 | Needs Review |
| ✅ | Approved |
| ❌ | Blocked |

---

### `/consistency`
Run a cross-file consistency check across all approved planning documents.

Checks for: PRD features missing from TRD, TRD entities missing from Schema, AppFlow screens without PRD backing, constraint/stack conflicts, and ImplementationPlan scope drift.

```
/consistency
```

---

### `/finalize`
Compile all approved PLANNER/ files into a single `CLAUDE.md` at the project root.

`CLAUDE.md` is the execution-context document your coding agent reads on every prompt — a dense, accurate summary of the full plan under 300 lines.

```
/finalize
```

---

### `/diagram [target]`
Regenerate architecture diagrams in `PLANNER/ARCHITECTURE_DIAGRAMS/`.

Without an argument, regenerates all three diagrams. With an argument, regenerates only the specified one.

```
/diagram              # regenerates all
/diagram er           # ERDiagram.md only
/diagram system       # SystemArchitecture.md only
/diagram flow         # DataFlow.md only
```

---

### `/reset <filename>`
Reset a file back to pending so it can be rewritten. Asks for confirmation first. Cascade-blocks all downstream files that depend on the reset file.

```
/reset TRD.md
```

---


## Design principles

- **One agent, one file.** No shared writers, no ambiguity about ownership.
- **Human approval at every step.** No file advances the sequence until you approve it.
- **Ask, don't guess.** Agents return a structured question signal rather than inventing specifics they don't know.
- **Isolated context per agent.** Each specialist runs in its own context window — quality doesn't drift as the session grows.
- **Nothing automatic.** Diagram regeneration and consistency checks only run when you ask for them.

---

## V1 scope

Not in V1: GitHub integration, live Kanban UI, blast-radius update propagation (`/update`), multi-project management, the `MODULES/` folder, or PS+Idea hybrid mode.
