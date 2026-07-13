---
name: grill-technique
description: Interview workflow for resolving PlanBoard planning decisions one at a time.
---

# Grill Technique Skill

How to interview the user to fill in missing project information.
Load this before every grill session (both Mode A and Mode B).

---

## Core principle

One question at a time. Always. Never batch two questions in one message.
The goal is alignment, not interrogation — keep the tone collaborative.

---

## What to ask about (Mode A — upfront broad interview)

Cover these areas, but only ask about a topic if it's genuinely unclear from StructuredPlan.md.
Do not ask about things already clearly stated.

**Must cover if not already known:**
- Who exactly is the target user? (if the persona in StructuredPlan.md is vague)
- What platform does this run on? (web, mobile, CLI, desktop, API-only)
- Are there any hard technical constraints? (must use X, must avoid Y, must run offline, free tier only)
- Are there legal or compliance requirements? (data residency, GDPR, HIPAA, etc.)
- Any budget constraints that affect tech choices? (no paid APIs, free hosting only, etc.)
- Any must-have integrations? (Stripe, Twilio, a specific database, etc.)

**Ask only if directly relevant to the project:**
- Authentication: does this project need user accounts? If yes, what auth method?
- Approximate scale: expected users/day, data volume, concurrent sessions?
- Any existing codebase or infrastructure to build around?

**Do NOT ask about:**
- Tech stack preferences (unless a constraint is mentioned) — TRD agent handles this
- Design / color / branding choices — out of scope for planning
- Vague future roadmap ideas — focus on the current version only

---

## Escalation rule (both modes)

If the user answers "I don't know", "not sure", "?", or clearly has no preference on a tech question, do NOT:
- Guess an answer
- Ask the same question again differently
- Move on without resolving it

Instead, immediately return:
```
NEEDS_EXPERT: <the question exactly as asked>
```

The main thread will invoke the tech-stack-expert-agent and relay an answer back.

---

## Interview technique rules

- Ask questions in plain conversational language — not formal survey language
- If the user's answer is ambiguous, ask one follow-up question to clarify before moving on
- If the user says "whatever you think is best" on a non-tech question (e.g. target users), push back once: "I want to make sure we're building for the right person — can you describe who would actually use this?"
- If the user says it a second time, record their answer as-is and move on
- After all questions are answered, summarize what you've learned in 3–5 bullets and ask the user to confirm before ending the session
- Do not end the grill session without confirmation

---

## Questions to never ask

- "What do you want me to build?" — that's what StructuredPlan.md is for
- "Any other requirements?" — too open-ended; ask about specific gaps instead
- Questions about visual design, colors, fonts, or branding
- "Should I use X or Y?" — that's TechStackExpert's job, not the user's
