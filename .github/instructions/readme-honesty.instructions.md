---
description: Keep public README honest — update when shipping user-visible changes
alwaysApply: true
---

# README honesty (standing)

**Duty:** Public README(s) must match what the repo actually ships. Do not leave install pins, version numbers, phase blurbs, CLI flags, or privacy claims stale after a feature lands.

This is part of the pre-publish gauntlet — same spirit as tests: strangers (and future-you) trust the front page.

## When it fires

Any change that affects what a careful reader would learn from the top-level **README** (and package/extension READMEs if the project has them), including:

| Change type | README usually needs |
|---|---|
| New CLI flag / command | Commands table or Getting started |
| Version bump (`pyproject`, extension `package.json`) | Install pins — **honest about PyPI vs this checkout** |
| New opt-in feature (LLM, telemetry, etc.) | What / default / privacy one-liner |
| Phase / roadmap status shift | Roadmap blurb if the README has one |
| Ethics / data-boundary change | Ethics & privacy section must stay true |
| New install path (script, marketplace) | Try in 60 seconds / Install |

## What the agent must do

1. **Before commit/push/PR** (with the verification gauntlet): skim the pending diff and ask — “Would the public README still be true?”
2. If not → **update the README in the same change** (minimal diff; no marketing rewrite).
3. If README is fine → one line is enough: “README still accurate (no user-facing surface change).”
4. Prefer **honest pins** over aspirational ones (e.g. do not advertise `pip install pkg>=X` if PyPI has not published X yet — say PyPI version vs clone/`uv sync`).

## What not to do

- Do not invent features in the README that are not on the branch being published
- Do not dump internal handoff / career / private cockpit notes into public README
- Hooks **remind**; they do not auto-edit README (agent owns the file change)

## Relationship

| Rule / hook | Role |
|---|---|
| **This file** | When/how to keep README true |
| `agent-verification-gauntlet.mdc` | Runs this check before publish |
| `pr-test-plan.mdc` | PR may include a README-honesty checkbox when relevant |
| Publish / session hooks | Remind at the shell / session start |
