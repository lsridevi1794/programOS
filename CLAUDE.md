# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ProgramOS is an AI-powered enterprise program management platform. MVP features: program creation, dashboards, milestones, risk register, stakeholders, status updates, AI summaries, role-based access, and weekly report export.

Stack: Next.js (frontend) + FastAPI (backend) + PostgreSQL + Claude (Anthropic) API.

## Commands

### Frontend (`frontend/`)

```bash
npm run dev      # Start dev server
npm run build    # Production build
npm run start    # Serve production build
npm run lint     # ESLint
```

### Backend (`backend/`)

```bash
# Activate virtual environment first
source .venv/bin/activate

# Run dev server
uvicorn main:app --reload

# Run tests
.venv/bin/pytest -v
```

## Coding Workflow

Before committing any changes, follow this loop:

1. **Build** — run `npm run build` (frontend)
2. **Lint** — run `npm run lint` (frontend)
3. **Test** — run `.venv/bin/pytest -v` (backend) from `backend/`
4. **Show results** to the user and wait for explicit approval
5. **Commit** only after approval is given

A pre-commit hook in `.claude/settings.json` enforces this automatically — it runs build + lint + pytest before every `git commit` and blocks if any step fails.

**Backend testing standards:**

- Tests live in `backend/tests/`; one `test_*.py` file per module (e.g., `test_main.py` for `main.py`)
- Use `fastapi.testclient.TestClient` for endpoint tests
- Every new endpoint or business logic function must have a corresponding test before committing

## Architecture

### Frontend

- Next.js 16 with App Router (`src/app/`)
- TypeScript strict mode; path alias `@/`* → `./src/*`
- Tailwind CSS 4 for styling

**Critical**: This project uses Next.js 16.2.4, which has breaking changes from prior versions. Before writing any Next.js code, read the relevant guide in `frontend/node_modules/next/dist/docs/` — APIs, conventions, and file structure may differ from training data.

### Backend

- FastAPI app defined in `backend/main.py`
- Python 3.14 virtual environment at `backend/.venv/`
- Database: PostgreSQL (connection string via `DATABASE_URL` env var)
- AI: Claude (Anthropic) API (via `ANTHROPIC_API_KEY` env var)

### Environment Variables

Copy `.env.example` to `.env` and populate:

```
DATABASE_URL=       # PostgreSQL connection string
ANTHROPIC_API_KEY=  # Claude (Anthropic) API key
AUTH_PROVIDER_KEY=  # Auth provider secret
```

