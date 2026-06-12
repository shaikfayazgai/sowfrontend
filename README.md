# sowfrontend — GlimmoraTeam super-admin platform

The deployable super-admin console + its backend for the GlimmoraTeam
(AI-governed global workforce) platform.

## Structure

| Folder | What it is |
|--------|------------|
| `newfrontend/` | **Super-admin** frontend (Next.js) — tenants, mentors, governance, rubrics, KYC, audit, etc. Plus per-role login pages. |
| `backends/super-admin/` | **Super-admin backend** (FastAPI) on Neon Postgres — auth, tenants, mentors, users, audit, governance. |

## Configuration

Real secrets are **not** committed. Each app ships an `.env.example`
(and `.env.local.example` for the Next.js frontend). Copy and fill in your values:

```bash
# backend (FastAPI)
cp backends/super-admin/backend/.env.example backends/super-admin/backend/.env

# frontend (Next.js)
cp newfrontend/frontend/.env.local.example newfrontend/frontend/.env.local
```

Required config: `DATABASE_URL` (Neon Postgres), `REDIS_URL`, `API_SECRET_KEY`
(must equal the frontend `AUTH_SECRET`), SMTP/email creds (onboarding emails),
Google OAuth, and the Vercel Blob token.

## Run (local)

```bash
# super-admin backend (port 8102)
cd backends/super-admin/backend && python -m uvicorn app:app --host 127.0.0.1 --port 8102

# super-admin frontend (port 3300)
cd newfrontend/frontend && npm install && npm run dev -- -p 3300
```

Open http://localhost:3300/admin/login.

## Deploy

- **Frontend** → Vercel (root: `newfrontend/frontend`). Set `AUTH_SECRET`,
  `BACKEND_SERVICE_URL` / `GLIMMORA_API_BASE_URL` → the backend URL.
- **Backend** → any Python host (root: `backends/super-admin/backend`,
  start: `python -m uvicorn app:app --host 0.0.0.0 --port $PORT`). Pin Python 3.12.
