# Development Lifecycle

17-step lifecycle for AI-assisted engineering.

## File Roles

| File | Purpose |
| --- | --- |
| `.open-project-manager/AGENTS.md` | Durable agent rules and instructions |
| `.open-project-manager/SPEC.md` | Requirements, boundaries, and acceptance criteria |
| `.open-project-manager/ROADMAP.md` | Ordered phases, outcomes, and exit criteria |
| `.open-project-manager/TASKS.md` | Active phase checklist and validation records |

---

## 17 Steps

1. **Init:** Run `$open-project-manager init` to create `.open-project-manager/`.
2. **Inspect:** Examine existing code, architecture, and tooling.
3. **Rules:** Establish repository boundaries and verified commands in `AGENTS.md`.
4. **Specify:** Define testable requirements and non-goals in `SPEC.md`.
5. **Roadmap:** Sequence deliverables and exit criteria in `ROADMAP.md`.
6. **Task Breakdown:** Define tasks with validation checks in `TASKS.md`.
7. **Draft Plan:** Formulate implementation plan via `$open-project-manager plan`.
8. **Approve:** Await user approval.
9. **Implement:** Execute phase via `$open-project-manager execute` with minimal edits.
10. **Focused Validation:** Run targeted checks during iteration.
11. **Local Gate:** Run full lint, typecheck, build, and test suite.
12. **Update Status:** Record verified task completions in `TASKS.md`.
13. **Review Loop:** Run `codex review --uncommitted` and remediate findings.
14. **Manual Test:** Validate behavior on target runtime.
15. **Open PR:** Submit pull request with structured evidence.
16. **Remote Checks:** Verify CI workflows and security scans.
17. **Merge:** Merge only when all gates pass; advance roadmap.
