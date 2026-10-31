# Phase 6 — Outputs & Reconcile

Before generating: summarize the session's key decisions as a final confirmation.

**Record, not plan.** The live records (`0.PRD.md`, `1.architecture.md`, `screens.md`) and ops ledgers are **never updated at design time** — they describe shipped reality. During design you produce *deltas* that live in the build plan; the build's final step reconciles the records. Design-time outputs are either **build inputs** (per-module `AGENTS.md`, `feature-contracts.md`) or **feature-scoped docs** (the PRD, SOPs, the build plan).

## Design-time outputs

**1. Updated feature PRD** — apply only feature-specific scope edits/clarifications; keep its structure; no changelog section. Must include a prose **"Screens & Flows"** section: screens touched (by registry/frame name), new screens, per-screen states, navigation, and links to the running-app screenshots / PRD-level pens / sketches. This is words + links — **not** a canonical-surface edit.

**1b. Generic Enhancements Log** — cross-cutting ideas not specific to this feature → `x.product/x.<date>.generic_enhancements.md` (append if today's exists). Each: what, why it matters, which modules.

**1c. Draft PRDs for future features** — ideas worth building but out of scope → `x.product/z.draft-<slug>.md` (lightweight: intent, rough scope, why it came up).

**2. The build plan — `x.product/00.build-<feature>.md`** — a sequenced, acceptance-gated plan a fresh session can pick up cold. It **carries every delta** (the design the plan owns, since the records don't yet reflect it) and a resume marker:

- **Blueprint-progress marker** (top) — phases/checkpoints reached, decisions locked. See `handoffs.md`.
- **Architecture Delta** — system diagram *only if shape changed*; tech-stack decisions (from Phase 2); **Module Registry delta** rows (path / purpose / exposes / depends-on / NEW|CHANGED); new system-level risks; Decision Log entries.
- **UX Delta** — screens added/changed (by frame name), their states, nav edges (from Phase 1.5).
- **Screen Registry delta** — rows to merge into `screens.md`: `Screen | route/component | Area | Purpose | States | Owning feature | Design ref`.
- **STACK / DEPLOYMENT delta** — new/changed services, plans, costs, secrets, deploy steps (from Phase 2).
- **Firmware changes** (only if the repo's `discipline.md` Part 2 applies) — which device types change, what each must do differently, the new version number for each, and any change to the device contract doc or the firmware-versions table. The firmware itself is edited from the build session, following the firmware repo's `discipline.md`.
- **Ordered build steps** — each: Goal · Modules/files · Depends on · Done when (tied to PRD user stories/FRs) · Verify (command or manual check). Respect Phase-5 phasing (complex/low-value last; label fast-follow vs core).
- **References** (doesn't duplicate) the per-module `AGENTS.md`, `feature-contracts.md`, the SOP.

> **The build file's existence = the WIP flag.** While `00.build-<feature>.md` sits in the active folder, the feature is incomplete. On successful completion it **moves to `x.product/archive/`** with a `DONE-` prefix.

**3. Per-module `AGENTS.md`** — for each module created/modified: Owns / Exposes / Depends on / Boundary rules / Implementation details (the module's LLD). If one is getting long, the module needs splitting.

**4. `feature-contracts.md`** (only if cross-module; repo root, append-only) — add one `# Feature Contracts: <feature> (status)` section with `## Module Seams` (must expose / accept / call) and `## Invariants`. Max ~20 lines; if you can't, revisit scope or boundaries. Never rewrite a shipped feature's section.

**5. SOPs / Runbook** — if the feature expects a human process (operator, admin, user), document/update `x.product/c.sop-<role>.md` (one living doc per role): When · Who · Steps · Done when · If it fails. If it adds no human process, state **"No new SOPs"** explicitly.

## The visual record during build

Only if the repo keeps a maintained design file (Pencil, Figma, etc.). Assemble the feature's screens into a **temporary copy next to it** (`<app>.wip.<ext>`, or a duplicated page in Figma) holding the **entirety** of the app's screens (last-shipped canonical + this feature's changes). Do **not** touch the canonical design file until the build is tested green. No design file → skip this; `screens.md` carries the UX.

## Mandatory final step — Reconcile every living doc

The build is **not done** until this checklist is walked. Each item: **update if this feature touched it, else state "no change" explicitly** (don't skip silently).

| Doc | Reconcile to |
|---|---|
| `0.PRD.md` (product) | add/adjust the high-level features this shipped |
| `1.architecture.md` (system) | merge the Architecture Delta (Module Registry rows, Decision Log, risks, diagram) |
| `screens.md` (UX) | merge the Screen Registry delta + Nav Map vs the shipped app |
| **canonical design file** (if any) | **promote the temporary `<app>.wip.<ext>` → replace the canonical design file** (test-gated) |
| `STACK.md` | new services/plans/costs/secrets |
| `DEPLOYMENT.md` | new deploy refs/steps/gotchas |
| `ROADMAP.md` | mark shipped |
| per-module `AGENTS.md` | fold in any implementation changes |
| `feature-contracts.md` | mark this feature's section shipped (append-only; never rewrite earlier sections) |
| `c.sop-<role>.md` | update the human process (or "no new SOPs") |
| firmware (if any) | device contract doc + firmware-versions table in this repo; in the firmware repo, `VERSIONS.md` status and devices (owner confirms each status) |
| **`00.build-<feature>.md`** | **move to `x.product/archive/DONE-…`** (feature complete) |

> This is what keeps every record honest: never updated speculatively at design time, never left stale after a build — the reconcile gate forces them all current exactly when reality changes.
