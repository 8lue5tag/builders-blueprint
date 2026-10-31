# Documentation Discipline

> **Builder's Blueprint discipline v1.0** — template from https://github.com/8lue5tag/builders-blueprint
> Local edits are fine. Keep this line: it lets Claude compare this copy with the current template.

How docs are kept honest in this repo. Portable: the same file goes into every repo that works
this way, including firmware repos.

The whole thing rests on one idea:

> **Ideas are written in parallel. Work happens one thing at a time. There must be exactly one
> place where "what's real" gets written down, and it only gets written when something actually
> ships.**

That single write-point is the **blueprint session** (the `builders-blueprint` skill). Everything
below follows from that.

**This file has two parts:**

- **Part 1 (§1–§7)** applies to every product repo: app, backend, website, API.
- **Part 2 (§8–§14)** applies only if the product includes **firmware or devices**. It covers both
  the firmware repo itself and what the main repo does when a feature changes a device.

---

## 0. How to use this file

The easiest way is to let the skill do it: open the repo in Claude Code and say
**"Onboard this repo to Builder's Blueprint"**. Without the skill, run Claude on this file and do
the following, in order:

1. **Work out this repo's role.**
   - **Main repo**: has PRDs, an app or backend, and users. Part 1 applies. If the product also has
     a firmware repo, the "in the main repo" parts of Part 2 apply too.
   - **Firmware repo**: holds device firmware only. Only Part 2 applies. A firmware repo has no
     PRDs, architecture doc, screens or feature contracts of its own.
2. **Read every doc (or, in a firmware repo, every file).** Sort each one into the families in §2
   (main repo) or the folders in §9 (firmware repo).
3. **Propose renames and moves** so names and locations match §3 (main repo) or §9 (firmware repo).
   Show the plan first. Change nothing until the owner has read it and said yes.
4. **Flag what doesn't fit**, and any two docs competing to be the same living record. Ask; don't
   guess.
5. **Never destroy history.** Old PRDs, `DONE-` files and archived firmware stay frozen (§5, §14).
   They may be moved or renamed, but their contents are never edited.
6. **Make sure the required records exist.** In a main repo: the three living records, especially
   `screens.md` (§2A). In a firmware repo: `VERSIONS.md` with an entry for every firmware folder
   (§11). Create the ones that are missing.
7. **Last step: seed `AGENTS.md`.** Write the matching short block from §15 into the root
   `AGENTS.md`, so the repo keeps this discipline after the run ends. Make sure the root `CLAUDE.md`
   contains the line `@AGENTS.md`, so Claude Code loads it in every session.

This file can stay in the repo as the full reference, or be deleted once `AGENTS.md` carries the
short version. Owner's call.

---

# Part 1 — Every product repo

## 1. The two clocks

- **PRDs are written all the time, in any order.** A growing pile of ideas. This is fine.
- **Building is linear.** One feature at a time.

The blueprint session is the valve between the two. It pulls **one** PRD out of the pile and turns
it into a build. Because it's the only valve, it's also the only place allowed to change the docs
that describe reality.

---

## 2. The four families of docs

Every doc in the repo is one of these four. Know which family a doc is in before you touch it: the
rules are different for each.

### A. Living records — always current, describe *what is real now*

Three of them, one per axis:

| Axis | Question it answers | The living record |
| --- | --- | --- |
| Product | What features exist? | `0.PRD.md` (repo root) |
| System | How is it built? | `1.architecture.md` (repo root) |
| UX | What does the user see? | `x.product/design/screens.md` (screen list + navigation map) |

These are edited **in place**. No dates, no history, no changelog inside them. They say what is
true today. Old truth is deleted, not struck through. They are updated **only** by the blueprint's
reconcile step (§4), when a feature actually ships.

