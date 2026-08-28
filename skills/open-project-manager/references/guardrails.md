# Command Guardrails

Execution boundaries to prevent repository damage and data loss.

## Forbidden Destructive Commands

Require explicit user confirmation for each invocation:

| Command | Risk |
| --- | --- |
| `git reset --hard` | Irreversibly destroys uncommitted work |
| `git clean -fd` | Permanently deletes untracked files |
| `git push --force` | Overwrites remote repository history |
| `gh repo delete` | Permanent repository destruction |

## Safe Read-Only Commands

- **System:** Standard read commands (`ls`, `cat`, `grep`, `rg`, `fd`, `jq`, etc.)
- **Git / GitHub CLI:** `git status`, `git diff`, `git log`, `gh pr view`, `gh run list`
