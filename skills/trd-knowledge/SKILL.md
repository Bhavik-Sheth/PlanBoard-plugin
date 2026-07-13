---
name: trd-knowledge
description: Guidance for producing a PlanBoard technical requirements document.
---

# TRD Knowledge

What makes a good TRD.md. The trd-agent loads this before writing.

---

## What TRD.md is

Translates PRD features into engineering language. Describes the **how**, not the **what**.
Audience: engineers and QA — not PMs or stakeholders.
Every requirement must be specific enough that QA can write a test for it.

---

## Required sections and depth

| Section | What it must contain |
|---|---|
| Tech Stack | Every layer: frontend (if any), backend, database, infra, auth, messaging. Each choice justified against a PRD requirement or constraint. |
| System Architecture Overview | How components connect. High-level data flow. Text description with ASCII or Mermaid diagram reference. |
| Functional Requirements | PRD features → specific technical behavior: API contracts, data validations, business logic rules. |
| Non-Functional Requirements (NFRs) | Numbers, not adjectives. "p95 latency < 200ms under 1000 concurrent users." "99.9% uptime." |
| API Design | Endpoints: method, path, request shape, response shape, error codes. At least the core CRUD surface. |
| Data Storage & Retrieval | Which DB, why, query patterns, caching strategy (if any). |
| Security | Auth mechanism, data encryption at rest/transit, input validation approach, rate limiting. |
| Third-Party Integrations | Each service: purpose, which API/SDK, what happens if it fails (fallback or hard failure). |
| Technical Constraints | Hard limits inherited from PRD.md's Constraints section — restated in technical terms. |

---

## Dos

- NFRs drive architecture — state them before justifying stack choices
- Name real libraries and frameworks: "FastAPI 0.111", not "a Python web framework"
- Justify every tech choice against a specific NFR or constraint: "PostgreSQL over SQLite because the schema requires joins across 5+ tables with concurrent writes"
- Every requirement must be testable — if QA can't verify it, rewrite it
- Explicitly state what this TRD does NOT cover (mirrors PRD's Out of Scope)
- Include error handling and failure modes — a common omission that causes production incidents

## Don'ts

- Don't use vague NFRs: "high performance", "secure", "scalable" — pick real numbers
- Don't write a TRD before PRD is stable — it will change
- Don't over-specify implementation to the point it prevents engineering judgment
- Don't conflate TRD with API documentation — TRD defines requirements, not usage instructions
- Don't skip the fallback behavior for third-party integrations

## Anti-patterns

- Stack choice with no rationale ("we'll use React")
- NFRs stated as "as fast as possible" or "no downtime" — pick real numbers
- Missing third-party fallback behavior
- Functional requirements so vague they could describe any app ("users can manage their data")
