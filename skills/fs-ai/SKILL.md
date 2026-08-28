---
name: fs-ai
description: Build and troubleshoot full stack AI applications using Python (FastAPI, OpenAI SDK) and TypeScript (Next.js, React).
---

# fs-ai

## Stack Defaults

- **Backend:** Python (FastAPI, Pydantic v2, `uvicorn`), official OpenAI SDK (`openai`), `uv` for dependency management.
- **Frontend:** TypeScript, Next.js (App Router, Server Components), React, Tailwind CSS.

## Workflow

1. Gather provider, model, dependency, prompt, tool schema, and data-flow context.
2. Verify dependency versions against current LTS/stable releases (FastAPI, OpenAI SDK, Next.js).
3. Implement minimal changes to model parameters, structured outputs, or API endpoints.
4. Validate deterministic code paths and mock model calls in unit tests.

## Diagnostics

```bash
# Python / FastAPI / AI Backend
uv run python --version
uv pip list
uv run pytest
uv run ruff check .
uv run mypy .
env | grep -E 'OPENAI|MODEL|API_KEY'

# Frontend / Next.js
pnpm run lint || npm run lint
pnpm run build || npm run build
pnpm test || npm test
```

## Safety Rules

- Never commit API keys, tokens, secret prompts, or private data fixtures.
- Never make live model calls in standard unit test suites.
- Use Pydantic models for structured outputs (`response_format` / `parse`).
- Confirm OpenAI API methods, parameter names, and SDK models against official SDK documentation.
- Keep tool definitions narrow, deterministic, and logged without secrets.

## Validation Checklist

- [ ] Tool schemas and parser tests pass.
- [ ] Pydantic validation handles malformed responses cleanly.
- [ ] Live smoke tests are gated behind explicit environment variables (`RUN_LIVE_TESTS=1`).
- [ ] Streaming endpoints (FastAPI `StreamingResponse` / Next.js AI SDK) handle disconnections cleanly.
