# open-project-manager

A portable, markdown-driven AI project management and workflow plugin for coding assistants (Codex, Antigravity, Claude Code, and AGY-compatible agents).

## Overview

`open-project-manager` extracts durable development workflows, quality standards, and planning methodologies into a modular, easily editable structure. All instructions and templates live in Markdown (`.md`) files without requiring build steps or runtime binaries.

### Key Capabilities

1. **Clean Project Initialization (`init`)**: Sets up `.open-project-manager/` within any project repository, containing tailored `AGENTS.md`, `SPEC.md`, `ROADMAP.md`, and `TASKS.md` without polluting the project root.
2. **Quality Pillars Enforced**:
   - **Latest LTS Dependencies**: Verified against official registries and release pages, never guessed from model training data.
   - **Minimal Required Changes**: Strictly minimal, best-practice diffs without drive-by refactoring.
   - **Maintainability & Readability**: Clean patterns, standard project structures, and clear human-readable code.
   - **UI Integrity**: Visual layout, styling, and spacing remain unchanged unless explicitly instructed.
3. **Phased Execution (`plan` & `execute`)**: Manages requirement-linked phases with clear validation gates and approval pause points.
4. **Readiness & Review Gate (`review`)**: Enforces uncommitted review loops (`codex review --uncommitted`), manual test checklists, and pull request readiness.
5. **Ambiguity Resolution**: Built-in Wayfinder methodology (`MAP.md` + `ANSWERS.md`) for navigating complex or foggy architectural decisions.

---

## Installation & Discovery

### 1. Antigravity / Gemini Plugin Discovery
Place or symlink this directory into your global customization plugins root:
```powershell
# Windows PowerShell
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.gemini\config\plugins\open-project-manager" -Target "C:\Users\MdAsif\Documents\Code\open-project-manager"
```
Or register it in `~/.gemini/config/plugins.json`:
```json
{
  "entries": [
    {
      "path": "C:/Users/MdAsif/Documents/Code/open-project-manager"
    }
  ]
}
```

### 2. Standalone Skill Installation (Codex / Agents)
Symlink or copy the skill directory into your user skills directory:
```powershell
# Windows PowerShell
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.agents\skills\open-project-manager" -Target "C:\Users\MdAsif\Documents\Code\open-project-manager\skills\open-project-manager"
```

---

## Usage

Invoke the skill explicitly using `$open-project-manager`:

- **Initialize a new or existing repository:**
  ```text
  $open-project-manager init
  ```
  Inspects the repository, detects tooling and RTK availability, asks for project requirements, and writes tailored templates into `.open-project-manager/`.

- **Plan the next phase or architectural step:**
  ```text
  $open-project-manager plan
  ```
  Reviews requirements in `.open-project-manager/`, establishes acceptance criteria, verifies dependencies, and generates a reviewable phase plan.

- **Execute an approved phase:**
  ```text
  $open-project-manager execute
  ```
  Implements the current phase with minimal edits, validates locally, and updates task progress.

- **Review and prepare for pull request / merge:**
  ```text
  $open-project-manager review
  ```
  Executes the uncommitted review loop, verifies findings, checks hard gates, and produces merge evidence.

---

## Customization Guide

Because all logic is Markdown, you can customize the plugin by directly editing files:

- **Global Agent Rules**: Edit [`rules/AGENTS.md`](rules/AGENTS.md) to adjust general agent behavior, platform rules, and operating principles.
- **Workflow References**: Edit markdown files under [`skills/open-project-manager/references/`](skills/open-project-manager/references/):
  - `lifecycle.md` — 17-step lifecycle
  - `pr-readiness.md` — PR gate checklist & review loop
  - `wayfinder.md` — Ambiguity and decision mapping
  - `security-baseline.md` — Security and dependency scanners
  - `rtk-guide.md` — Token-optimized command reference
  - `guardrails.md` — Non-destructive execution boundaries
- **Project Templates**: Edit files under [`templates/`](templates/) to change the starting structure of generated project documents (`AGENTS.md`, `SPEC.md`, `ROADMAP.md`, `TASKS.md`).
