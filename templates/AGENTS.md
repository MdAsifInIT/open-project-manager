# Project Instructions

## Purpose

Describe the project purpose, target audience, and primary outcomes here.

## Architecture & Tech Stack

- **Directory Structure:** Describe the key directories, modules, and components.
- **Toolchain & Runtime:** Supported runtime versions (e.g., Node.js LTS, Python 3.12, Rust stable).
- **External Interfaces:** APIs, database connections, third-party integrations.

## Quality Pillars

### 1. Latest LTS Dependencies
- When adding or upgrading dependencies, always select the current LTS or latest stable release.
- Verify version numbers against official package registries (npm, PyPI, Crates.io, etc.) before writing configuration. Never guess versions.

### 2. Minimal Required Changes
- Implement only the minimum changes necessary to satisfy each task.
- Avoid drive-by refactoring, style overhaul, or modifications to untouched files.
- Clean up only artifacts and imports introduced by your own change.

### 3. Maintainability & Readability
- Follow established project naming conventions, directory patterns, and idiomatic styles.
- Prefer clear, self-explanatory code over complex abstractions.

### 4. UI & Visual Integrity
- Do not alter visual styling, layout, typography, or component arrangement unless explicitly requested by the task.

## Working Boundaries

- Keep credentials, tokens, secrets, and private local files out of version control.
- Seek explicit confirmation before executing destructive operations or database schema alterations.
- Preserve existing user modifications across sessions.

## Verified Commands

Document the exact verified commands for this repository:

- **Setup:** `...`
- **Lint / Type Check:** `...`
- **Test:** `...`
- **Build / Run:** `...`

## Validation Standard

- Run focused test and lint checks during feature iteration.
- Execute the complete local test suite before marking any task complete.
- Verify user-facing or visual changes with rendered output or screenshots.

## Documentation Routing

- **Requirements & Boundaries:** Read [`.open-project-manager/SPEC.md`](./SPEC.md)
- **Phase Roadmap & Exit Criteria:** Read [`.open-project-manager/ROADMAP.md`](./ROADMAP.md)
- **Active Task Progress:** Read [`.open-project-manager/TASKS.md`](./TASKS.md)
