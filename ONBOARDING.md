# Onboarding your repo to Builder's Blueprint

This guide takes you from "never used it" to "first feature designed". Setting up a repo takes one
sitting of about 30–60 minutes, most of it reading Claude's plan and answering questions.

Not sure this is for you? Read [Who this is for](README.md#who-this-is-for) first.

---

## What you need

- **Claude Code**, installed and signed in. Any version: the terminal, the desktop app, or the
  VS Code extension.
- **Git**, so you can clone this skill. On Windows, install
  [Git for Windows](https://git-scm.com/download/win).
- **A project repo**: a folder with your product's code, ideally already tracked with git, so you
  can review and undo what onboarding changes.

You don't need to know how to code.

---

## 1. Install the skill (once per computer)

Open a terminal and run the command for your system.

**macOS / Linux:**

```bash
git clone https://github.com/8lue5tag/builders-blueprint.git ~/.claude/skills/builders-blueprint
```

**Windows (PowerShell):**

```powershell
git clone https://github.com/8lue5tag/builders-blueprint.git "$HOME\.claude\skills\builders-blueprint"
```

Restart Claude Code. To check, ask Claude **"what skills do you have?"**. You should see
`builders-blueprint` in the list.

---

## 2. Onboard your repo (once per repo)

Open Claude Code **in your project's folder** and say:

```
Onboard this repo to Builder's Blueprint
```

### What Claude asks you

Claude reads your repo first, then asks a few questions in one message, with its best guess filled
in for each. Usually you only confirm or correct:

1. **Is this your main product repo, or a firmware repo?**
2. **Where should product docs go?** The default is a folder called `x.product/`. If you already
   have a docs folder you like, keep it.
3. **Does your product have screens?** If yes: do you have a design file (Figma, Pencil, etc.), and
   is it kept up to date with what's actually shipped, or is it more of a vision?
4. **Do you also have a firmware repo?** Only if your product includes devices.
5. **How do changes reach your users?** For example: "code, test on staging, commit, then
   production". And when Claude is allowed to commit.

### What Claude does

1. **Copies the discipline file** into your repo as `discipline.md`. It holds the rules for how
   your docs are named, where they go, and when they may change.
2. **Sorts every doc you already have** into four groups:
   - **Living records** describe what your product is today.
   - **North-stars** are your original vision. They're kept for context, never built against.
   - **Ledgers** are running facts: services and costs, deploy steps, roadmap.
   - **History** means PRDs and finished build plans, which are never rewritten.
3. **Shows you one plan**: every move and rename, and why. It also asks about anything that
   doesn't fit, or two docs that both claim to describe the same thing.
4. **Waits for your "yes".** Nothing changes before that. Push back on anything you disagree with.
5. **Creates what's missing**, as drafts for you to approve:
   - `0.PRD.md`, the features that exist today;
   - `1.architecture.md`, how the product is built;
   - `x.product/design/screens.md`, every screen and how you move between them (built from your
     code or your design file, never invented).
6. **Writes a short rules block** into `AGENTS.md`, and makes sure `CLAUDE.md` loads it. From now
   on, every Claude session in this repo follows the rules without being reminded.

### What you do afterwards

- **Read the new records.** They're Claude's best reading of your product. Correct anything wrong:
  these files are now the truth that every feature is designed against.
- **Review the changes and commit them.** Claude doesn't commit unless you tell it to. If you use
  git, `git status` shows everything that changed.

---

## 3. Design your first feature

### Write the PRD

A PRD (product requirements document) describes one feature: the problem, who it's for, what it
must do, and how you'll know it works. Save it as `x.product/prd-<name>.md`. Half-formed ideas go
in `x.product/z.draft-<name>.md` until they're ready.

### Run the blueprint session

```
Run the blueprint session on x.product/prd-<name>.md
```

Claude works through the phases in order:
1. checks your repo and the PRD;
2. sketches how the system fits together;
3. designs the screens and their states;
4. makes the technical decisions;
5. designs the data;
6. defines how the parts connect;
7. names the risks;
8. writes the outputs.

After each phase, Claude stops, names the 2–3 things most likely to go wrong, and waits for your
approval.

**What you're there for:**

- **The user's side.** Is any screen confusing? What happens when the list is empty, the connection
  drops, or a payment fails? Claude raises these; you judge them.
- **The shape.** Does it make sense that this part owns this data? What if this step fails halfway?
- **The decisions.** Claude gives options with a recommendation and what you'd lose with each. Ask
  "why" as often as you like. Any technical term should be explained the first time it's used. If
  one isn't, ask.

The session ends with a build plan: `x.product/00.build-<name>.md`.

### Build it

Start a **new** Claude Code session (so it starts clean) and say:

```
Build, follow x.product/00.build-<name>.md
```

### Finish it

When the build works and its tests pass, say:

```
Build is green, run the reconcile
```

Claude updates your living records to match what shipped, and moves the build plan to
`x.product/archive/`. That's one feature done.

---

## If your product includes devices

Onboard **both** repos: your main repo, and your firmware repo. When onboarding the firmware repo,
answer "firmware repo" to the first question.

From then on, you never open a separate firmware session. When a feature changes what a device
does, the blueprint session in your main repo adds a "Firmware changes" section to the build plan,
and the build edits the firmware from there.

The firmware repo keeps:
- one current folder per device type;
- a frozen copy of every finished version;
- `VERSIONS.md`, which says which devices run which version.

---

## Keeping a repo up to date

The discipline file improves over time. Once in a while, in each onboarded repo, say:

```
Check this repo's discipline against Builder's Blueprint
```

Claude compares your copy with the current template. It shows each difference in plain words,
tells you whether it's an improvement you don't have yet or a change you made on purpose, and
updates only what you approve.

To get the latest skill itself, run `git pull` in the skill folder (`~/.claude/skills/builders-blueprint`).

---

## Common questions

**Can I onboard a brand-new, empty repo?** Yes. Claude creates the folders and empty records, and
`screens.md` grows as screens ship.

**My repo has no screens (an API or a backend).** That's fine. Claude records that the repo has no
UI, and the screen design phase is skipped.

**I don't have a design file.** That's fine, and common. `screens.md` describes your UX in words,
and Claude can screenshot the running app.

**Claude changed `0.PRD.md` or `screens.md` in the middle of a build.** Ask it to move the change
into the build plan. Living records only change at the end, when the build passes.

**A session ran out of context halfway through.** Start a new one and say "Resume the blueprint
session from x.product/00.build-<name>.md". The top of the build plan records where you were and
what's already decided.
