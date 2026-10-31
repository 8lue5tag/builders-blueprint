# Onboarding mode — set up a repo, or check one that's already set up

Load this file when the user asks to onboard a repo ("onboard this repo to Builder's Blueprint",
"set up builders blueprint here") or to check an onboarded one ("check this repo's discipline
against Builder's Blueprint"). The template is `templates/discipline.md` in this skill's folder.

The same ground rules apply as in a blueprint session: explain in plain words, recommend, and
**change nothing until the owner approves the plan.** Never commit.

---

## A. Onboard a repo

### Step 1 — Look before asking

Read the repo quickly: the root files, any `AGENTS.md` / `CLAUDE.md`, the docs folders, and
whether there is UI code (pages, routes, screens) or firmware (`.ino`, `platformio.ini`, etc.).
If a `discipline.md` with a "Builder's Blueprint discipline" version line already exists, switch to
**B. Check an onboarded repo**.

### Step 2 — Ask the setup questions (one message)

Pre-fill each answer with your best guess from Step 1, so the owner mostly confirms:

1. **Role.** Is this a main product repo (app, backend, website), or a firmware repo?
2. **Product-docs folder.** Use `x.product/` (default), or keep an existing folder name?
3. **UI.** Does the product have screens? If yes: is there a design file (Pencil, Figma, etc.), and
   is it kept in sync with what's shipped, or is it aspirational?
4. **Firmware** (main repo only). Does the product also have a firmware repo? If yes: its path, the
   device contract doc, and where the main repo records what each firmware version can do.
5. **Device types** (firmware repo only). The device types, with the main repo's name for each,
   and the main repo's path.
6. **Ship order.** What is the team's real order from code to production (for example code →
   staging → test → commit → production)? When may Claude commit?

### Step 3 — Copy the template

Copy `templates/discipline.md` from this skill's folder to the repo root as `discipline.md`.
Fill in what the answers decide:

- the product-docs folder name, if not `x.product/`;
- the ship order in the hard rules (§5);
- in Part 2, the placeholders (`<main-repo>`, `<firmware-repo>`, the device contract doc, the
  firmware-versions table, the device type table) if firmware applies.

Leave the version line and the rest of the text as it is, so a later check can compare it with
the template. If Part 2 doesn't apply, leave it in; it is skipped by role.

### Step 4 — Run the alignment

Follow "How to use this file" (§0) in the copied `discipline.md`:

1. Sort every doc (main repo) or file (firmware repo) into its family or folder.
2. Present **one plan** as a table: current path → new path, and why. Include a list of docs that
   don't fit and any two docs competing to be the same living record, each with a question.
3. **Stop and wait for approval.** Apply only what was approved.
4. Never edit the contents of PRDs, `DONE-` files or archived firmware. Moving and renaming is
   fine.

### Step 5 — Create the missing records

**Main repo:**

- `0.PRD.md` — the features that exist today, drafted from the code and existing docs.
- `1.architecture.md` — the modules, how they connect, and the main technical decisions so far.
- `x.product/design/screens.md` — if the repo has a UI. Derive it as Phase -1, Part B of the
  blueprint describes (`phases/phase-repo-health.md`): from the code routes and pages, or from a
  maintained design file. Never invent screens.
- `STACK.md`, `DEPLOYMENT.md`, `ROADMAP.md`, `feature-contracts.md` — create short stubs only if
  the owner wants them now; otherwise the first blueprint session creates them when needed.

**Firmware repo:** `VERSIONS.md` with one entry per firmware folder. Ask the owner for anything you
can't read from the code (status, which devices run it). Firmware with no version is `0.0.0`.

Present every new record as a draft for approval before writing it. They describe what is real
today, so check each claim against the code.

### Step 6 — Seed `AGENTS.md` and `CLAUDE.md`

- Write the matching block from §15 of `discipline.md` into the root `AGENTS.md` (create it if
  missing): block A for a main repo, plus block B if it has a firmware repo; block C for a firmware
  repo. Replace every placeholder with real paths and names.
- Make sure the root `CLAUDE.md` contains `@AGENTS.md`. If `CLAUDE.md` holds real project rules,
  propose moving them into `AGENTS.md` first.
- **Paired repos:** onboarding a firmware repo means the main repo's `AGENTS.md` needs block B, and
  the other way round. Offer to write it; it's a change to another repo, so ask first.

### Step 7 — Wrap up

Tell the owner, in plain words:

- what was moved, renamed and created (with links);
- any open questions you parked;
- that nothing was committed, and what to review before they commit;
- **what to do next:** pick a PRD (or turn a draft into one) and say
  "Run the blueprint session on x.product/prd-<name>.md".

---

## B. Check an onboarded repo

Use this when the repo already has a `discipline.md` with a "Builder's Blueprint discipline"
version line, or the owner asks to check or update it.

1. **Compare versions.** Read the version line in the repo's copy and in `templates/discipline.md`.
   If the repo's copy has no version line, treat it as older than v1.0.
2. **Compare the text.** For each difference, decide which of these it is:
   - **a template change** the repo doesn't have yet;
   - **a local edit** the owner made on purpose (filled-in placeholders, a different folder name,
     the team's ship order, extra rules);
   - **unclear**.
3. **Present the differences** as a list. For each: what the difference is in plain words, which
   kind it is, and your recommendation (take the template's version, or keep the local one).
4. **Apply only what the owner approves.** Update the version line to the template's version once
   the template changes are taken.
5. **Check `AGENTS.md`.** If the approved changes affect the short block, propose the matching edit.
6. **Check the layout.** If the template's rules changed where files go, run Step 4 of section A
   for just those rules.

Never rewrite history while doing this: PRDs, `DONE-` files and archived firmware stay as they are.
