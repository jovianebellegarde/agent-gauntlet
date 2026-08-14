# agent-gauntlet

**Make AI coding agents earn trust before they ship code.**

This is a small set of [Cursor](https://cursor.com) rules and hooks. Cursor is an editor with an AI agent that can edit files and run terminal commands. Agents are fast — they can also leave unclear code or skip tests. This template puts a **checkpoint** in front of commit, push, and pull requests.

**North star:** code that is easy to understand and still proven to work.

## The idea in one minute

```text
You ask to commit / push / open a PR
        ↓
Agent reviews the change for clarity (names, small functions, common code smells)
        ↓
Agent runs this project's tests / checks
        ↓
Only then: commit, push, or PR — with a Test plan that has real evidence, not empty checkboxes
```

Hooks ask you to confirm before git publish steps, and remind the agent that the checks should already have passed.

Inspiration (not a book dump): Uncle Bob–style *process* — constrain agents with a gauntlet; Contieri-style *smell → fix* checklists for what to look for in a diff.

## Who this is for

Anyone using Cursor (or a similar agent workflow) who wants **readable code** and **verification before publish** — without relying on hope.

## What you get

### Rules (loaded as a Cursor plugin)

| File | Plain English |
|---|---|
| `rules/swe-principles.mdc` | Standards for clear code + a short smell checklist |
| `rules/agent-verification-gauntlet.mdc` | “Do the review and run checks *before* commit/push” |
| `rules/pr-test-plan.mdc` | On a PR, the agent runs what it can and checks boxes with proof |

### Hooks (recommended — rules alone are easy to skip)

| File | When it runs | Plain English |
|---|---|---|
| `hooks/session-start-reminder.sh` | New agent session | Reminds: review + tests before publish; PR evidence |
| `hooks/ask-before-git-publish.sh` | `git commit`, `git push`, or `gh pr create` | Asks you before those commands; reminds the agent of the gauntlet |
| `hooks/hooks.json` | — | Wires the hooks together |

## Install (macOS)

### 1. Clone

```bash
git clone https://github.com/j0viane/agent-gauntlet.git ~/Cursor/agent-gauntlet
```

Use any path you like — keep the symlinks below in sync with it.

### 2. Load as a local Cursor plugin

```bash
mkdir -p ~/.cursor/plugins/local
ln -sf "$HOME/Cursor/agent-gauntlet" ~/.cursor/plugins/local/agent-gauntlet
```

### 3. Install hooks (global)

```bash
mkdir -p ~/.cursor/hooks
ln -sf "$HOME/Cursor/agent-gauntlet/hooks/session-start-reminder.sh" ~/.cursor/hooks/session-start-reminder.sh
ln -sf "$HOME/Cursor/agent-gauntlet/hooks/ask-before-git-publish.sh" ~/.cursor/hooks/ask-before-git-publish.sh
ln -sf "$HOME/Cursor/agent-gauntlet/hooks/hooks.json" ~/.cursor/hooks.json
```

### 4. Reload

In Cursor: **Developer → Reload Window**.

Confirm under **Cursor Settings → Plugins** and the **Hooks** output channel.

## Adapt for your project

- Document how to test locally (README, `TESTING.md`, or CI). The agent looks there first; otherwise it tries common commands (`pytest`, `npm test`, and similar).
- Put **project-specific** rules in that repo’s `.cursor/rules/` (APIs, privacy, product limits). This template stays generic on purpose.
- Commit, push, and open PRs only when **you** ask — the hooks reinforce that.

## Not included

No personal profiles, teaching curricula, or product-specific rules. Add those privately if you need them.

## License

MIT — see [LICENSE](LICENSE).

This repository is the Copilot-adapted public-facing version of Agent Gauntlet.
It contains Copilot instruction manifests and sanitized prompts for public use.
