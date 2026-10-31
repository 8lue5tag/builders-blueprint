# Builder's Blueprint

**Design every feature with Claude before it's built, the user's experience and the architecture
together, and keep your product's docs true after it ships.**

Builder's Blueprint is a [Claude Code](https://claude.com/claude-code) skill. You give it one
feature PRD. Claude works through it with you as a senior engineer and design partner, one phase at
a time, stopping for your approval at every step. The result is a build plan any developer, human
or AI, can pick up cold. When the build passes its tests, the same skill updates your product's
living docs so they match what shipped.

It also sets your repos up for this way of working. A single file, `discipline.md`, defines how docs
are named, where they go, and when they may change. It covers app, backend and firmware repos alike.

---

## Who this is for

You know what your users need and how your product's pieces should fit together, but you don't
write code by training. You might be a product manager, a founder, a designer or a domain expert,
using Claude Code as your developer.

**This skill expects two things from you:**

- **User empathy.** You notice when a flow leaves someone stuck, when an error message would
  confuse them, or when someone on a poor connection would give up. The skill treats screens and
  their states (empty, loading, error, offline) as seriously as the backend, and it asks you to
  judge them. At least one risk in every session is about how real people will behave.
- **Architectural thinking.** You can reason about which parts talk to which, which part should own
  which data, and what happens when something fails halfway, even if you couldn't write the code
  for any of it. The skill asks you to make those calls.

**The skill does not expect tech know-how.** You don't need to know a framework, write SQL or set
up servers. Claude explains every option in plain words, recommends one, and tells you what you'd
lose with each, so you can decide.

**It's probably not for you if:**

- You're an experienced engineer who wants code fast. Stopping for approval after every phase will
  feel slow.
- You want Claude to make the decisions. The skill stops and asks at every checkpoint.
- You're building a throwaway prototype. Keeping the docs disciplined pays off over months, not an
  afternoon.

---

## What it does

- **Designs one feature at a time, before code.** System shape, screens and their states, technical
  decisions, data, interfaces and risks, each one explained and approved by you.
- **Keeps your docs honest.** Your product doc, architecture doc and screen list describe only what
  has actually shipped. Planned changes wait in the build plan and are merged in only when the build
  passes. The docs never drift away from the product.
- **Survives lost context.** The build plan records which phase you reached and what's already
  decided, so a fresh Claude session continues exactly where the last one stopped.
- **Onboards any repo** with one sentence, and later checks it against the latest rules.

---

## One discipline file for every repo

Everything the method needs from a repo is in **one file**:
[`templates/discipline.md`](templates/discipline.md). Onboarding copies it into your repo and
reorganizes your docs to match.

- **Part 1 applies to every product repo.** It defines the four kinds of docs: living records,
  north-stars, ledgers, and history. It also sets the file naming that shows a doc's state at a
  glance, the path from idea to shipped, and the hard rules.
- **Part 2 applies only if your product includes devices.** Skip it otherwise.
- **The same file goes into every repo**, with a version line at the top. Claude can then tell an
  outdated copy from one you edited on purpose, and offer updates.

---

## Works for hardware products too

If your product includes devices, such as sensors, controllers or gateways, the same method covers
the firmware. The firmware is never designed separately.

- **Firmware changes are decided in your main repo's blueprint session.** When a feature changes
  what a device does, the build plan gets a "Firmware changes" section: which device types change,
  how, and the new version number for each. The build edits the firmware from that same session.
- **Messages between devices and the cloud are defined in one place only**: a contract doc in your
  main repo. The firmware repo never keeps its own copy, so the two sides can't disagree.
- **Every finished firmware version is frozen.** Each device type has one current folder that is
  edited, plus a frozen copy of every version that was ever bench-tested. You can always open the
  exact code a device runs.
- **`VERSIONS.md` tracks every version and every device.** It records what changed, where the code
  came from (a build or an outside hardware developer), what it works with, and which physical
  devices run it. That matters when fielded devices can't be updated over the air.

Onboard your firmware repo the same way as any other repo, and answer "firmware repo" when Claude
asks.

---

## Get started

**→ Follow [ONBOARDING.md](ONBOARDING.md)** for the full walkthrough. The short version:

**1. Install the skill** (once per computer):

```bash
# macOS / Linux
git clone https://github.com/8lue5tag/builders-blueprint.git ~/.claude/skills/builders-blueprint
```

```powershell
# Windows PowerShell
git clone https://github.com/8lue5tag/builders-blueprint.git "$HOME\.claude\skills\builders-blueprint"
```

Restart Claude Code.

**2. Onboard your repo** (once per repo). Open Claude Code in your project and say:

```
Onboard this repo to Builder's Blueprint
```

Claude asks a few questions, shows a plan, and changes nothing until you approve it.

**3. Take a feature from idea to shipped:**

| Step | What you say to Claude |
| --- | --- |
| Design the feature | `Run the blueprint session on x.product/prd-<name>.md` |
| Build it (new session) | `Build, follow x.product/00.build-<name>.md` |
| Update the docs once tests pass | `Build is green, run the reconcile` |
| Resume after lost context | `Resume the blueprint session from x.product/00.build-<name>.md` |
| Check a repo against the latest rules | `Check this repo's discipline against Builder's Blueprint` |

