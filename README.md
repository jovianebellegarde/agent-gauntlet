# agent-gauntlet

**Make AI coding agents earn trust before they ship code.**

This is a Copilot-focused instruction pack for repository safety gates. Agents are fast, but they can still miss quality checks. This template adds a **checkpoint** before commit, push, and pull requests.

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

Anyone using GitHub Copilot agent workflows (or similar tooling) who wants **readable code** and **verification before publish** — without relying on hope.

## What you get

### Rules (loaded from `.github/instructions`)

| File | Plain English |
|---|---|
| `.github/instructions/swe-principles.instructions.md` | Standards for clear code + a short smell checklist |
| `.github/instructions/agent-verification-gauntlet.instructions.md` | “Do the review and run checks *before* commit/push” |
| `.github/instructions/pr-test-plan.instructions.md` | On a PR, the agent runs what it can and checks boxes with proof |

### Prompt helper

| File | Plain English |
|---|---|
| `.github/prompts/verify-before-publish.prompt.md` | Adds a reusable “verify before publish” prompt for local/project workflows |

## Install (macOS)

### 1. Clone

```bash
git clone https://github.com/jovianebellegarde/agent-gauntlet.git ~/agent-gauntlet
```

Use any path you like.

### 2. Use in a target repository

```bash
mkdir -p /path/to/your-repo/.github/instructions /path/to/your-repo/.github/prompts
cp -R ~/agent-gauntlet/.github/instructions/* /path/to/your-repo/.github/instructions/
cp -R ~/agent-gauntlet/.github/prompts/* /path/to/your-repo/.github/prompts/
```

### 3. Reload your editor

In VS Code: **Developer: Reload Window**.

## Adapt for your project

- Document how to test locally (README, `TESTING.md`, or CI). The agent looks there first; otherwise it tries common commands (`pytest`, `npm test`, and similar).
- Put **project-specific** rules in that repo’s `.github/instructions/` (APIs, privacy, product limits). This template stays generic on purpose.
- Commit, push, and open PRs only when **you** ask — the hooks reinforce that.

## Not included

No personal profiles, teaching curricula, or product-specific rules. Add those privately if you need them.

## License

MIT — see [LICENSE](LICENSE).

This repository is the Copilot-adapted public-facing version of Agent Gauntlet.
It contains Copilot instruction manifests and sanitized prompts for public use.
