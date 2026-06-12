# sowfrontend — GlimmoraTeam workforce platform

Per-role login systems + super-admin console + backend for the GlimmoraTeam
(AI-governed global workforce) platform.

## Structure

| Folder | What it is |
|--------|------------|
| `newfrontend/` | **Super-admin** frontend (Next.js) — tenants, mentors, governance, etc. + the super-admin backend scaffold. |
| `newfrontend2/` | Per-role **login portals** (Next.js) — `mentor/`, `enterprise/`, `reviewer/`, `contributor/`, each with its own role backend. |
| `backends/` | **Super-admin backend** (FastAPI) + the per-role backends (`enterprise`, `mentor`, `reviewer`, `freelancer`, `super-admin`) on the shared Neon Postgres DB. |

## Configuration

Real secrets are **not** committed. Each app ships an `.env.example`
(and `.env.local.example` for the Next.js frontends) listing the required keys.
Copy it and fill in your values:

```bash
# backend (FastAPI)
cp backends/super-admin/backend/.env.example backends/super-admin/backend/.env

# frontend (Next.js)
cp newfrontend/frontend/.env.local.example newfrontend/frontend/.env.local
```

Required config includes: `DATABASE_URL` (Neon Postgres), `REDIS_URL`,
`API_SECRET_KEY` (must match the frontend `AUTH_SECRET`), SMTP/email creds for
onboarding emails, Google OAuth, and the Vercel Blob token.

## Run (local)

```bash
# super-admin backend (port 8102)
cd backends/super-admin/backend && python -m uvicorn app:app --host 127.0.0.1 --port 8102

# super-admin frontend (port 3300)
cd newfrontend/frontend && npm install && npm run dev -- -p 3300
```
