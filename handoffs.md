# Handoffs between sessions

The blueprint session and the build session are usually **different sessions**, and a long session can lose context mid-way. The durable carrier across every boundary is `**x.product/00.build-<feature>.md**` — it accumulates all deltas plus a progress marker so any fresh session resumes cold.

## The blueprint-progress marker

Keep a short block at the **top** of `00.build-<feature>.md`, updated at each checkpoint:

```
## Blueprint progress
- Source PRD: x.product/<feature-prd>.md
- UX-source mode: code-first | design-source | 0→1 | no-UI   (from Phase -1)
- Phase reached: <e.g. Phase 3 — Data model, checkpoint APPROVED>
- Decisions locked: <one line each — the choices already approved>
- Open / deferred: <anything parked, and why>
```

This is the resume point: a fresh session reads it and knows exactly where to continue and what's already settled — no re-litigating approved checkpoints.

## The boundaries

*   **PRD → Blueprint.** Input = the **feature PRD** + anything it references (running-app screenshots, PRD-level pens, sketches, scratch pens). Never a live/north-star record.
*   **Blueprint → Build.** Hand over `00.build-<feature>.md` — its deltas + progress marker let the builder start cold ("build, follow `00.build-<feature>.md`"). Read order for the builder: the build plan's deltas → the relevant module `AGENTS.md` → `feature-contracts.md` (if any) → the build steps. Use `1.architecture.md` / `screens.md` to see what already exists.
*   **Mid-session context clear.** Same as above — the build plan + marker is the whole state; resume from the phase the marker names.
*   **Build → Reconcile.** On green, promote the temporary design file (`<app>.wip.<ext>`, if the repo keeps one) to canonical and walk the Phase 6 reconcile checklist; then the build file moves to `x.product/archive/` with a `DONE-` prefix (its absence signals the feature is done).

## Resume protocol (fresh session)

1.  Read `SKILL.md` (this skill's overview).
2.  Read `00.build-<feature>.md` — the progress marker says where you are and what's locked.
3.  Load the current phase doc from `phases/` and continue from that checkpoint.
4.  Don't reopen approved decisions unless new information forces it.