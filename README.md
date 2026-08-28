# open-project-manager

Portable, markdown-driven project manager and workflow plugin for coding assistants.

## Included Skills

- **`open-project-manager`**: Project lifecycle manager (`init`, `plan`, `execute`, `review`). Sets up and manages `.open-project-manager/` in repositories.
- **`python-ai`**: AI backend (Python, FastAPI, OpenAI SDK) and frontend (TypeScript, Next.js, React).
- **`emil-design-eng`**: UI polish, interaction physics, and component mechanics.
- **`design-taste-frontend`**: Anti-slop frontend engineering for landing pages and interfaces.

---

## Installation

### Antigravity / Gemini Plugin
Symlink into plugins directory:
```powershell
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.gemini\config\plugins\open-project-manager" -Target "C:\Users\MdAsif\Documents\Code\open-project-manager"
```
Or register in `~/.gemini/config/plugins.json`:
```json
{
  "entries": [{ "path": "C:/Users/MdAsif/Documents/Code/open-project-manager" }]
}
```

### Codex Skills
Symlink individual skills into `~/.agents/skills/`:
```powershell
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.agents\skills\open-project-manager" -Target "C:\Users\MdAsif\Documents\Code\open-project-manager\skills\open-project-manager"
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.agents\skills\python-ai" -Target "C:\Users\MdAsif\Documents\Code\open-project-manager\skills\python-ai"
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.agents\skills\emil-design-eng" -Target "C:\Users\MdAsif\Documents\Code\open-project-manager\skills\emil-design-eng"
New-Item -ItemType SymbolicLink -Path "$env:USERPROFILE\.agents\skills\design-taste-frontend" -Target "C:\Users\MdAsif\Documents\Code\open-project-manager\skills\design-taste-frontend"
```

---

## Usage

- `$open-project-manager init` — Bootstrap `.open-project-manager/` in current project.
- `$open-project-manager plan` — Formulate phased plan with verified LTS dependencies.
- `$open-project-manager execute` — Implement approved phase with minimal edits.
- `$open-project-manager review` — Run uncommitted review loop and verify PR readiness.
- `$python-ai` — Build/troubleshoot FastAPI + OpenAI SDK + Next.js stack.
- `$emil-design-eng` — Polish UI interactions, transitions, and component feedback.
- `$design-taste-frontend` — Build distinctive landing pages and interfaces.
