# Phase -1 — Repo & UX-source Health Check

Establish context before designing anything. Two parts: repo structure, then UX source.

## Part A — Repo structure

**Q1: 0→1 or existing repo?** If 0→1, skip to Part B, then Phase 0 (module structure gets designed in Phase 1). If existing, continue.

**Q2: Is the root `AGENTS.md` set up right?**

| State | Action |
|---|---|
| `AGENTS.md` exists at root with real project rules | Proceed |
| `AGENTS.md` missing, `CLAUDE.md` has real content | Move it into a new `AGENTS.md`; replace `CLAUDE.md` with one line pointing to it. Get approval before writing. |
| Neither exists | Create an `AGENTS.md` stub + one-line `CLAUDE.md` pointer. Get approval before writing. |

**Q3: Module structure?** A repo has it if logical directories group related code AND each has an `AGENTS.md`.

| State | Action |
|---|---|
| Directories + `AGENTS.md` present | Proceed |
| Directories, no `AGENTS.md` | **Module Documentation Sprint**: read each dir briefly, draft an `AGENTS.md` stub (Owns / Exposes / Depends on / Boundary rules), present all for approval, then write. |
| Flat, no groupings | **Module Migration Sprint**: propose logical groupings (name + what each owns), present the directory map (don't move files yet), get approval, then create dirs + `AGENTS.md` and move files. |

> **Residual-risk check** after any sprint: name 2-3 things that could still be wrong about these boundaries.

## Part B — UX-source health check (detect the mode)

Determine how this repo holds its screens. This picks the visual-source mode for Phase 1.5 and how `screens.md` is derived. Absence is **routed**, never an error.

| Repo state | Registry (`screens.md`) source of truth | Visualize *existing* screens | Design *new* screens |
|---|---|---|---|
| **Code-first** — screens live in code; any design file is aspirational/optional | reverse-engineer `screens.md` from route/page components | **run the app** (dev-browser screenshots) | at the gate: optional sketch or prose + component refs → build → record from shipped result |
| **Maintained design source** — a `.pen`/Figma kept in sync with shipped | derive `screens.md` from it | render the design source | design into the source at the gate |
| **0→1** — no UI yet | grow `screens.md` as screens ship | n/a | design fresh (tool optional) → build → record |
| **No UI** — backend/CLI/lib | — | — | state **"No UX surface — Phase 1.5 skipped"** explicitly |

**Invariants:**
- `screens.md` is always *derived* (from code, a design source, or grown) — never hand-built from nothing, so a missing registry is a cheap regeneration.
- A missing or **aspirational** design file never blocks — it's an optional aid, at most a per-screen "design ref" pointer. (An aspirational/forward-looking pen ⇒ treat the repo as having **no canonical design source**, i.e. code-first.)

If `screens.md` is absent, seed it now (**Screen Registry Sprint**): enumerate the source (code routes/pages, or the design file), draft the registry, present for approval.

**Checkpoint:** "Repo structure + UX source mode agreed? Any boundary or classification that feels wrong?"
