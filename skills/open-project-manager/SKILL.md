---
name: open-project-manager
description: >-
  Initialize, plan, execute, and validate AI-assisted development projects.
  Bootstrap planning documents in .open-project-manager/, manage phased implementation,
  and validate pull requests. Invoke with $open-project-manager.
---

# open-project-manager

Markdown-driven project lifecycle manager enforcing quality standards and verified execution.

## Action Modes

1. **`init`**: Bootstrap `.open-project-manager/` in a repository.
2. **`plan`**: Create a verified phased implementation plan.
3. **`execute`**: Implement an approved phase with minimal changes.
4. **`review`**: Run uncommitted review loop and verify PR merge readiness.

---

## 1. Init (`init`)

1. **Inspect repository state:**
   ```bash
   git status --short
   git branch --show-current
   ```
2. **Detect environment:**
   - Detect `rtk` on `PATH` (`rtk --version`).
   - Detect OS (Windows PowerShell vs WSL/Linux/macOS).
   - Detect project toolchains and package managers.
3. **Gather project requirements:** Solicit core goals, constraints, and architecture.
4. **Stamp templates into `.open-project-manager/`:**
   - Read from `templates/` (`AGENTS.md`, `SPEC.md`, `ROADMAP.md`, `TASKS.md`).
   - Fill in project details; eliminate placeholder text.
   - Include RTK guidance in `AGENTS.md` **only if `rtk` is installed**.
   - Embed quality pillars (LTS dependencies, minimal edits, UI integrity).
5. **Write files:**
   - `.open-project-manager/AGENTS.md`
   - `.open-project-manager/SPEC.md`
   - `.open-project-manager/ROADMAP.md`
   - `.open-project-manager/TASKS.md`

---

## 2. Plan (`plan`)

1. Load `.open-project-manager/` documents.
2. If the problem space is ambiguous, consult [`references/wayfinder.md`](./references/wayfinder.md).
3. Verify all dependency versions against current LTS/stable releases.
4. Draft phase plan with acceptance criteria, automated checks, and manual validations.
5. Present plan and await explicit user approval.

---

## 3. Execute (`execute`)

1. Confirm phase approval in `.open-project-manager/TASKS.md`.
2. Apply minimal changes only. No unsolicited refactors.
3. Use verified LTS dependencies and preserve UI layout.
4. Run focused tests during iteration, then run full local test gate.
5. Inspect `git diff` for stray edits and update task status.

---

## 4. Review (`review`)

1. Load [`references/pr-readiness.md`](./references/pr-readiness.md).
2. Run uncommitted review loop:
   ```bash
   codex review --uncommitted
   ```
3. Fix actionable findings; re-run until clean.
4. Verify hard merge gates (clean CI, tests passing, no leaked secrets).
5. Output structured verification evidence.

---

## References

- [Lifecycle (17 Steps)](./references/lifecycle.md)
- [PR Readiness & Gates](./references/pr-readiness.md)
- [Wayfinder (Ambiguity)](./references/wayfinder.md)
- [Security Baseline](./references/security-baseline.md)
- [RTK Guide](./references/rtk-guide.md)
- [Guardrails](./references/guardrails.md)
