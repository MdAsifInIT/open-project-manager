# open-project-manager

Portable, markdown-driven project manager and workflow plugin for coding assistants.

## Included Skills

- **`open-project-manager`**: Project lifecycle manager (`init`, `plan`, `execute`, `review`). Sets up and manages `.open-project-manager/` in repositories.
- **`python-ai`**: AI backend (Python, FastAPI, OpenAI SDK) and frontend (TypeScript, Next.js, React).
- **`emil-design-eng`**: UI polish, interaction physics, and component mechanics.
- **`design-taste-frontend`**: Anti-slop frontend engineering for landing pages and interfaces.

---

## Installation

To install this plugin and its bundled skills, you have two options:

### Option 1: Register via plugins.json (Recommended for local dev)
Add the repository path to your global plugins configuration in `~/.gemini/config/plugins.json`:

```json
{
  "entries": [
    { "path": "C:/Users/MdAsif/Documents/Code/open-project-manager" }
  ]
}
```

### Option 2: Install directly to the plugins directory
Clone or copy this repository directly into your Antigravity global plugins directory:

```powershell
git clone https://github.com/MdAsifInIT/open-project-manager.git "$env:USERPROFILE\.gemini\config\plugins\open-project-manager"
```

*(Note: Installing the plugin automatically registers all included skills.)*
---

## Usage

- `$open-project-manager init` — Bootstrap `.open-project-manager/` in current project.
- `$open-project-manager plan` — Formulate phased plan with verified LTS dependencies.
- `$open-project-manager execute` — Implement approved phase with minimal edits.
- `$open-project-manager review` — Run uncommitted review loop and verify PR readiness.
- `$python-ai` — Build/troubleshoot FastAPI + OpenAI SDK + Next.js stack.
- `$emil-design-eng` — Polish UI interactions, transitions, and component feedback.
- `$design-taste-frontend` — Build distinctive landing pages and interfaces.
