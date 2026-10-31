# Builder's Blueprint — rules for working on this repo

This repo **is** the `builders-blueprint` Claude Code skill. It is not a product repo, so the
documentation discipline does not apply to it.

## What's where

| Path | What it is |
| --- | --- |
| `SKILL.md` | The skill's entry point. Claude Code reads its header (name, description) to decide when to load it. |
| `phases/` | One file per blueprint phase, loaded by `SKILL.md` as each phase starts. |
| `handoffs.md` | How sessions hand over and resume. |
| `onboarding/onboard-repo.md` | Instructions Claude follows to onboard or check another repo. |
| `templates/discipline.md` | The discipline file that gets copied into other repos. |
| `README.md`, `ONBOARDING.md` | For people: what this is, who it's for, how to set it up. |

## Rules

- **Never apply `templates/discipline.md` to this repo.** It is a template for other repos. Don't
  create `x.product/`, `0.PRD.md`, `screens.md` or any other discipline file here.
- **When you change the rules in `templates/discipline.md`, raise its version line** (minor for
  clarifications and additions, major when a rule changes where files go or what an onboarded repo
  must do). Onboarded repos use the version line to tell whether they're out of date.
- **Keep everything generic.** No real project, company, device or person names in any file. Use
  placeholders like `<main-repo>` or neutral examples like `hub_wifi`.
- **Keep the `SKILL.md` header valid.** It must start with `---` on line 1, then `name:` and
  `description:` with no blank lines in between. The description must stay under 1024 characters
  and keep the trigger phrases people already use.
- **Keep `README.md` and `ONBOARDING.md` in step with the skill.** If a command, path or phase
  changes, update both.
- **Commit only when the owner says so.**
