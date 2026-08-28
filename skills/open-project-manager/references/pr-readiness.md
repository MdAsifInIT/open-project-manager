# PR Readiness & Merge Gates

Standardized protocol for validating changes, executing review loops, and preparing pull requests for merge.

## 1. Local Review Loop

Before opening or updating a PR, run the built-in local review tool:

```bash
codex review --uncommitted
```

1. **Evaluate Findings:** Inspect each comment against the current diff.
2. **Fix Actionable Defects:** Remediate verified defects, adding regression tests where appropriate.
3. **Re-run Validation:** Ensure all automated checks pass after each fix.
4. **Repeat Review:** Run `codex review --uncommitted` until no actionable findings remain.
5. **False Positives:** Document verified false positives without altering correct code.

> [!CAUTION]
> If Codex review fails or is unavailable, report it as an explicit blocker. Do not silently substitute third-party review CLIs.

---

## 2. Hard Merge Gates

Do not mark a pull request or change as ready to merge while any of the following exist:

- [ ] Required CI checks are failing, pending, or missing on the latest commit.
- [ ] Actionable review comments or conversation threads remain unresolved.
- [ ] Local review loop (`codex review --uncommitted`) is incomplete or shows unresolved defects.
- [ ] Required manual testing is incomplete, skipped without reason, or undocumented.
- [ ] The working branch contains unrelated modifications, debug code, temporary files, or secrets.
- [ ] Project documentation in `.open-project-manager/` does not reflect the current implementation.

---

## 3. Pull Request Evidence Standard

Every pull request description must document:

1. **Problem & Scope:** Clear description of the issue addressed and boundaries of the change.
2. **Implementation Approach:** Architectural overview and non-obvious design choices.
3. **Automated Verification:** Exact commands executed and their output summary.
4. **Manual Verification:** Steps performed on the target runtime environment with screenshots/recordings for UI changes.
5. **Known Limitations & Follow-ups:** Any residual risk or future tasks.
