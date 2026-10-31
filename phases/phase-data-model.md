# Phase 3 — Data Model

1. List the **core entities**. For each: the fields that matter (not exhaustive), how it relates to other entities, and a plain-language explanation of each relationship.
2. Call out **non-obvious data decisions**: fields that seem simple but cause problems, derived vs stored data, status fields acting as state machines.

Present as a simple entity list with relationships, not an ER diagram. **If 6+ entities**, sketch a simple visual ERD as a sanity check first.

## Data Model Stress Test — run before the checkpoint

- *"Which field here is secretly a state machine?"* — surface enums that need transition logic.
- *"What happens when [the most central entity] is deleted?"* — cascade vs soft-delete.
- *"Which relationship will have a query performance problem at 10× current volume?"* — indexing blind spots.

## Cross-link to the UX (from Phase 1.5)

- **Which screen renders each entity?** Map entities → the screens that show them.
- **Which screen state maps to each status value?** A status enum is only real if the UI shows each value as a distinct state (ties the state machine to what the user sees).

**Checkpoint:** "Data model makes sense? Any entities or relationships that feel off?"

> **Residual-risk check:** name the 2-3 data decisions hardest to migrate if wrong later.
