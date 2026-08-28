---
name: open-project-manager
description: >-
  Initialize, plan, execute, and validate AI-assisted development projects.
  Use this skill to bootstrap a repository with planning documents in .open-project-manager/,
  manage phased implementation with approval checkpoints, or validate changes for merge readiness.
  Invoke explicitly with $open-project-manager.
---

# open-project-manager

A structured, markdown-driven project lifecycle manager enforcing quality standards, phased execution, and verified merge readiness.

## Action Modes

Determine the requested mode based on the user's intent or explicit invocation:

1. **`init`**: Bootstrap a repository with standard `.open-project-manager/` documentation.
2. **`plan`**: Create or refine a phased implementation plan based on project specs.
3. **`execute`**: Implement an approved phase with minimal edits and local verification.
4. **`review`**: Enforce local review loops, hard gates, and PR readiness evidence.

---

## 1. Project Initialization (`init`)

Use when bootstrapping a new codebase or adding structured project management to an existing repository:

1. **Inspect repository state:**
   ```bash
   git status --short
   git branch --show-current
   ```
2. **Environment & Toolchain Detection:**
   - Detect if `rtk` (Rust Token Killer) is available on `PATH` (`rtk --version`).
   - Detect platform (native Windows vs WSL / Linux / macOS).
   - Detect existing build tools, package managers, test runners, and linters.
3. **Clarify Project Requirements:**
   - Ask the user for project goals, target users, core requirements, and technical constraints.
4. **Read & Adapt Templates:**
   - Read source templates from `templates/` (`AGENTS.md`, `SPEC.md`, `ROADMAP.md`, `TASKS.md`).
   - Fill in actual project details; remove placeholder text and irrelevant sections.
   - **RTK Inclusion Rule:** Include RTK command guidance in `.open-project-manager/AGENTS.md` **only if `rtk` was detected** in Step 2. If `rtk` is missing, omit RTK references entirely.
   - **Embed Quality Pillars:** Ensure the generated `.open-project-manager/AGENTS.md` incorporates strict rules for latest LTS dependencies, minimal changes, maintainability, and UI integrity.
5. **Write Project Documents:**
   - Create `.open-project-manager/` directory at the repository root and write:
     - `.open-project-manager/AGENTS.md`
     - `.open-project-manager/SPEC.md`
     - `.open-project-manager/ROADMAP.md`
     - `.open-project-manager/TASKS.md`
6. **Summary:** Present created documents and confirm readiness for initial planning.

---

## 2. Phased Planning (`plan`)

Use to scope features, define milestones, or resolve architectural ambiguity:

1. **Read Project Context:**
   - Load `.open-project-manager/AGENTS.md`, `SPEC.md`, `ROADMAP.md`, and `TASKS.md`.
2. **Handle Ambiguity (Wayfinder Mode):**
   - If the task is foggy or involves deep architectural decisions with multiple open branches, consult [`references/wayfinder.md`](./references/wayfinder.md) to map decisions using `MAP.md` and `ANSWERS.md`.
3. **Verify Dependencies & Technology Stack:**
   - When specifying any framework, library, or dependency versions, **verify against current LTS or stable releases** from official registries. Never guess versions.
4. **Formulate Phase Plan:**
   - Break work into small, reviewable phases.
   - Map each phase to acceptance criteria with automated checks and manual verification steps.
   - Establish rollback boundaries and pause points.
5. **Approval Checkpoint:** Present the plan clearly and stop for explicit user approval before writing code.

---

## 3. Phased Implementation (`execute`)

Use to execute an approved phase from `.open-project-manager/TASKS.md`:

1. **Verify Authorization & Scope:**
   - Ensure the current phase in `.open-project-manager/TASKS.md` is approved.
2. **Adhere to Quality Pillars:**
   - **Minimal Changes:** Touch only what the active task requires. No unsolicited refactors or styling adjustments.
   - **LTS / Stable Modules:** Introduce only verified current LTS/stable dependencies.
   - **Maintainability:** Follow existing repository conventions, structure, and naming.
   - **UI Integrity:** Do not alter layout, component structure, or spacing unless explicitly tasked.
3. **Iterative Verification:**
   - Run focused tests and linters during development.
   - Read full error traces before fixing issues.
4. **Complete Local Gate:**
   - Run the project's full test suite, build, and linter commands.
5. **Diff Inspection & Task Update:**
   - Inspect `git diff` to ensure no unrelated files or debug code remain.
   - Update task status in `.open-project-manager/TASKS.md` with verifiable evidence.

---

## 4. PR Readiness & Review (`review`)

Use when preparing a branch for review, pull request, or merge:

1. **Load Reference:** Read [`references/pr-readiness.md`](./references/pr-readiness.md) for complete gate requirements.
2. **Local Review Loop:**
   - Run uncommitted review:
     ```bash
     codex review --uncommitted
     ```
   - Verify each finding against the diff, fix actionable defects, and repeat until clean.
3. **Verify Hard Gates:**
   - Automated test suite passing.
   - No uncommitted secrets, debug junk, or unrelated modifications.
   - Documentation in `.open-project-manager/` synchronized with implementation.
   - Documented manual testing completed on the target environment.
4. **Evidence Summary:** Generate a structured summary of changes, test results, manual observations, and residual risk.

---

## Reference Guides

- [Development Lifecycle (17 Steps)](./references/lifecycle.md)
- [PR Readiness & Merge Gates](./references/pr-readiness.md)
- [Wayfinder Ambiguity Framework](./references/wayfinder.md)
- [Security Baseline & Governance](./references/security-baseline.md)
- [RTK Command & Token Guide](./references/rtk-guide.md)
- [Command Safety & Guardrails](./references/guardrails.md)
