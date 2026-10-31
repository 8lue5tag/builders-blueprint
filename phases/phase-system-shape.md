# Phase 1 — System Shape + Module Boundaries

## Part A — System shape

Describe the overall shape:
1. Main "boxes" (web app, API server, DB, worker, external services).
2. How they talk.
3. Where data lives.
4. Walk 1-2 core user flows **at the architecture level**.

Present as a simple ASCII diagram.

## Part B — Module boundary design (for this feature)

1. **Which existing modules does it touch?** List them.
2. **Does it need a new module?** Warranted only when the feature has its own state and lifecycle that doesn't belong to any existing module.
3. **If it touches 3+ modules, apply the orchestrator test:**

> Does the coordination logic have its own state and lifecycle?
> - **YES** → create a new orchestrator module that owns the coordination; other modules just expose interfaces.
> - **NO** → it's a cross-cutting concern (infra, protocol, auth, telemetry). Update each module's `AGENTS.md` with the new boundary rule. No orchestrator.

4. **If cross-module:** define the seam — one line per module for what it must expose/accept. This becomes `feature-contracts.md` (Phase 6).

**Checkpoint:** "System shape and module boundaries make sense? Any box that feels wrong or over-complicated?"

> **Residual-risk check:** name the 2-3 things that could still go wrong with this shape.
