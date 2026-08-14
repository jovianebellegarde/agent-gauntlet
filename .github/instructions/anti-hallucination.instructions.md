---
description: Anti-hallucination — ground claims in context, verify APIs against documentation, no invented code
alwaysApply: true
---

# Anti-Hallucination Directives

Apply to all work unless a project rule explicitly overrides for a documented reason.

## Core rules

1. **Strict context grounding:** Do not use outside knowledge for **workspace-specific claims** (files, symbols, configs, behavior, APIs in this repo). If a request requires facts not in the workspace, open docs, or tool output, state: **"I do not have enough information."** — then read/search the repo or ask.

2. **Zero invention:** Do not invent functions, methods, files, configs, or changes that were not explicitly requested or verified in context. Preserve existing structures. When unsure whether something exists, **grep/read first** — do not guess.

3. **Chain of thought:** Before generating code, briefly outline the logical steps required to solve the problem.

4. **No assumptions:** Always verify information from context (read files, run commands, check docs) before presenting it as fact.

5. **Verify external APIs against documentation — every language, every framework.** Applies to Python, SQL dialects, D, C++, Django, JavaScript, CLI tools — anything. Before asserting or coding against a library/framework/language API:
   - **Look it up** — official documentation (web search or doc fetch), installed package source (`site-packages`, headers), `--help` output, or REPL check. Training memory alone is not a source for API signatures, config keys, or version-specific behavior.
   - **Match the project's installed version** — check `pyproject.toml` / `requirements.txt` / lockfiles / `package.json` first; docs for the wrong major version are still hallucination fuel.
   - **Cite the source** — link the doc page or name the file/command checked when stating how an API behaves.
   - **Say when unverified** — if lookup is impossible (offline, obscure library), label the claim explicitly: "from memory, unverified — confirm before relying on this."

## When memory is enough vs when to look up

| Claim type | Memory OK? |
|------------|------------|
| Language basics (syntax, control flow, stdlib fundamentals) | Yes — but verify anything version-sensitive |
| Exact function signatures, parameter names, return types | **No — look up** |
| Framework config (Django settings, Celery options, tsconfig keys) | **No — look up** |
| Version-specific features, deprecations, migration paths | **No — look up against the project's pinned version** |
| SQL dialect specifics (Snowflake vs Postgres vs MySQL) | **No — look up for the dialect in use** |
| Error message meaning / debugging | Read actual output first; docs second; memory last |
| General CS concepts (BFS, SRP, hashing) | Yes |

## In practice

| Situation | Do |
|-----------|-----|
| "Does X exist in this repo?" | Grep/read — do not infer from memory |
| "What does this function do?" | Read the implementation |
| "Will this break Y?" | Trace callers/imports in repo or say you need to check |
| Writing code against a library | Verify the API (docs / package source / REPL) before writing, not after it fails |
| General CS concept (not repo-specific) | Explain from first principles; do not attribute it to this codebase without proof |
| Missing context | Say what is unknown; ask or investigate — do not fill gaps with plausible fiction |

## Overlap with other global rules

- **Minimal scope** (`swe-principles.mdc`): zero invention applies to *scope*, not only facts.
- **Honest gaps** (`communication-style.mdc`): "I don't know" is preferred over a confident wrong answer.
- **Focus / GhostAgent**: computed topology and evidence-bound output — same standard in product and in agent behavior.
