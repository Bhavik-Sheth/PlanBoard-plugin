---
name: rules-knowledge
description: Guidance for producing PlanBoard coding and agent behavior rules.
---

# Rules Knowledge

What makes a good Rules.md. The rules-agent loads this before writing.

---

## What Rules.md is

Coding standards and AI agent behavior rules for this specific project.
Not aspirational. Not generic. Every rule must be specific enough to enforce — if you can't check whether code violates it, it's not a rule.

---

## Required sections and depth

| Section | What it must contain |
|---|---|
| Naming Conventions | Variables, functions, classes, files, DB columns — language-specific patterns with examples. |
| Code Structure | Folder layout rules, import rules, layering (e.g. "no DB queries in route handlers, only in services"). |
| Error Handling | How errors propagate, what gets logged vs surfaced to user, error response format. |
| Validation Rules | Where validation happens (API boundary, service layer, DB), what library, what pattern. |
| Testing Requirements | Minimum coverage target, what must have unit tests, what uses integration tests, mocking policy. |
| AI Agent Behaviour Rules | What the AI can do freely, what requires user confirmation, banned operations. |
| Security Rules | Auth token handling, secrets management (no hardcoded secrets), input sanitisation requirements. |

---

## Dos

- Be specific: "use snake_case for Python variables, PascalCase for class names" not "follow language conventions"
- State the layering rule explicitly: "no direct DB queries in route handlers"
- Define the exact error response format: `{"error": "message", "code": 400}` — not "return an error"
- AI behaviour rules must be granular:
  - "AI may add new functions freely"
  - "AI must ask before deleting any existing function or file"
  - "AI must ask before modifying DB schema or migration files"
  - "AI must not add features not listed in PRD.md Core Features without asking"
- Keep it short — rules that take too long to read get ignored by both humans and AI
- Base every rule on the actual tech stack from TRD.md — generic rules that apply to any project have no value here

## Don'ts

- Don't write aspirational rules: "write clean code", "keep it simple" → unenforceable
- Don't omit AI behavior rules — without them the agent makes well-intentioned wrong decisions
- Don't duplicate what the linter/formatter already enforces — just reference the config file
- Don't create rules that conflict with each other
- Don't write a 30-page rules doc — nobody reads it; aim for 1–2 pages

## Anti-patterns

- Rules so vague they apply to any project: "write tests", "handle errors", "use environment variables"
- No AI behavior rules → agent modifies things it shouldn't
- Security rules that say "be careful with secrets" without specifying how to handle them
- Rules that contradict the tech stack chosen in TRD.md