`screens.md` is the **required** UX record. It holds the truth in words: every screen, and how you
move between them. Every repo with a UI has one. A visual design file (Pencil, Figma, etc.) is
**optional**; it is only a picture of what `screens.md` already says. No design file? `screens.md`
alone still carries the UX. A repo with no UI at all (a backend or a library) says so in
`AGENTS.md` and has no `screens.md`.

### B. North-stars — the frozen dream, never a build target

The original vision docs. Read them for context. **Never build against them.** Never reconcile a
living record *toward* one. A north-star is a museum, not a plan. Keep north-stars in
`z.misc/superseded/`. A north-star may share a file name with a living record (`0.PRD.md`,
`1.architecture.md`); only the ones at the repo root are living records.

### C. Operational ledgers and runbooks — updated whenever a feature touches them

These carry the plain facts that keep the product running:

| Doc | What it holds |
| --- | --- |
| `STACK.md` | Which account owns each service, its plan, its cost, where each secret is stored and how it is rotated. The owner's ledger, **not** wiring. |
| `DEPLOYMENT.md` | Real refs, URLs, steps, and gotchas for pushing a change out. |
| `feature-contracts.md` | The module seams and invariants of each shipped feature. **Repo root, append-only.** One `# Feature Contracts: <name> (status)` section per feature, each with `## Module Seams` and `## Invariants`. Written in the blueprint or build; a shipped section is never rewritten. |
| `ROADMAP.md` | The order of things: shipped, next, parked, and **why in that order**. |
| per-module `AGENTS.md` | The rules for that one module: its low-level design and what it owns. |
| `x.product/c.sop-<role>.md` | The human process for one role (operator, admin, support, etc.). |
| `x.product/a.product-principles.md` | The durable *why* behind product and design calls. Project memory that travels with the repo. |

### D. PRDs, drafts, and build files — history, not current state

These are the paper trail of *thinking* and *doing*. Once written for a moment in time, they stay
frozen as a record of that moment.

- `x.product/z.draft-<name>.md` — a raw idea, still cooking.
- `x.product/prd-<name>.md` — a PRD that's ready to be built (or being built).
- `x.product/00.build-<feature>.md` — the **active build file** for the one feature in flight (§4).
  The `00.` prefix lists it first. Its existence *is* the "this feature is not done yet" flag.
- `x.product/archive/` — finished PRDs and build files, prefixed `DONE-`.

---

## 3. Naming, at a glance

The prefix tells you the state without opening the file.

| Prefix / location | Meaning |
| --- | --- |
| `z.draft-…` | Idea, not ready. |
| `prd-…` | PRD ready to build. |
| `00.build-…` | Feature in flight right now. |
| `x.<date>.…` | A dated snapshot (for example a batch of small enhancements). |
| `archive/`, `DONE-…` | Finished. Do not edit. |
| `z.misc/superseded/` | North-star or abandoned vision. Read-only, never build against it. |
| `a.` … `c.` … | Sort-order helpers so the important files are listed first in the folder. |

The product-docs folder is `x.product/` by default. If this repo uses another name, say so in
`AGENTS.md` and use it everywhere this file says `x.product/`.

---

## 4. The lifecycle — idea to shipped

This is the one path a feature travels. The **`builders-blueprint`** skill is the engine for the
middle of it.

```
draft    →   PRD    →   BLUEPRINT SESSION   →   build        →   reconcile        →   archive
z.draft-x    prd-x      (the valve)             00.build-x       (docs updated)       archive/DONE-…
```

1. **Draft.** A rough idea is written as `z.draft-…`. No rules, just thinking.
2. **PRD.** When the idea is real, it becomes a `prd-…` file: one feature, its problem, goals,
   requirements and acceptance criteria.
3. **Blueprint session.** Run the `builders-blueprint` skill on that one PRD. This is the heart of
   the discipline. It:
   - checks the repo and the PRD are sane,
   - designs the system shape, the **UX screens**, the data model and the interfaces,
   - runs a risk check,
   - and writes every doc change it *will* need into the build file. It does **not** touch the
     living records yet.
