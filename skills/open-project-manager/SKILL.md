---
name: open-project-manager
description: >-
  Initialize, plan, execute, and validate AI-assisted development projects.
  Bootstrap planning documents in .opm/, manage phased implementation,
  and validate pull requests. Invoke with $open-project-manager.
---

# open-project-manager

Markdown-driven project lifecycle manager enforcing quality standards and verified execution.

## Initialization Contract

`plan` and `execute` require `AGENTS.md`, `.opm/SPEC.md`, `.opm/ROADMAP.md`, and `.opm/TASKS.md`. If any are missing, stop and direct the user to run `$open-project-manager init`; do not initialize implicitly.

## Action Modes

1. **`init`**: Bootstrap `.opm/` and root `AGENTS.md` in a repository.
2. **`plan`**: Create a verified phased implementation plan.
3. **`execute`**: Implement an approved phase with minimal changes.
4. **`review`**: Run uncommitted review loop and verify PR merge readiness.

---

## 1. Init (`init`)

1. **Inspect repository state:**
   ```bash
   git rev-parse --is-inside-work-tree
   ```
   - If this succeeds, inspect `git status --short` and `git branch --show-current`.
   - If it fails, continue without Git; never run `git init` without explicit permission.
2. **Detect environment:**
   - Detect project toolchains and package managers.
3. **Gather project requirements:** Solicit purpose, target users, constraints, and intended stack/architecture.
   - For an empty repository, ask for any known setup, lint, test, build, and run commands.
   - Write `Not established yet` for commands that do not exist; never invent commands.
4. **Stamp project instructions and planning documents:**
   - Read from `templates/` (`AGENTS.md`, `SPEC.md`, `ROADMAP.md`, `TASKS.md`).
   - Fill in project details; remove instructional comments, ellipses, blank fields, and prompt placeholders.
   - Write `Not established yet` for required unknowns and `None` for unavailable optional values.
   - Embed quality pillars (LTS dependencies, minimal edits, UI integrity).
   - In root `AGENTS.md`, create or replace one `## Open Project Manager` section while preserving all other content.
   - Create missing `.opm` documents only. Preserve existing documents unless the user explicitly requests regeneration.
5. **Write files:**
   - `AGENTS.md` (repository root)
   - `.opm/SPEC.md`
   - `.opm/ROADMAP.md`
   - `.opm/TASKS.md`

---

## 2. Plan (`plan`)

1. Enforce the initialization contract, then load root `AGENTS.md` and `.opm/` documents.
2. If the problem space is ambiguous, consult [`references/wayfinder.md`](./references/wayfinder.md).
3. Treat existing manifests and lockfiles as authoritative. For newly requested dependencies, choose compatible stable/LTS versions; never perform unrelated upgrades.
4. Draft phase plan with acceptance criteria, automated checks, and manual validations.
5. Set the active phase in `.opm/TASKS.md` to `**Approval:** Pending`, present the plan, and await explicit user approval.
6. After explicit approval, set the marker to `**Approval:** Approved`.

---

## 3. Execute (`execute`)

1. Enforce the initialization contract.
2. Require the active phase marker in `.opm/TASKS.md` to be exactly `**Approval:** Approved`; stop for `Pending`, `Completed`, missing, or malformed values.
3. Apply minimal changes only. No unsolicited refactors or unrelated dependency upgrades.
4. Use repository-pinned dependencies and preserve UI layout.
5. Run focused tests during iteration, then run full local test gate.
6. Inspect `git diff` for stray edits and update task status.
7. Set the marker to `**Approval:** Completed` only after all phase acceptance criteria pass.

---

## 4. Review (`review`)

1. Load [`references/pr-readiness.md`](./references/pr-readiness.md).
2. Require a Git repository; otherwise report review as blocked.
3. Resolve the base branch from Git metadata. Ask the user when it is ambiguous; never guess.
4. Review all committed branch changes:
   ```bash
   codex review --base <base-branch>
   ```
5. If the working tree is dirty, also run `codex review --uncommitted`.
6. Fix actionable findings; re-run the applicable reviews until clean.
7. Verify hard merge gates (clean CI, tests passing, no leaked secrets).
8. Output structured verification evidence. If Codex review is unavailable, report it as a blocker.

---

## References

- [Lifecycle (17 Steps)](./references/lifecycle.md)
- [PR Readiness & Gates](./references/pr-readiness.md)
- [Wayfinder (Ambiguity)](./references/wayfinder.md)
- [Security Baseline](./references/security-baseline.md)
- [Guardrails](./references/guardrails.md)
