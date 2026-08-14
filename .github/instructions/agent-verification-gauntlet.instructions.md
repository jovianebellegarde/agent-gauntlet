---
description: Pre-publish gauntlet — craft/smell review + project checks before commit/push
alwaysApply: true
---

# Agent verification gauntlet (standing)

**North star:** code that is easy to understand *and* still proven. Agents do not get trust from velocity — they pass this gauntlet before publish.

Process inspiration (Uncle Bob–style constraints): surround agent output with checks. Rubric: `swe-principles.mdc` (durable craft + Contieri-inspired smells). Neither book is copied verbatim.

## When it fires

Owner asked for **`commit`**, **`push`**, or a **PR** — or the agent is about to run those after an explicit ask.

Do **not** wait until PR time to verify. Review + project checks run **before** `git commit` / `git push`. PR later records evidence (re-run if the branch moved).

## Step 1 — Craft + smell review (automatic)

Against the **pending diff** (`git diff` / staged), walk `swe-principles.mdc`:

1. Durable craft (names, focused units, obvious intent, no surprise side effects, simplicity)
2. Contieri-inspired smells (long method, primitive obsession, feature envy, shotgun risk, flag soup, noise comments, premature abstraction)
3. Change discipline (minimal diff, conventions)

**Behavior:**

| Outcome | Do |
|---|---|
| Clear, local issue | **Fix** before commit (still minimal diff; no silent large refactor) |
| Ambiguous / large structural move | **Stop and ask** (owner-profile decision protocol) |
| Pass | Proceed to Step 2 |

Report briefly to the owner (bullets: checked / fixed). Teach one line if a smell is new this session — then continue.

If a “clean” fix would make the code harder to follow, prefer the simpler readable version.

## Step 2 — Project verification surface (automatic)

1. **Discover** the project’s checks (in order): project docs / README / `TESTING.md` / CI workflow local equivalents / common defaults (`pytest`, `uv run pytest`, `npm test`, package scripts). Prefer documented commands over guessing.
2. **Run** every automatable check relevant to the change.
3. **Failures block publish** until fixed or the owner explicitly overrides.
4. **Skip only with a stated reason** (no test harness yet; docs-only change and no docs lint; etc.).

**Not required by default:** mutation testing, Gherkin, coverage theater — only if a project opts in.

One short teach line when running is enough (what the check proves), then run it.

## Step 2b — README honesty (automatic)

Follow `readme-honesty.mdc`: if the pending change affects what strangers learn from the public README(s) (versions, install pins, flags, phases, privacy), **update those README(s) in the same change** — or state that they remain accurate. Do not advertise unpublished package versions as if they were on the registry.

## Step 3 — PR evidence (existing)

When opening or updating a PR, follow `pr-test-plan.mdc`: mark `[x]` with evidence; leave only true *(owner)* items unchecked. Commit-time verification should already have happened; re-run if needed so the Test plan stays honest.

## Mentorship preserved

This gauntlet does **not** mean “never read agent code.” Upskilling / teach-first still apply. Constraints raise confidence; understanding remains the goal.

## Relationship

| Rule / hook | Role |
|---|---|
| `swe-principles.mdc` | What “easy to understand” means (craft + smells) |
| **This file** | When/how to review + run tests before publish |
| `readme-honesty.mdc` | Keep public README(s) true when shipping visible changes |
| `pr-test-plan.mdc` | PR body evidence |
| `hooks/session-start-teach.sh` | Session reminder (upskilling brief from rule file) |
| `hooks/ask-before-git-publish.sh` | Ask at commit/push/`gh pr create`; **deny** main on product repos unless `ALLOW_MAIN_COMMIT`; **`cursor-rules` may use main** |
| Project docs / CI | Which commands are the verification surface |
