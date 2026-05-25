# TrailTrack

TrailTrack is a training project from the 10xdevs course.

For hobby hikers, runners, and cyclists, TrailTrack turns a `.gpx` file into an immersive 3D flythrough of the route — the path only, not activity analytics. Upload a track from a sport watch or route planner and relive or preview the trail without a paid subscription or a flat 2D map.

## MVP (v1)

- Sign up, sign in, and sign out (email + password)
- Upload a `.gpx` file from the device; each upload creates a private preview project
- Same-session 3D preview: route as a 3D curve with a highlighted point auto-moving forward along it
- Scrub the playback timeline to jump anywhere; releasing resumes forward play from that position
- Camera defaults to a navigation-style follow view (behind and above the point, trail ahead visible); drag/orbit/zoom to reposition freely; reset view restores follow
- My Projects list to reopen any saved preview
- Web app for desktop browsers (latest two major versions of Chrome, Edge, Firefox, Safari)

## Not planned

- Points of interest, waypoints, photos, or other per-point metadata beyond the route
- Performance, fitness, or biometric analysis (pace, heart rate, training metrics, etc.)
- External integrations (Strava, Garmin Connect, OAuth import, activity sync)
- URL or paste-based GPX import (device upload only)

## Planned for v2

- Playback controls beyond scrub: play/pause, step forward/back, reverse auto-play
- Camera tuning, keyframe authoring, and multiple presets (e.g. orbit vs follow)
- Share previews with non-account-holders (read-only links)
- Delete projects from My Projects
- Elevation profile chart synced with playback

## Typical flow

1. Sign in or register
2. Upload a `.gpx` file
3. View the 3D preview in the same browser session
4. Scrub the timeline and adjust the camera; use reset view to return to follow mode
5. Reopen the project later from My Projects

## Open ends

- It should not be impossible to create a twin version in form of a mobile application
- It should be possible to add a 3D model of the terrain later
- It should be possible to use the output directly, or convert it to a format that can be an input for a video renderer (a video composed from the 3D terrain model, the track and the camera movement customizations)

## Success criteria

- A signed-in user can upload a `.gpx` file and see a 3D preview in the same session, with the highlighted point auto-animating along the trail and the camera in the default follow view
- The user can scrub the timeline, freely reposition the camera, and reset view to re-engage follow mode
- Uploaded tracks and projects remain private to the account

## Foundation docs

Full product spec: [`context/foundation/prd.md`](context/foundation/prd.md)

# 10x Astro Starter

![](./public/template.png)

A modern, opinionated starter template for building fast, accessible web applications.

## Tech Stack

- [Astro](https://astro.build/) v6 - Modern web framework with server-first rendering
- [React](https://react.dev/) v19 - UI library for interactive components
- [TypeScript](https://www.typescriptlang.org/) v5 - Type-safe JavaScript
- [Tailwind CSS](https://tailwindcss.com/) v4 - Utility-first CSS framework
- [Supabase](https://supabase.com/) - Authentication and backend-as-a-service
- [Cloudflare Workers](https://workers.cloudflare.com/) - Edge deployment runtime

## Prerequisites

- Node.js v22.14.0 (as specified in `.nvmrc`)
- npm (comes with Node.js)

## Getting Started

1. Clone the repository:

```bash
git clone https://github.com/przeprogramowani/10x-astro-starter.git
cd 10x-astro-starter
```

2. Install dependencies:

```bash
npm install
```

3. Set up Supabase and configure environment variables — see [Supabase Configuration](#supabase-configuration) below.

4. Create a `.dev.vars` file for local Cloudflare dev secrets:

```bash
cp .env.example .dev.vars
```

5. Run the development server:

```bash
npm run dev
```

## Available Scripts

- `npm run dev` - Start development server (Cloudflare workerd runtime)
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run lint` - Run ESLint with type-checked rules
- `npm run lint:fix` - Auto-fix ESLint issues
- `npm run format` - Run Prettier

## Project Structure

```md
.
├── src/
│ ├── layouts/ # Astro layouts
│ ├── pages/ # Astro pages
│ │ └── api/ # API endpoints
│ ├── components/ # UI components (Astro & React)
│ └── assets/ # Static assets
├── public/ # Public assets
├── wrangler.jsonc # Cloudflare Workers config
```

## Supabase Configuration

This project uses [Supabase](https://supabase.com/) for authentication. Environment variables are declared via Astro's `astro:env` schema and are treated as **server-only secrets** — they are never exposed to the client.

### First-time setup (local, no cloud project needed)

Requires [Docker](https://www.docker.com/) and ~7 GB RAM.

1. Create your `.env` file:

```bash
cp .env.example .env
```

2. Initialize the local Supabase project (creates a `supabase/` config folder):

```bash
npx supabase init
```

3. Start the local stack (downloads Docker images on first run):

```bash
npx supabase start
```

4. Copy the credentials printed by the CLI into your `.env` and `.dev.vars`:

```
SUPABASE_URL=http://127.0.0.1:54321
SUPABASE_KEY=<anon key from CLI output>
```

5. To stop the stack when done:

```bash
npx supabase stop
```

The local Studio UI is available at `http://localhost:54323`.

No database tables or migrations are required — this project uses Supabase Auth's built-in `auth.users` table only.

### Using a cloud Supabase project instead

If you prefer to use a hosted Supabase project, add these variables to your `.env` and `.dev.vars` files:

| Variable       | Description                                                |
| -------------- | ---------------------------------------------------------- |
| `SUPABASE_URL` | Project URL from Supabase dashboard → Settings → API       |
| `SUPABASE_KEY` | `anon` public key from Supabase dashboard → Settings → API |

```
SUPABASE_URL=https://<project-ref>.supabase.co
SUPABASE_KEY=<anon-key>
```

### Email confirmation in local development

By default Supabase requires email confirmation before a user can sign in. To skip this during local development:

1. Open the Supabase dashboard for your project
2. Go to **Authentication → Email → Confirm email**
3. Toggle it **off**

Users can then sign in immediately after sign-up without clicking a confirmation link.

### Auth routes

| Route                 | Description                                                             |
| --------------------- | ----------------------------------------------------------------------- |
| `/auth/signin`        | Email/password sign-in form                                             |
| `/auth/signup`        | Email/password sign-up form                                             |
| `/auth/confirm-email` | Post-signup "check your inbox" page                                     |
| `/dashboard`          | Example protected page (redirects to `/auth/signin` if unauthenticated) |

Route protection is handled in `src/middleware.ts`. Add paths to the `PROTECTED_ROUTES` array there to require authentication.

## Deployment

This project deploys to [Cloudflare Workers](https://workers.cloudflare.com/).

1. Build the project:

```bash
npm run build
```

2. Deploy with Wrangler:

```bash
npx wrangler deploy
```

Set `SUPABASE_URL` and `SUPABASE_KEY` as secrets in your Cloudflare dashboard or via `npx wrangler secret put`.

## CI

GitHub Actions runs lint + build on every push and PR to `master`. Configure `SUPABASE_URL` and `SUPABASE_KEY` as repository secrets in GitHub for the build step.

## License

MIT
