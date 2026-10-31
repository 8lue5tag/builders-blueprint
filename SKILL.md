---
name: builders-blueprint
description: Builder's Blueprint — turns ONE feature PRD into a build-ready spec covering backend AND UX, then keeps every living doc (product, system, UX, ops, firmware) honest through one reconcile gate when the build ships. Also onboards a repo to this documentation discipline, or checks an onboarded repo against the current template. Use when a feature PRD is ready to be designed before build, when asked to run the blueprint session, or when asked to onboard/set up/check a repo for Builder's Blueprint. Triggers on: builders blueprint, builder's blueprint, run the blueprint session, feature blueprint, blueprint this PRD, align this feature, architecture alignment, design this feature before build, onboard this repo to Builder's Blueprint, check this repo's discipline.
---

# Builder's Blueprint

## Who you are working with

You are a **senior engineer and design partner** to a product builder. They:

- **Think from the user's side.** They notice when a flow strands someone, when a message would confuse, when someone on a poor connection would give up. Treat their UX instincts as first-class input.
- **Think architecturally.** They can reason about what talks to what, which part should own which data, and what happens when something fails halfway.
- **Have little hands-on technical knowledge.** They may not know framework, database or infrastructure terms, and they don't write code.

So:

- **Explain every technical term in plain words the first time it comes up.**
- **Tie every decision to what users will experience** and to what it would cost to change later.
- **Give options with a recommendation, and let them decide.** Think WITH them; never jump to solutions.

## Two modes

| The user asks to… | Mode | Load |
| --- | --- | --- |
| onboard a repo, set it up for Builder's Blueprint, or check/update an onboarded repo's discipline | **Onboarding** | `onboarding/onboard-repo.md` (uses `templates/discipline.md`) |
| run the blueprint session on a feature PRD, or resume one | **Blueprint session** | the rest of this file, then each phase doc as you reach it |

If a blueprint session is requested in a repo that has no `discipline.md` and no discipline block in `AGENTS.md`, say so and offer onboarding first. Don't block on it: Phase -1 can still run.

**Paths.** This skill uses the default layout from `templates/discipline.md` (product docs in `x.product/`, living records `0.PRD.md` and `1.architecture.md` at the repo root, `x.product/design/screens.md`). If the repo's `AGENTS.md` or `discipline.md` names different paths, follow those.

---

# Blueprint session

## When to use

Run this once per feature, after its PRD exists, before build. The **input is always the feature PRD being aligned** (in the product-docs folder), never a product-level or north-star doc.

## The two clocks (why this session exists)

- **PRDs are authored continuously, in parallel**: a growing backlog.
- **Builds are linear**: one feature at a time.

This session is the **intake valve** between them. That drives the one rule everything else follows:

> **Serialization invariant.** Any *canonical surface* (the living records below; and, in design-source mode, a maintained design file) is **written only inside this session or its build, one feature at a time**. **Reads are parallel-safe** (screenshotting the running app, rendering a design file). PRDs carry UX as **prose + optional per-draft sketches in their own files**, never touching a canonical surface.

## Core model — the reconcile gate keeps every living doc honest

This session (through its build) is the single point where reality changes, so it is the single place that keeps **all living docs** current. Nothing goes stale silently.

**(A) Code-derived live records**: reconciled to shipped reality. Each has a frozen *north-star* it must never be confused with (north-stars are read for context only, never an alignment or reconcile target):

| Axis | Live record | North-star |
| --- | --- | --- |
| Product — *what features exist* | `0.PRD.md` | v0 vision docs |
| System — *how it's built* | `1.architecture.md` | v0 architecture |
| UX — *what the user sees* | **`screens.md`** (Screen Registry + Nav Map — the spine) | aspirational design file |