4. **Build.** Code the feature. All planned doc changes ("deltas") are kept inside
   `00.build-<feature>.md` and edited alongside the code. UX changes are written into the build
   file's screen list changes. If the repo also keeps a visual design file, screens are assembled
   in a temporary copy of it, never in the main one.
5. **Reconcile (the gate).** Only when the build is complete and tests pass does the blueprint's
   final step merge the deltas into the living records (`0.PRD.md`, `1.architecture.md`,
   `screens.md`), update any ledgers the feature touched (`STACK.md`, `DEPLOYMENT.md`,
   `ROADMAP.md`, module `AGENTS.md`, SOPs), and, if the repo keeps a visual design file, replace
   the main one with the temporary one. Nothing goes stale, because reality and the docs move
   together.
6. **Archive.** The `00.build-…` file moves to `x.product/archive/` with a `DONE-` prefix. The
   feature is done.

**Why this shape:** the blueprint is the only valve, so it's the only place that writes the living
records, so the docs can never quietly drift away from the app.

---

## 5. The hard rules

These are the ones that get broken and cause pain. Follow them exactly.

**Living records are current; PRDs and build files are history.** Edit `0.PRD.md`,
`1.architecture.md`, `screens.md` and `AGENTS.md` **in place**. **Never** rewrite a PRD or a
`DONE-`/archived doc to match new reality. It's a record of a moment; leave it.

**Only the blueprint writes the living records, one feature at a time.** The three living records
and the main design file are written **inside a blueprint session or its build, for one feature**.
Reading them (screenshotting the app, rendering the design) is always safe, in parallel. PRDs
describe their UX in words and sketches in their **own** files; they never touch a living record.

**Update docs only at successful completion.** Deltas wait in the build file. The living records
change at the reconcile gate, not mid-build. If there's a design file, don't merge it until tests
pass.

**Never advance a status before the owner has read every line and said yes.** Do not move a PRD
or roadmap item to "ready" or "done" on your own. Confirm first.

**Say what a code or ID means, in words.** Don't cite a bare requirement ID or section number.
Write the rule in plain language, and put the file or section at the end for whoever needs to open
it.

**Record durable product decisions where they belong:** in `x.product/a.product-principles.md`
(product and UX judgment) or the module `AGENTS.md` (code conventions). Keep each entry short: the
principle, why, how to apply. Delete ones that turn out to be wrong.

**Ship, then document, in that order, and only commit when told.** Default order: code → deploy to
staging → test → iterate → *then* commit → deploy to production. "Deploy to staging" never means
"commit". Reconcile the docs as part of finishing the feature, not before it works. *(Replace this
order with your team's real one during onboarding.)*

---

## 6. Where things are (quick map)

- Start here, in this order: `0.PRD.md`, `1.architecture.md`, `x.product/design/screens.md`, then
  the root `AGENTS.md` and the per-module `AGENTS.md` files.
- Order and what's next: `ROADMAP.md`.
- Accounts, costs, secrets: `STACK.md`. How to deploy: `DEPLOYMENT.md`.
- Product memory (the *why*): `x.product/a.product-principles.md`.
- Ideas and their trail: `x.product/` (`z.draft-…`, `prd-…`, `archive/`).

---

## 7. The one thing to remember

*Change reality and the docs describing it in the same motion, at one valve: the blueprint
session. Everything else here is naming and housekeeping around that.*

---

# Part 2 — Firmware and device repos

Skip this part if your product has no firmware.

The idea for firmware:

> **Firmware is not designed in the firmware repo. Every change is decided in a feature build in
> the main repo, written from that session, and recorded in the firmware repo. Each device type has
> exactly one current firmware, and every version that was ever finished stays frozen, so you can
> always see the exact code a device runs.**

In this part:

- **Main repo** is the repo that runs the blueprint sessions (the cloud, app or backend repo).
  Written below as `<main-repo>`.
- **Firmware repo** is the separate repo that holds device firmware. Written below as
  `<firmware-repo>`.
- **Device type** is one kind of board (for example a hub, a sensor node, a motor controller),
  named exactly as the main repo names it.

## 8. Where firmware decisions are made

- **What the firmware must do** is decided in a feature build in the main repo
  (`00.build-<feature>.md`). That build file has a **"Firmware changes"** section: which device
  types change, what each must do differently, and the new version number for each.
- **What goes over the wire** (messages between devices and the cloud, and between devices) is
  defined only in the main repo's **device contract doc** (for example
  `x.product/device-contract.md`). The firmware repo never keeps its own copy of the contract.
