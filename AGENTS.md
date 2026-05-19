# AGENTS.md

## Cursor Cloud specific instructions

### Project Overview

HAJIMI is a Gemini API Proxy built with FastAPI (Python 3.12) that provides OpenAI-compatible API endpoints for Google Gemini models. It includes a Vue 3 frontend dashboard for monitoring/configuration.

### Running the Backend

```bash
SKIP_CHECK_API_KEY=true GEMINI_API_KEYS="fake-key" .venv/bin/uvicorn app.main:app --host 0.0.0.0 --port 7860
```

- `SKIP_CHECK_API_KEY=true` bypasses API key validation at startup (useful for dev without real keys)
- Default password for API auth and dashboard: `123` (set via `PASSWORD` env var)
- Dashboard API is prefixed with `/api` (e.g., `/api/dashboard-data`)
- The OpenAI-compatible API is at `/v1/chat/completions` and `/v1/models`

### Running the Frontend Dev Server

```bash
cd hajimiUI && npx vite --port 5173 --host 0.0.0.0
```

The frontend builds into `app/templates/assets/` (served by FastAPI). To rebuild:
```bash
cd hajimiUI && npm run build
```

### Key Gotchas

- No test suite exists in this repository. Testing is done via API endpoint verification.
- No ESLint/linting configuration exists for the frontend or backend.
- The `page/` directory is an older/duplicate frontend; `hajimiUI/` is the active one.
- Storage persistence directory defaults to `/hajimi/settings/` — the app works without it (uses in-memory state).
- The app checks for valid Gemini API keys on startup unless `SKIP_CHECK_API_KEY=true`.
- `uv` must be on PATH (`$HOME/.local/bin`) — it's installed via pip as a user package.

### Package Managers

- Python: `uv` (lockfile: `uv.lock`, config: `pyproject.toml`)
- Frontend (hajimiUI): `npm` (lockfile: `package-lock.json`)
