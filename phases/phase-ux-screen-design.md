# Phase 1.5 — UX / Screen Design

The front-end twin of Phase 1: agree on **what the user sees** before deciding how it's built. Skip only if Phase -1 found no UI (state it). Mode-aware — the *visual surface* is whatever Phase -1 detected.

## Steps

1. **Screen inventory** — from `screens.md`, which existing screens does this feature touch? Which are net-new?

2. **Visualize the existing screens.**
   - *Code-first:* run the app and take dev-browser screenshots — the running app is the truth (an aspirational pen is NOT).
   - *Design-source mode:* render the maintained source.
   - **If the PRD references PRD-level pens / sketches / other visual artifacts:** validate **whether and how they fit the overall picture** — reconcile them against `screens.md` (and the design source if one exists). Then **validate the flow is complete in itself** — walk it end-to-end: every entry point leads somewhere, every screen has a way back/forward, no dead-ends, no referenced-but-missing screen.

3. **Design the new/changed screens to a build-ready spec.** Use the repo's chosen surface (a maintained design file, an optional scratch/PRD-level pen, or prose + explicit references to existing components). What must exist for build is a **clear per-screen spec** — layout intent, the data it shows, the actions on it — not a particular tool. Nothing is written to a *canonical* surface here (serialization invariant); canonical screens are recorded from the build (Phase 6).

4. **Screen states** — for each touched screen, enumerate: **empty / loading / error / success / offline**. Reuse existing state components where they exist.

5. **Navigation** — the screen-to-screen edges (which action goes to which screen). This becomes the Mermaid **Nav Map** in `screens.md`.

## UX Stress Test — run before the checkpoint

- *"Which screen here has no empty state — what does the user see with zero data?"*
- *"What does the user see while the slowest call on this screen is loading?"*
- *"What's the error / offline path for the primary action on this screen?"*

## Checkpoint

"Screen set and flows make sense? Any screen or state that feels off?"

> **Residual-risk check:** name 2-3 screens or states we might have missed, or flows that aren't complete end-to-end.

The screens + states + nav edges feed the **UX Delta** and **Screen Registry delta** in the build plan (Phase 6).
