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
