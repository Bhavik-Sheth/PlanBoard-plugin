---
name: diagram-update
description: Generate or refresh PlanBoard ASCII architecture diagrams on demand.
---

# Diagram Update Skill

Generates or regenerates ASCII architecture diagrams into PLANNER/ARCHITECTURE_DIAGRAMS/.
On-demand only — never auto-triggered. Runs in the main thread.

---

## What to generate

Three diagrams, each as a separate Markdown file:

| File | Contents |
|---|---|
| `PLANNER/ARCHITECTURE_DIAGRAMS/SystemArchitecture.md` | High-level component map — how layers and services connect |
| `PLANNER/ARCHITECTURE_DIAGRAMS/DataFlow.md` | How data moves through the system — key flows with direction |
| `PLANNER/ARCHITECTURE_DIAGRAMS/ERDiagram.md` | Entity-relationship diagram — tables, PKs/FKs, cardinality |

Create `PLANNER/ARCHITECTURE_DIAGRAMS/` if it doesn't exist.

---

## Source files to read

Read whichever of these exist in PLANNER/:
- `TRD.md` — for SystemArchitecture and DataFlow
- `Schema.md` — for ERDiagram
- `AppFlow.md` — optional supplement for DataFlow user-facing paths

If TRD.md and Schema.md are both absent or empty, tell the user there is not enough context to generate diagrams and stop.

---

## Target selection (from `/diagram` argument)

| Argument | Regenerate |
|---|---|
| empty / none | All three diagrams |
| "system" or "architecture" | SystemArchitecture.md only |
| "flow" or "dataflow" | DataFlow.md only |
| "er" or "erd" or "schema" | ERDiagram.md only |

---

## Diagram format rules

All diagrams use horizontal ASCII art inside a Markdown code block:
- Arrows: `-->` or `==>` for flow direction
- Boxes: `[ComponentName]` or `[Table: name]`
- Width: fit within 80–100 characters per line
- No Mermaid, no PlantUML, no external rendering required

**SystemArchitecture.md format:**
```
# System Architecture

[Brief one-paragraph description of the architecture]

```
[Client] --> [API Gateway] --> [Auth Service]
                          --> [Core Service] --> [PostgreSQL]
                                             --> [Redis Cache]
```
[Explanation of key components and their responsibilities]
```

**DataFlow.md format:**
```
# Data Flow

## Flow: [Name of flow, e.g. "User Authentication"]
[One sentence describing what this flow represents]

```
[Browser] --> POST /auth/login --> [Auth Service] --> [users table]
                                                  --> [sessions table]
           <-- JWT token + 200 <--
```
[Repeat for each major data flow]
```

**ERDiagram.md format:**
```
# Entity Relationship Diagram

```
[users] --1---N-- [sessions]
   |
   1
   |
   N
[posts] --N---M-- [tags] (via [post_tags])
```

| Entity | Key Fields |
|---|---|
| users | id (PK), email, created_at |
| sessions | id (PK), user_id (FK), expires_at |
| posts | id (PK), user_id (FK), title, body |
| tags | id (PK), name |
| post_tags | post_id (FK), tag_id (FK) |
```

---

## Rules

- Derive everything from the source files — do not invent components, tables, or flows
- ERDiagram must include every entity defined in Schema.md — no omissions
- SystemArchitecture must reflect the actual tech stack from TRD.md — no generic placeholders
- If AppFlow.md is absent, DataFlow.md covers only backend data flows (no UI flows)
- Write clean Markdown with the ASCII diagram inside a code block — no outer fences
