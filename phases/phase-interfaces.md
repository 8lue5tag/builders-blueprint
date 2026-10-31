# Phase 4 — API & Interface Boundaries

1. **Who calls what?** List the main API endpoints or function interfaces, grouped by module. For each: method, path/name, one-line purpose, who calls it.
2. **External API dependencies** — for each third-party service: what you call and when; what happens if it's down or slow; rate limits / costs.
3. **Cross-module contracts** — for each module boundary this feature crosses, confirm the seam defined in Phase 1 still holds. (This is the source for `feature-contracts.md` in Phase 6.)

**Checkpoint:** "Interface boundaries are clear? Any flow that feels awkward?"

> **Residual-risk check:** name the 2-3 external dependencies or interface assumptions most likely to break during development.
