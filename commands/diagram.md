---
description: Regenerate architecture diagrams in ARCHITECTURE_DIAGRAMS/
---
Read skills/diagram-update/SKILL.md and follow its instructions.

Optional target: $ARGUMENTS

Behavior:
- If $ARGUMENTS is empty: regenerate all diagrams (SystemArchitecture, DataFlow, ERDiagram).
- If $ARGUMENTS is provided, match it case-insensitively to one of:
  - "system" or "architecture" → regenerate SystemArchitecture.md only
  - "flow" or "dataflow" → regenerate DataFlow.md only
  - "er" or "erd" or "schema" → regenerate ERDiagram.md only
  - Anything else: tell the user the valid targets (system, flow, er) and do nothing.

Source files to read when regenerating: whichever of TRD.md, Schema.md, AppFlow.md exist in PLANNER/.
