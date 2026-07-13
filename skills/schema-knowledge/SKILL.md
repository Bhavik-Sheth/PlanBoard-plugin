---
name: schema-knowledge
description: Guidance for producing a PlanBoard data schema document.
---

# Schema Knowledge

What makes a good Schema.md. The schema-agent loads this before writing.

---

## What Schema.md is

The data model: tables/entities, columns, types, constraints, relationships, indexes.
Source of truth for all data decisions. Engineers build against this — it must be precise.

---

## Required sections and depth

| Section | What it must contain |
|---|---|
| Entity Overview | Brief description of each table: purpose, who creates records, who reads them. |
| Table Definitions | For each table: column name, type, constraints (PK/FK/UNIQUE/NOT NULL/CHECK), business meaning of the column. |
| Relationships | Each FK stated explicitly with cardinality: 1:1, 1:N, M:N (via join table). |
| Entity Relationship Overview | Text description of all entities and their connections. (Diagrams are generated separately via /diagram — do not embed them here.) |
| Indexing Strategy | Which columns are indexed, why (state the query pattern), composite vs single-column. |

---

## Table definition format

```
### table_name
| Column | Type | Constraints | Business Meaning |
|---|---|---|---|
| id | UUID | PK, NOT NULL | Surrogate key |
| user_id | UUID | FK → users.id, NOT NULL | Owner of this record |
| email | VARCHAR(255) | UNIQUE, NOT NULL | Login identifier |
| status | VARCHAR(20) | NOT NULL, CHECK (status IN ('active','inactive')) | Account state |
| created_at | TIMESTAMPTZ | NOT NULL, DEFAULT now() | Audit trail |
| updated_at | TIMESTAMPTZ | NOT NULL, DEFAULT now() | Audit trail |
```

---

## Dos

- Include `created_at` and `updated_at` on every table — non-negotiable for debugging and audits
- Use surrogate keys (UUID or auto-increment int) over natural keys — natural keys change
- Enforce FK constraints at DB level — application-layer validation alone is insufficient
- Use the most specific type: `DATE` not `TIMESTAMP` for birthdates, `BOOLEAN` not `INT(1)` for flags
- Document the business meaning of every column — schema without meaning is just data types
- Use CHECK constraints for enumerated business values
- Document indexing rationale: "indexed on user_id + created_at for timeline queries"
- Entity and field names must match the vocabulary used in TRD.md exactly

## Don'ts

- Don't store multiple values in one column (comma-separated IDs, JSON arrays for relational data)
- Don't use VARCHAR for everything — right type matters for performance and DB-level validation
- Don't leave nullable/NOT NULL ambiguous — it is a business rule, not a DB detail
- Don't invent entities or tables not implied by the TRD or StructuredPlan
- Don't use ambiguous column names: `data`, `info`, `value`, `flag` — name by meaning
- Don't denormalize prematurely — normalize first, denormalize only when query performance requires

## Anti-patterns

- Missing FK constraints → orphan records guaranteed over time
- Everything as TEXT/VARCHAR → loses type safety and DB-level validation
- No indexes → schema looks fine, queries are unusable at scale
- Missing `updated_at` → impossible to detect stale data, breaks cache invalidation
- Schema that adds tables not mentioned anywhere in TRD or PRD → flag as cross-file inconsistency