- **What each firmware version can do, as the main repo sees it,** is recorded in the main repo's
  **firmware-versions table**: the place in the main repo's code that lists, per device type and
  major version, how the cloud must treat that device.
- **The firmware repo holds** the firmware code, one folder per device type, and `VERSIONS.md`, the
  record of every version.

---

## 9. Folder layout and names (firmware repo)

```
<device_type>/
  <device_type>_CURRENT/                       the newest firmware; the only folder anyone edits
    <device_type>_CURRENT.ino                  (or your toolchain's main file)
    contract.h                                 and any other files the firmware needs
  z.archive/
    <device_type>_v<major>_<minor>_<patch>/    one frozen folder per finished version
VERSIONS.md                                    every version of every device type (§11)
AGENTS.md
discipline.md
```

**Device type names are the main repo's names**, exactly as the device reports itself. Keep a
table of them in the firmware repo's `AGENTS.md`, for example:

| Folder | Device | Name in the main repo |
| --- | --- | --- |
| `hub_wifi/` | Wi-Fi hub that connects sensor nodes to the cloud | hub type `wifi` |
| `sensor_node/` | Battery sensor board, paired to a hub | node type `sensor_node` |

- **A new device type gets its name in the main repo first** (in the device contract doc), then its
  folder here.
- **A folder is created when its first firmware arrives.** No empty placeholders.
- **The version is set in one place** in each firmware folder (for example `FW_VERSION` in
  `contract.h`).
- **Each firmware folder is self-contained.** If two device types share a file (for example a
  packet layout), each keeps its own copy. The copies must be identical and carry the same layout
  version.

**If you build with the Arduino IDE:**

- **The `.ino` file has the same name as its folder.** The Arduino IDE needs this to open the
  sketch.
- **The device type is in every sketch name** (`hub_wifi_CURRENT.ino`, not `CURRENT.ino`), so the
  IDE's title bar shows which board you are about to flash.
- **Archive names use underscores in the version** (`v4_0_0`, not `v4.0.0`), because dots in a
  sketch name cause trouble in the Arduino IDE.
- The Arduino IDE builds a sketch from a copy of its own folder, so a file outside the folder can't
  be included. That is why each folder is self-contained.

---

## 10. Version numbers

Each device type has its **own** version number, `major.minor.patch`. A device reports it to the
cloud when it comes online. Versions of different device types are unrelated: `sensor_node 1.2.0`
has nothing to do with `hub_wifi 1.2.0`.

| Bump | When |
| --- | --- |
| **major** | Something else must treat the device differently: the main repo needs a new row in its firmware-versions table, or another device type must change to talk to it. |
| **minor** | New behaviour that needs no change anywhere else. |
| **patch** | A fix. Same intended behaviour, same messages. |

- **A version number means one exact piece of code.** Once a version is bench-tested, it is frozen.
  Any change after that, however small, is a new version.
- **While a version is still being written**, changes stay under the same number. Flashing a bench
  device during development is fine; bench devices are expected to run unfinished code.
- **Firmware that reports no version** is recorded as `0.0.0`.

---

## 11. `VERSIONS.md` — the record of every version (firmware repo)

