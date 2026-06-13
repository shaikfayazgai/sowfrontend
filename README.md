# GlimmoraTeam — Unified Frontend + Super-Admin Backend (snapshot)

A combined snapshot of the working code:

- **`frontend/`** — the unified Next.js 16 app (all role logins in one app:
  admin, enterprise, enterprise users, mentor, reviewer; `proxy.ts` portal guard).
- **`backend/`** — the FastAPI super-admin backend (runs all role services in one
  process: auth, super-admin, enterprise team, complaints, etc.) on shared Neon Postgres.

## Run

### Backend (`:8102`)
```bash
cd backend
cp .env.example .env   # fill in real DATABASE_URL / API_SECRET_KEY / SMTP creds
python -m uvicorn app:app --host 127.0.0.1 --port 8102
```

### Frontend (`:3300`)
```bash
cd frontend
cp .env.example .env.local   # fill in DATABASE_URL / AUTH_SECRET / BACKEND_SERVICE_URL
npm install
npm run dev
```

## Login doors
- Workspace admin → `/enterprise/login`
- Tenant users (PMO/Finance/Security/Legal/Sponsor) → `/enterprise/users/login`
- Reviewer → `/reviewer/login`
- Mentor → `/mentor/login`
- Platform super-admin → `/admin/login`

## Security
No real secrets are committed — only `.env.example` placeholders. Provide your own
`.env` / `.env.local` locally (both are gitignored).
