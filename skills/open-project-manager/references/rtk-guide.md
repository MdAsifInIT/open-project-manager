# RTK Command Guide

RTK compresses noisy command output to save context window tokens.

## Usage Rule

- **Use `rtk`** for high-volume or repetitive command output (test suites, builds, linters, dependency trees).
- **Use raw commands** when output is short, exact exit codes/pipes matter, or when debugging failures.
- **Do not use `rtk`** if not installed on the system.

## Savings Overview

| Category | Commands | Token Reduction |
| --- | --- | --- |
| Testing | `rtk cargo test`, `rtk pytest`, `rtk vitest` | 90–99% |
| Build & Lint | `rtk tsc`, `rtk ruff check`, `rtk next build` | 70–87% |
| Git | `rtk git status`, `rtk git log`, `rtk git diff` | 60–80% |
| Package Managers | `rtk pnpm install`, `rtk npm list` | 70–90% |

*Diagnostic:* Run `rtk gain` to view cumulative token savings.