One file at the firmware repo's root, with a section per device type and an entry per version,
newest first.

```
## hub_wifi

### 2.0.0 — writing
- **Date started:** 2026-10-24
- **From:** Claude, main repo build `00.build-<feature>.md` | or: hardware developer, received 2026-10-24
- **What changed:** in plain words, what the device now does differently
- **Works with:** device contract v2 | for a paired device: hub_wifi 2.x
- **Devices:** none yet | DEV-0007 (bench) | DEV-0007, DEV-0012
- **Folder:** `hub_wifi/hub_wifi_CURRENT/` | `hub_wifi/z.archive/hub_wifi_v2_0_0/`
```

**Statuses**, in order:

| Status | Meaning |
| --- | --- |
| `writing` | Still being changed. Only ever one per device type, and it is always `_CURRENT`. |
| `bench-tested` | Passed on a bench device. Frozen from here on; tagged in git. |
| `in the field` | Running on at least one real customer device. |
| `retired` | No device runs it any more. |
| `dropped` | Never left the bench. Kept for the record. |

**How the file is maintained:**

- **Add an entry** when a new version starts (§12, step 2), with status `writing`.
- **"What changed" and "From" are written once** and not rewritten later. If the description was
  wrong, add a correction line under it; don't replace it.
- **Status, Devices and Folder are kept current** as the version moves along.
- **Never advance a status before the owner says so.** Claude proposes; the owner confirms.
- **Update Devices whenever a device is flashed.** If fielded devices can't be updated over the
  air, this is the only place that says which old versions still have to be supported.
- **Mark a version `retired`** only when Devices is empty, and then tell the main repo: its
  firmware-versions row may be removable.

---

## 12. The lifecycle of one firmware change

### From a main-repo build (the normal path)

1. The main repo's build file has a **"Firmware changes"** section saying which device types
   change, how, and the new version for each.
2. **Start the version** (once per device type, before its first edit):
   - copy `<device_type>_CURRENT/` to `z.archive/<device_type>_v<old version>/`, renaming the main
     file to match. If the old version was never bench-tested, its entry becomes `dropped`;
   - set the new version number in `_CURRENT/`;
   - add the new entry to `VERSIONS.md` with status `writing`.
3. **Edit `_CURRENT/`** from the main-repo session, alongside the main repo's code.
4. **Bench test.** The owner flashes a bench device and confirms. Status → `bench-tested`, commit,
   tag.
5. **Field.** The owner flashes real devices. Status → `in the field`, Devices updated.
6. **Main-repo reconcile.** In the main repo, at its normal reconcile step: the firmware-versions
   table, the device contract doc and `ROADMAP.md`.

### From an outside hardware developer

1. **Archive the current version** exactly as in step 2 above.
2. **Put their files into `_CURRENT/`**, renaming their main file to `<device_type>_CURRENT`.
   Don't change their code while doing this.
3. **Record it:** a `VERSIONS.md` entry with "From: hardware developer, received <date>" and the
   version their code reports. If it reports none, or a number already used, ask the owner what to
   call it; don't change it silently.
4. **Check it against the device contract doc** before it goes on any device: every message it
   sends and accepts, and whether its major version has the right row in the main repo's
   firmware-versions table. Report differences to the owner; don't fix them in their code without
   asking.
5. Continue from the bench test (step 4 above).

---

## 13. Git (firmware repo)

Git and the archive folders are both kept, for different reasons:

- **Git** keeps every edit, including those made while a version was `writing`.
- **Archive folders** let anyone open and flash an old version straight from their IDE, and let a
  main-repo session read old firmware without checking anything out.

Rules:

- **Tag every bench-tested version** as `<device_type>/v<major>.<minor>.<patch>` (for example
  `hub_wifi/v2.0.0`). The tag points at the commit that was bench-tested.
- **Commit only when the owner says so.**
- **Never rewrite history** (no force-push, no amending a tagged commit).