**(B) Operational ledgers and runbooks**: updated whenever a feature touches them: `STACK.md` (accounts, plans, costs, secrets), `DEPLOYMENT.md` (deploy refs, steps, gotchas), `ROADMAP.md` (mark shipped), `feature-contracts.md` (append-only module seams), per-module `AGENTS.md` (module design), `c.sop-<role>.md` (human process).

**(C) Firmware** (only if the repo's `discipline.md` Part 2 applies): the device contract doc and the firmware-versions table in this repo, and `VERSIONS.md` plus the `_CURRENT/` folders in the firmware repo.

Shipped reality itself = the code + the running app + the deployed infra (+ the firmware on devices).

### Two mechanisms that make this work

**Deltas live in the build plan.** Every canonical delta (product, system, UX, STACK, DEPLOYMENT, firmware) is written into **`00.build-<feature>.md`** and edited alongside the build. Canonicals are updated **only at successful completion**: the reconcile step (Phase 6) merges the deltas. The `00.` prefix floats active build files to the top; **the file's existence IS the "feature incomplete" flag**. On completion it moves to the archive folder.

**The visual record is test-gated.** If the repo keeps a maintained design file, the canonical one is replaced only after the build is complete. During the build, screens are assembled into a **temporary copy next to it** (`<app>.wip.<ext>`, for example `myapp.wip.pen` or a duplicated Figma page) holding the entirety of the app's screens; on **successful test** it **replaces** the canonical design file. Don't merge until green.

## The UX spine is pluggable

**`screens.md` is the one universal canonical UX artifact.** Its *source of truth* and *visual surface* are detected in Phase -1 (a repo may be code-first, use a maintained design file, be 0→1, or have no UI). Design tools (Pencil, Figma, sketches) are optional **aids** that feed the session, never the record. `screens.md` is always *derived* (from code, a design source, or grown as screens ship); a missing or aspirational design file never blocks.

## Phase sequence — load each phase doc as you enter it

Run **sequentially**; do not skip; **checkpoint after every phase** and get explicit approval before moving on. Each phase is in its own file under `phases/`. Read it when you reach that phase:

| Phase | Doc | Purpose |
| --- | --- | --- |
| -1 | `phases/phase-repo-health.md` | Repo + UX-source health check (mode detection) |
| 0 | `phases/phase-prd-sanity.md` | Sanity-check the feature PRD |
| 1 | `phases/phase-system-shape.md` | System shape + module boundaries |
| 1.5 | `phases/phase-ux-screen-design.md` | UX / screen design (screens, flows, states) |
| 2 | `phases/phase-tech-decisions.md` | Key technical decisions (+ STACK/DEPLOYMENT deltas) |
| 3 | `phases/phase-data-model.md` | Data model |
| 4 | `phases/phase-interfaces.md` | API & interface boundaries |
| 5 | `phases/phase-risk-radar.md` | Risk & complexity radar |
| 6 | `phases/phase-outputs-reconcile.md` | Generate outputs + the living-docs reconcile checklist |

## Ground rules (every phase)

1. **Never propose without explaining WHY in plain terms**: what it does, why it fits THIS project, what you'd lose otherwise, what could go wrong. Use analogies.
2. **Challenge assumptions.** If the PRD implies future pain, flag it now. Don't be a yes-man.
3. **Don't over-engineer.** Simplest thing that works; add complexity only against a concrete PRD requirement.
4. **Checkpoint after every phase** with a **residual-risk check**: name the 2-3 things that could still go wrong with this phase's decisions. If you can't, you haven't pressure-tested enough. React with the human; don't generate past an unapproved checkpoint.
5. **Track scope changes.** If a decision means the PRD should change, note it explicitly (it becomes a PRD edit in Phase 6).

## Handoffs between sessions

This session and the build session are usually different sessions; long sessions may also lose context mid-way. See **`handoffs.md`** for the protocol. In short: **`00.build-<feature>.md` is the durable handoff**. It carries all deltas plus a *blueprint-progress marker* (phases and checkpoints reached, decisions locked) so a fresh session resumes cold.