---

## How a feature travels

```
draft    →   PRD    →   blueprint session   →   build        →   reconcile        →   archive
z.draft-x    prd-x      (design, with you)      00.build-x       (docs updated)       archive/DONE-…
```

- **Draft and PRD.** You write ideas whenever you like, in any order. When one is ready, it becomes
  a PRD.
- **Blueprint session.** You and Claude design one feature. Every planned change to your docs is
  written into the build plan, not into the docs themselves.
- **Build.** A fresh session builds from the plan.
- **Reconcile.** Only when tests pass, the planned changes are merged into the living records.
- **Archive.** The build plan moves to the archive. While it's in `x.product/`, the feature isn't
  finished.

---

## The blueprint session

Each phase ends with a checkpoint. Claude names the 2–3 things most likely to go wrong with that
phase's decisions, then waits for your approval.

| Phase | What happens |
| --- | --- |
| **-1. Repo health** | Checks the repo's structure, and works out where screens are defined: in code, in a maintained design file, nowhere yet, or no UI at all. |
| **0. PRD sanity check** | Flags ambiguities, hidden complexity, missing decisions, scale assumptions, and which parts and screens the feature touches. |
| **1. System shape** | Sketches the main parts and how they talk, then decides where this feature's boundaries are. |
| **1.5. Screens** | Lists the screens affected and designs new ones, including their empty, loading, error, success and offline states, and how users move between them. |
| **2. Tech decisions** | 2–3 options per decision (database, hosting, sign-in, background jobs, etc.), each explained, with a recommendation tied to the PRD. |
| **3. Data** | What information the feature stores and how it connects, stress-tested for status changes, deletions and speed. |
| **4. Interfaces** | How the parts talk to each other and to outside services. |
| **5. Risk radar** | The top risks (at least one about how real users will behave), the hardest parts, and what is deliberately not being built. |
| **6. Outputs and reconcile** | Writes the build plan and related docs, and the checklist that updates every living doc once the build passes. |

## What a session produces

- **An updated feature PRD**, including a "Screens & Flows" section.
- **A build plan (`00.build-<feature>.md`)** with ordered steps, each with a "done when" condition
  and a way to check it. It also carries every planned change to your docs, and a progress block so
  a new session can resume.
- **Module notes (`AGENTS.md` per module)**: what each part owns, offers and depends on.
- **A section in `feature-contracts.md`**, when the feature crosses module boundaries.
- **SOPs** (standard operating procedures) for any human process the feature introduces, or an
  explicit "No new SOPs".
- **Draft PRDs** for good ideas that came up but are out of scope.

---

## Project layout after onboarding

```
your-project/
├── CLAUDE.md                     # "@AGENTS.md", so Claude Code loads the rules
├── AGENTS.md                     # project rules, including the discipline block
├── discipline.md                 # the full discipline (from this skill's template)
├── 0.PRD.md                      # living record: what features exist
├── 1.architecture.md             # living record: how it's built
├── feature-contracts.md          # how modules connect, per shipped feature (append-only)
├── STACK.md                      # services, plans, costs, where secrets are stored
├── DEPLOYMENT.md                 # deploy steps and gotchas
├── ROADMAP.md                    # shipped / next / parked, and why
├── x.product/
│   ├── a.product-principles.md   # the lasting reasons behind product decisions
│   ├── c.sop-<role>.md           # human processes, one per role
│   ├── design/screens.md         # living record: every screen + navigation map
│   ├── z.draft-<name>.md         # ideas
│   ├── prd-<name>.md             # PRDs ready to build
│   ├── 00.build-<feature>.md     # the feature being built now
│   └── archive/                  # DONE- PRDs and build plans
└── z.misc/superseded/            # original vision docs: read, never built against
```

A firmware repo looks different: one folder per device type and a `VERSIONS.md`. See Part 2 of
[`templates/discipline.md`](templates/discipline.md).

---

## What's in this repository

| Path | What it is |
| --- | --- |
| [`SKILL.md`](SKILL.md) | The skill's entry point. Claude reads it to decide when to use the skill and how. |
| [`phases/`](phases/) | One file per phase of the blueprint session. |
| [`handoffs.md`](handoffs.md) | How sessions hand over and resume. |
| [`onboarding/onboard-repo.md`](onboarding/onboard-repo.md) | The steps Claude follows to onboard or check a repo. |
| [`templates/discipline.md`](templates/discipline.md) | The one discipline file copied into every onboarded repo. |
| [`ONBOARDING.md`](ONBOARDING.md) | The step-by-step guide for people. |

**Updating:** run `git pull` in `~/.claude/skills/builders-blueprint`, then restart Claude Code.
Then say "Check this repo's discipline against Builder's Blueprint" in each onboarded repo.

**Contributing:** changes to the rules go in `templates/discipline.md`. Raise its version line, so
onboarded repos can tell they're out of date.

---

## License

[MIT](LICENSE).
