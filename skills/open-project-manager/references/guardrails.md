# Command Safety & Execution Guardrails

Policies to prevent data loss, remote repository corruption, and unintended modifications during automated agent sessions.

## 1. Forbidden Destructive Operations

The following commands must **never** be executed by an automated agent without explicit, separate user authorization for each invocation:

| Command | Rationale |
| --- | --- |
| `git reset --hard` | Destroys uncommitted changes irreversibly. Use targeted file restores instead. |
| `git clean -fd` | Deletes untracked files permanently. Inspect and remove files individually. |
| `git push --force` / `-f` | Overwrites remote branch history and disrupts collaborators. |
| `gh repo delete` | Permanent repository destruction. Must be performed manually in web UI. |
| `rm -rf /` or recursive root deletions | Destructive system impact. |

---

## 2. Safe Read-Only Inspection Commands

The following commands are safe for routine environment and code discovery:

### Windows / PowerShell
- `Get-ChildItem`, `Get-Content`, `Get-Item`, `Select-String`, `Test-Path`, `Resolve-Path`
- `rg` (ripgrep), `fd`, `jq`, `where.exe`, `tasklist`

### POSIX / Bash
- `ls`, `cat`, `head`, `tail`, `wc`, `grep`, `find`, `which`, `stat`, `file`
- `rg`, `fd`, `jq`, `ps`

### Git & GitHub CLI (Read-Only)
- `git status`, `git diff`, `git log`, `git show`, `git branch`, `git rev-parse`
- `gh pr view`, `gh pr list`, `gh pr checks`, `gh issue view`, `gh run list`
