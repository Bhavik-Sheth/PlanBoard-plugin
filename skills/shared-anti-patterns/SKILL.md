---
name: shared-anti-patterns
description: Cross-cutting PlanBoard rules that prevent common planning mistakes.
---

# Shared Anti-Patterns

Cross-cutting rules every agent must follow. Load this alongside your specialist knowledge.

---

- **LLM padding:** Never open with "This document outlines...", "In order to ensure...", "As a [role], I will...", or any similar filler introduction. Start with the content immediately.

- **Vague adjectives:** fast, scalable, secure, modern, robust, user-friendly, flexible, powerful → always quantify or don't include. "Responds in < 200ms at p95" is acceptable. "Fast" is not.

- **Hallucinated specifics:** If a fact (library version, API endpoint name, metric, third-party service) is not confirmed by StructuredPlan.md, PRD.md, TRD.md, or explicit user input, do not invent it. Return `❓ NEEDS_INPUT` instead.

- **Missing negatives:** Every planning document needs its "what this is NOT" section — out-of-scope, non-goals, must-not-do. A document without explicit scope boundaries causes scope creep.

- **Premature detail:** Do not write TRD-level implementation detail in the PRD. Do not write schema-level field definitions in the TRD. Each document owns its layer — stay in your lane.

- **Cross-file inconsistency:** If you notice that something you are writing contradicts an already-approved upstream document (e.g. your Schema introduces a table not mentioned in TRD, or your Rules conflict with the TRD tech stack), flag it in your FILE_COMPLETE summary rather than silently proceeding. Write: "Note: [specific inconsistency detected] — user should run /consistency."

- **Surrounding code fences:** Do not wrap your entire Markdown output in triple backticks. Write Markdown content directly. Code fences are only acceptable inside the document for code samples, schema blocks, or diagram blocks.

- **Generic personas:** "Users", "developers", "businesses", "people who want to be productive" are not personas. If target user information is too vague, return `❓ NEEDS_INPUT`.

- **Untestable requirements:** Any requirement that QA cannot verify in a pass/fail way is not a requirement. Rewrite it or return `❓ NEEDS_INPUT`.
