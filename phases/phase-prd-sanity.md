# Phase 0 — PRD Sanity Check

The **input is the feature PRD being aligned** — never the product-level record (`0.PRD.md`) or a north-star doc. Read it and flag, as a numbered list, resolving each before moving on:

- **Ambiguities** — anything open to multiple interpretations.
- **Hidden complexity** — features that sound simple but are architecturally expensive.
- **Missing decisions** — things the PRD doesn't address that architecture/UX will force.
- **Scale assumptions** — 10 users? 10k? 1M?
- **Module impact** — which existing modules does it touch? If "all" or "unclear", that's a flag.
- **UX surface** — does the PRD name the screens it touches (by registry/frame name)? Which are net-new? Are the referenced visuals attached — running-app screenshots for existing screens, and any **PRD-level pens / sketches / design artifacts** for new ones? Flag screens that don't exist yet. (These get validated and designed in Phase 1.5.)

**Checkpoint:** "PRD is clean and complete enough to design against. Agreed?"

> **Residual-risk check:** name the 2-3 assumptions in the PRD that, if wrong, would force a significant rework.
