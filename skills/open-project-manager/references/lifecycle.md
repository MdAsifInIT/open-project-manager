# Development Lifecycle

A complete 17-step lifecycle for AI-assisted engineering projects.

## Project File Roles

| File | Purpose |
| --- | --- |
| `.open-project-manager/AGENTS.md` | Durable repository rules and instructions loaded by coding agents |
| `.open-project-manager/SPEC.md` | Requirements, system boundaries, and acceptance criteria |
| `.open-project-manager/ROADMAP.md` | Ordered phases, outcomes, dependencies, risks, and exit criteria |
| `.open-project-manager/TASKS.md` | Active phase checklist and validated task status |
| `references/` | Detailed reference materials and runbooks |

---

## 17-Step Lifecycle

1. **Initialize Project:** Run `$open-project-manager init` to detect environment and stamp `.open-project-manager/` templates.
2. **Inspect Context:** Examine repository architecture, existing code, branches, and tooling.
3. **Establish Rules:** Populate `.open-project-manager/AGENTS.md` with project boundaries, verified commands, and style guides.
4. **Specify Requirements:** Define testable requirements, user constraints, and non-goals in `.open-project-manager/SPEC.md`.
5. **Phase the Roadmap:** Break deliverables into ordered phases with exit criteria in `.open-project-manager/ROADMAP.md`.
6. **Task Breakdown:** Break the current phase into granular, testable items in `.open-project-manager/TASKS.md`.
7. **Draft Plan:** Use `$open-project-manager plan` to draft a requirements-linked implementation plan.
8. **User Approval:** Stop at the approval checkpoint and await explicit confirmation.
9. **Implement Phase:** Use `$open-project-manager execute` to implement the approved phase with minimal edits.
10. **Focused Validation:** Run focused unit and integration tests during implementation.
11. **Complete Local Gate:** Run full lint, type check, build, and test suite.
12. **Update Task Status:** Mark completed tasks in `TASKS.md` with verified evidence.
13. **Local Review Loop:** Use `$open-project-manager review` to run uncommitted review (`codex review --uncommitted`), resolving all actionable findings.
14. **Manual Environment Testing:** Perform and document manual verification on the real target runtime.
15. **Prepare PR:** Open a ready-for-review pull request with structured evidence when authorized.
16. **Remote Verification:** Verify CI checks, security scans, and independent reviews against the latest commit.
17. **Merge & Archive:** Merge only after all gates pass and update project roadmap status.

---

## Documentation Synchronization Rule

Never rely on an agent guessing project structure. All architecture, conventions, and planning files must be referenced from `.open-project-manager/AGENTS.md` or active task prompts. Keep specification, roadmap, tasks, and implementation synchronized at all times.
