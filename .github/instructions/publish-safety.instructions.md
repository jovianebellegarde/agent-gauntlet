<!-- Intent: Publish/push safety checks and recommended git workflows.
     Always-apply in private sessions: yes (short summary included). -->
# Publish Safety Checks & Recommended Git Workflows

Always-apply summary:
- Require pre-publish evidence (tests, changelog, security scan) and an explicit "publish-evidence" artifact before pushing to remote.

Recommended workflow:
1. Branching
   - Work in feature branches: feature/<short-desc>.
   - Open PRs for reviews, attach generated evidence files.

2. Pre-publish checklist (automatable)
   - Unit tests: pass or documented failures with reason.
   - Lint: run and fix significant issues.
   - Security scan: run secret scanning, dependency scan, SAST if available.
   - Evidence produced: a small markdown file with what was run and result (.copilot/evidence/<branch>-YYYYMMDD.md).

3. Evidence structure (example)
   - .copilot/evidence/feature-some-fix-20260813.md:
     - commands run
     - test counts (passed/failed)
     - known failing tests with rationale
     - signature: owner name and timestamp

4. Push gating
   - pre-push hook blocks if no evidence file for the branch exists.
   - Hooks print what they'd do; they do not auto-push.

5. Publishing to public repos
   - Never publish private prompts or instructions to public remotes.
   - If you must push to an upstream, remove .github/instructions and .github/prompts or ensure repo is private.

6. Integration with CI
   - CI should re-run checks and validate the evidence file matches the CI logs.
