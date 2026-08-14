---
description: PR test plan — agent must execute every automatable checkbox before/when opening a PR
alwaysApply: true
---

# Pull request test plan (standing)

When Joviane asks for a **pull request**, the agent owns the **Test plan** — not as empty checkboxes for her to redo later.

## Why

A PR Test plan is **evidence before merge**, not a TODO list dumped on the owner. Unchecked boxes mean “not yet proven.” If the agent can prove an item (CLI, pytest, curl, code-path check), the agent **must** do it and check the box with a short evidence note.

## Required workflow (every PR)

1. **Draft** Summary + Test plan with concrete checkboxes (commands / behaviors).
2. **Execute** every item the agent can run without a human-only UI click.
3. **Record** results: mark `[x]` and append one-line evidence (e.g. `57 passed`, `exit 0 ~1s`, `Ollama qwen2.5:7b`).
4. **Create or update** the PR body with those checked items (`gh pr create` / `gh pr edit`).
5. **Leave unchecked** only what truly needs the owner (e.ide click, paid API key she alone has, visual UX judgment). Label those *(owner)*.

## What “do as much as you can” means

| Do (agent) | Leave for owner only if needed |
|---|---|
| `pytest` / `uv run pytest` / project test command | Manual UI dogfood in the IDE |
| CLI dry-runs (`focus audit`, lint, build) | Paid third-party accounts without local creds |
| Unit/integration proofs of gates (flags, skip paths, validators) | Subjective “does this caption feel good?” |
| Code-path confirmation when a live UI click is redundant | Screenshot / accessibility eyeball |
| Update PR body checkboxes after runs | — |

**Never** open a PR with a full unchecked Test plan if those commands were runnable in the workspace.

## Timing

- Prefer: run tests **before** `gh pr create`, then open with boxes already checked.
- If the PR already exists: run the plan and **`gh pr edit`** the body — do not wait for Joviane to re-dogfood agent-runnable items.

## Teach briefly

When executing a Test plan, one short line is enough: what a checkbox proves (e.g. “opt-in gate = no LLM unless `--llm-captions`”). Then run it.

## Relationship

| Rule / hook | Role |
|---|---|
| **This file** | Canonical PR Test plan duty (all projects) |
| `agent-verification-gauntlet.mdc` | Craft/smell review + project checks **before** commit/push |
| `readme-honesty.mdc` | Keep public README(s) true when shipping visible changes |
| `swe-principles.mdc` | Understandability rubric the gauntlet walks |
| `owner-profile.mdc` | Commit/push/PR only when she asks |
| `hooks/ask-before-git-publish.sh` | Asks before commit/push/`gh pr create`; reminds gauntlet + Test plan |
| Project docs / CI | Source of truth for *which* commands belong in the plan |
