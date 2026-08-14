---
description: SWE craft + Contieri-inspired smells — code that is easy to understand
alwaysApply: true
---

# SWE Best Practices

**North star:** code that is easy to understand — for Joviane mid-session, in an interview, and on a PR.

Apply unless a project rule explicitly overrides. Inspiration (not verbatim books): durable craft from Uncle Bob–style professionalism; smell → fix recipes in the Contieri mold.

Before `commit` / `push` / PR, the agent must walk this rubric on the pending diff — see `agent-verification-gauntlet.mdc`.

## A — Durable craft

| Check | Fail if |
|---|---|
| **Meaningful names** | Vague (`process`, `data`, `handleStuff`) or misleading |
| **Small focused units** | New/grown function does several unrelated jobs (**SRP**) |
| **Obvious intent** | Reader needs a paragraph of comments to know what changed |
| **No surprise side effects** | Function named like a query also mutates or hides I/O |
| **Professional simplicity** | Cleverness, deep nesting, or “magic” where plain code works |

Also:

- **DRY:** Extract duplication that would change together. Do not abstract one-off similarity — two similar lines beat a premature helper.
- **KISS:** Prefer the simplest correct solution. No extra layers, flags, or config for hypothetical futures.
- **YAGNI:** Do not build features, abstractions, or error paths for cases that do not exist yet.
- **Booleans** read as questions: `is_awaiting_otp`, `has_submitted_form`.
- Avoid abbreviations unless universal (`id`, `url`, `api`).

## B — Smell → fix (Contieri-inspired)

Walk these on the **pending diff only** (not drive-by file rewrites).

| Smell | Fail if (in this diff) | Fix direction |
|---|---|---|
| **Long method / god function** | Many unrelated steps in one unit | Extract cohesive helpers |
| **Primitive obsession** | Raw primitives carry domain meaning without project-normal naming/types | Constant, small type, or existing helper |
| **Feature envy** | Logic mostly operates on another module’s data | Move closer to data (or ask) |
| **Shotgun surgery risk** | One concept needs scattered edits with no home | Prefer one owner module (ask if large) |
| **Flag / boolean soup** | New flags encode real variants | Clear branches or separate functions |
| **Noise comments** | Restate code; debug leftovers | Delete or rename instead |
| **Premature abstraction** | Helper for a single call site | Inline until a second real caller |

## C — Change discipline

- **Minimal diff:** Only modify code required by the current task. No drive-by refactors, renames, or formatting sweeps.
- **Match conventions:** Read surrounding code first. Match naming, types, imports, and patterns already in the file.
- **Comments:** Code should be self-explanatory. Comment only non-obvious business logic or tricky invariants — not what the code literally does.

### Guardrails

- If a “clean” change makes the code *harder* to follow, it fails the north star — prefer the simpler readable version.
- Prefer a **small recipe fix** over a large extract-until-pure rewrite when scope would explode.
- Ambiguous structure → **ask** (owner-profile decision protocol); do not silently redesign.
- Project rules (Focus ROA, LLM boundary, etc.) still win on product surfaces.

## Dependency and CI currency

- **Prefer current stable versions** for new dependencies, lockfile refreshes, and GitHub Actions (`uses:` pins).
- When adding or editing a workflow, **check the action's latest release tag on GitHub** (e.g. `actions/checkout`, `astral-sh/setup-uv`) — do not copy old major tags from memory or invent floating majors (`@v8`) unless that exact tag exists on the repo.
- **CI deprecation warnings and unresolved-action failures are defects.** Fix them in the same change when practical (or immediately after), especially on a young project — shipping with known-outdated or invalid Action pins looks careless.
- Upgrade Python deps with the project's package manager (`uv lock` / `uv add`) unless a version pin exists for a documented compatibility reason. State that reason in the PR/commit if you stay behind.

## Examples

```python
# BAD — does too much, vague name
def process(data):
    r = requests.get(data["url"])
    return json.loads(r.text)["results"]

# GOOD — single responsibility, clear name
async def fetch_search_results(url: str) -> list[SearchResult]:
    response = await http_client.get(url)
    response.raise_for_status()
    return parse_search_results(response.json())
```

```typescript
// BAD — premature abstraction
const useMaybeFetch = (cond: boolean, fn: () => void) => cond ? fn() : undefined;

// GOOD — inline until a second caller exists
if (job.status === "awaiting_otp") {
  openOtpModal(job.id);
}
```

## Relationship

| Rule / hook | Role |
|---|---|
| **This file** | Canonical craft + smell rubric (understandability) |
| `agent-verification-gauntlet.mdc` | When to run this review + project tests before publish |
| `pr-test-plan.mdc` | PR body evidence after checks run |