---

## 14. The hard rules for firmware

- **Only `_CURRENT/` folders are edited.** Anything under `z.archive/` is never changed. It is the
  record of what a device runs.
- **One version in progress per device type.** Finish or drop it before starting the next.
- **A bench-tested version is frozen.** A further change is a new version.
- **Never change a message a device sends or accepts unless the device contract doc changes in the
  same build.** The wire format is defined in the main repo only.
- **A major bump means the main repo must know.** Every new major version needs a matching row in
  the main repo's firmware-versions table, written in the same build.
- **Never advance a status before the owner says so.**
- **Record every flash** in Devices.

---

## 15. What to leave in `AGENTS.md`

The **last step** of onboarding writes the matching block below into the root `AGENTS.md`. Keep it
short; the full detail stays in this file. Adjust paths to match wherever onboarding actually put
things.

### A. Every main repo

> ### Documentation discipline
>
> This repo keeps its docs honest with one rule: **change the app and the docs that describe it in
> the same motion, at one valve — the blueprint session (`builders-blueprint` skill).** Full
> reference: `discipline.md`.
>
> **Living records — always current, edited in place** (no dates, no history inside):
>
> - `0.PRD.md` — what features exist.
> - `1.architecture.md` — how it's built.
> - `x.product/design/screens.md` — what the user sees. **Required** if the repo has a UI. Truth in
>   words; a design file (Pencil, Figma) is an optional picture of it.
>
> **Feature seams — `feature-contracts.md` (repo root, append-only):** one section per shipped
> feature (`## Module Seams` + `## Invariants`). Add a section in the build; never rewrite a
> shipped one.
>
> **History — frozen, never rewritten:** PRDs (`x.product/prd-*.md`), build files
> (`x.product/00.build-*.md`), and anything under `x.product/archive/` (`DONE-*`).
>
> **Hard rules:**
>
> - Only a blueprint session or its build writes the living records, one feature at a time.
> - Update docs at the reconcile gate (build done, tests pass), not mid-build.
> - Never rewrite a PRD or a `DONE-` file to match new reality. It's history.
> - Never advance a status to "ready" or "done" before the owner reads it and says yes.
> - Ship, then document: <your team's order, e.g. code → staging → test → commit → production>.
>   Commit only when told.

### B. Add to a main repo that has a firmware repo

> ### Firmware
>
> Firmware is in `<firmware-repo>`, one folder per device type. When a feature changes what a
> device does, its build file gets a **"Firmware changes"** section (device types, what changes,
> new version for each), and the firmware is edited from this session following
> `<firmware-repo>/discipline.md` (Part 2): archive the current version, bump the version number,
> add a `VERSIONS.md` entry, then edit `<device_type>_CURRENT/`. Messages between devices and the
> cloud are defined only in `<device contract doc>`. At reconcile, update `<firmware-versions table>`
> for any new major version.

### C. A firmware repo

> ### Firmware discipline
>
> Firmware changes are decided in a feature build in `<main-repo>` and written from that session.
> Full reference: `discipline.md`, Part 2.
>
> - One folder per device type, named with the main repo's name for it: <list the device types>.
>   Inside: `<device_type>_CURRENT/` (the only folder edited) and
>   `z.archive/<device_type>_v<x>_<y>_<z>/` (frozen).
> - `VERSIONS.md` records every version: status (`writing` → `bench-tested` → `in the field` →
>   `retired`, or `dropped`), what changed, where it came from, what it works with, which devices
>   run it.
> - Before the first edit of a new version: archive `_CURRENT/`, bump the version number, add a
>   `writing` entry.
> - A bench-tested version is frozen; any further change is a new version.
> - The wire format is defined only in the main repo's device contract doc. A new major version
>   needs a matching row in the main repo's firmware-versions table.
> - Never advance a status, commit or tag before the owner says so.
