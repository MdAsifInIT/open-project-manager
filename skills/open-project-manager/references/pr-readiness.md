# PR Readiness & Merge Gates

Protocol for reviewing changes and validating merge readiness.

## 1. Local Review Loop

```bash
codex review --uncommitted
```

1. Evaluate each finding against the diff.
2. Fix actionable defects and add regression tests.
3. Re-run local gate.
4. Repeat `codex review --uncommitted` until no actionable findings remain.

---

## 2. Hard Merge Gates

Do not mark ready to merge if any condition is unmet:

- [ ] Required CI checks passing on latest commit.
- [ ] No unresolved review comments or threads.
- [ ] Local review loop clean.
- [ ] Manual testing documented on target runtime.
- [ ] No extraneous diffs, debug logs, or leaked secrets.
- [ ] Documentation in `.open-project-manager/` updated.

---

## 3. PR Evidence Standard

Every pull request must document:

1. **Problem & Scope:** Exact issue addressed.
2. **Approach:** Architectural decisions.
3. **Automated Verification:** Test/build commands and results.
4. **Manual Verification:** Steps run and UI screenshots/recordings.
5. **Limitations:** Known boundaries or residual risk.
