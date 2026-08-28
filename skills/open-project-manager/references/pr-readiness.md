# PR Readiness & Merge Gates

Protocol for reviewing changes and validating merge readiness.

## 1. Local Review Loop

```bash
codex review --base <base-branch>
```

1. Require a Git repository and resolve the base branch from Git metadata; ask the user if ambiguous.
2. Review committed branch changes with `codex review --base <base-branch>`.
3. When the working tree is dirty, also run `codex review --uncommitted`.
4. Evaluate each finding, fix actionable defects, and add regression tests where appropriate.
5. Re-run the local gate and applicable reviews until no actionable findings remain.
6. Document verified false positives without altering correct code.

> **Blocker:** If Codex review fails or is unavailable, report it as an explicit readiness blocker. Do not silently substitute a third-party review CLI.

---

## 2. Hard Merge Gates

Do not mark ready to merge while any condition is unmet:

- [ ] Required CI checks passing on the **latest commit** (not a stale run).
- [ ] No unresolved review comments or conversation threads.
- [ ] Committed branch review (`codex review --base <base-branch>`) clean.
- [ ] Uncommitted review clean when the working tree is dirty.
- [ ] **Independent review completed.** Self-review + green CI is not sufficient. At least one independent reviewer must approve.
- [ ] Manual testing documented on target runtime.
- [ ] No extraneous diffs, debug logs, temporary files, or leaked secrets.
- [ ] Documentation in `.opm/` synchronized with implementation.
- [ ] Never fabricate commit hashes, test output, or review status.

---

## 3. Draft vs Ready-for-Review

- Open PRs in **draft** during active development.
- Convert to **ready-for-review** only after the local review loop passes and all hard gates are met.
- If post-review changes require force-push or significant rework, return to draft.

---

## 4. Contributor Fork PRs

When submitting from a fork:
- Ensure the fork is up to date with the upstream target branch.
- CI must run against the merge result, not the fork branch alone.
- Include the upstream base branch in the PR description.

---

## 5. PR Evidence Standard

Every pull request must document:

1. **Problem & Scope:** Issue addressed and boundaries of the change.
2. **Approach:** Architecture decisions and non-obvious rationale.
3. **Automated Verification:** Exact commands run and result summary.
4. **Manual Verification:** Steps performed on target runtime, with screenshots for UI changes.
5. **Known Limitations:** Residual risk, follow-up items, deferred work.
