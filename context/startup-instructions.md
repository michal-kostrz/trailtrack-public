# Startup Instructions

## Quick start

All commands run from the `code/` directory.

```bash
cd code
npm run dev
```

The dev server starts at **http://localhost:4321/** (falls back to 4322, 4323, etc. if ports are already taken).

---

## First-time setup: Supabase credentials

The app requires Supabase for authentication. Without credentials the server starts fine, but sign-up and sign-in return errors.

### Option A — Cloud Supabase (simplest, no Docker)

1. Create a free project at https://supabase.com
2. Go to **Project Settings → API** and copy:
   - **Project URL** → `SUPABASE_URL`
   - **anon / public key** → `SUPABASE_KEY`
3. Create `code/.dev.vars` (used by the Cloudflare dev runtime):

```
SUPABASE_URL=https://<your-project-ref>.supabase.co
SUPABASE_KEY=<your-anon-key>
```

4. Optionally turn off email confirmation for local dev:
   Supabase dashboard → **Authentication → Email → Confirm email → OFF**

5. Restart `npm run dev`.

### Option B — Local Supabase (requires Docker, ~7 GB RAM)

```bash
cd code
npx supabase start        # first run downloads Docker images — takes a few minutes
```

Copy the printed `API URL` and `anon key` into `code/.dev.vars` as above, then `npm run dev`.

Stop the local stack when done:

```bash
npx supabase stop
```

---

## Node version

The project targets Node **v22.14.0** (see `code/.nvmrc`). Node v24 also works for local dev.
To switch with nvm: `nvm use` inside `code/`.

---

## Available routes

| URL | Description |
|-----|-------------|
| `/` | Landing / home page |
| `/auth/signin` | Sign in |
| `/auth/signup` | Register |
| `/auth/confirm-email` | Post-signup confirmation notice |
| `/dashboard` | Protected — redirects to `/auth/signin` if not logged in |

---

## What doesn't need configuration

- Tailwind, React, and all other UI dependencies are bundled — no extra setup.
- No database migrations are needed; Supabase Auth uses its built-in `auth.users` table only.
- The Cloudflare adapter is used at build/deploy time; `npm run dev` uses a local Vite server.
