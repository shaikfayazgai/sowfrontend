# sowfrontend — GlimmoraTeam platform

One unified frontend (all role logins) + the role backends for the GlimmoraTeam
(AI-governed global workforce) platform.

## Structure

| Folder | What it is |
|--------|------------|
| `newfrontend/` | **The unified frontend** (Next.js) — a single app serving every role's sign-in and portal: super-admin (`/admin/login`), mentor (`/mentor/login`), enterprise (`/enterprise/login`), reviewer (`/reviewer/login`), contributor (`/auth/login`). Routing + role-scoped access is enforced by `proxy.ts` + `normalizeRole` in `auth.ts`. |
| `backends/` | The FastAPI backends on the shared Neon Postgres DB — `super-admin` (auth, tenants, mentors, users, audit, governance) plus the per-role backends (`enterprise`, `mentor`, `reviewer`, `freelancer`). |

## Logins (one app, all roles)

| Role | Sign-in route | Lands on |
|------|---------------|----------|
| Super-admin | `/admin/login` | `/admin/dashboard` |
| Mentor | `/mentor/login` | `/mentor/dashboard` |
| Enterprise | `/enterprise/login` | `/enterprise/dashboard` |
| Reviewer | `/reviewer/login` | `/enterprise/reviewer/queue` |
| Contributor | `/auth/login` | `/contributor/dashboard` |

All authenticate via NextAuth `credentials` → the backend `POST /api/v1/auth/login`
(shared `login_accounts`). `normalizeRole()` collapses tier roles
(`mentor.senior`/`mentor.lead` → `mentor`, `ent.*` → `enterprise`, etc.) so each
portal's guard accepts its role family.

## Configuration

Real secrets are **not** committed. Each app ships an `.env.example`
(`.env.local.example` for the frontend). Copy and fill in your values:

```bash
cp backends/super-admin/backend/.env.example backends/super-admin/backend/.env
cp newfrontend/frontend/.env.local.example   newfrontend/frontend/.env.local
```

Required: `DATABASE_URL` (Neon), `REDIS_URL`, `API_SECRET_KEY` (== frontend
`AUTH_SECRET`), SMTP/email creds (onboarding), Google OAuth, Vercel Blob token.

## Run (local)

```bash
# backend (port 8102)
cd backends/super-admin/backend && python -m uvicorn app:app --host 127.0.0.1 --port 8102

# unified frontend (port 3300)
cd newfrontend/frontend && npm install && npm run dev -- -p 3300
```

Open http://localhost:3300/admin/login (or /mentor/login, /enterprise/login, /reviewer/login).
