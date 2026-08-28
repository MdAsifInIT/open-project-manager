# RTK (Rust Token Killer) Command Guide

RTK is a command output compression proxy that reduces context window consumption by 60–90% on noisy development commands.

## Golden Rule

- **Use `rtk`** when command output is likely to be large, noisy, or repetitive, and a summarized result is sufficient. (e.g., test suites, full builds, linter sweeps, dependency lists).
- **Keep commands raw** when output is naturally short, exact exit codes/stdout/stderr matter, or when debugging specific test failures.
- **Do not use `rtk`** on systems where `rtk` is not installed or when validating exact shell piping.

---

## Token Savings by Command Category

| Category | Command Examples | Typical Reduction |
| --- | --- | --- |
| **Testing** | `rtk cargo test`, `rtk pytest`, `rtk vitest`, `rtk jest` | 90–99% |
| **Build & Lint** | `rtk tsc`, `rtk cargo clippy`, `rtk lint`, `rtk next build` | 70–87% |
| **Version Control** | `rtk git status`, `rtk git log`, `rtk git diff` | 60–80% |
| **GitHub CLI** | `rtk gh pr view`, `rtk gh pr checks`, `rtk gh run list` | 30–85% |
| **Package Managers**| `rtk pnpm install`, `rtk npm list`, `rtk cargo tree` | 70–90% |
| **Files & Search** | `rtk ls <dir>`, `rtk grep <pattern>`, `rtk find <pattern>` | 60–75% |

---

## Platform Details

- **Windows PowerShell**: `rtk` commands run directly. Note that PowerShell aliases `curl` and `wget` to `Invoke-WebRequest`; use `curl.exe` or `wget.exe` for standard CLI tool behavior.
- **WSL / Linux / macOS**: Standard shell integration applies.
- **Diagnostic / Statistics**: Run `rtk gain` to inspect lifetime token reduction and savings.
