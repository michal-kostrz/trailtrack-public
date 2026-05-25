---
starter_id: 10x-astro-starter
package_manager: npm
project_name: trailtrack
hints:
  language_family: js
  team_size: solo
  deployment_target: cloudflare-pages
  ci_provider: github-actions
  ci_default_flow: auto-deploy-on-merge
  bootstrapper_confidence: first-class
  path_taken: custom
  quality_override: false
  self_check_answers:
    typed: true
    from_official_starter: true
    conventions: true
    docs_current: true
    can_judge_agent: false
  has_auth: true
  has_payments: false
  has_realtime: false
  has_ai: false
  has_background_jobs: false
---

## Why this stack

Solo, after-hours, 3-week MVP with email+password auth (FR-001/002/003) and a 3D route-preview core ahead (FR-006/007/008), plus deferred-but-anticipated 3D-terrain map-provider integration and future video-generation enqueueing. 10x-astro-starter — Astro 6 + React 19 + TypeScript + Tailwind 4 + Supabase + Cloudflare — was picked from a constrained candidate set (Next, T3, React Router, Angular all surviving the agent-friendly gates) for three load-bearing reasons. First, opinionated full-stack: auth, Postgres, storage, and edge deploy are wired out of the box, so the 3-week budget goes to the 3D rendering work rather than infrastructure assembly. Second, the islands model isolates heavy client-only components — the Three.js / react-three-fiber scene and the future map-provider integration become opt-in client islands without polluting the rest of the app. Third, the React binding ecosystem (react-three-fiber, react-map-gl, resium) is the most mature for the upcoming 3D + map work. Forward compatibility for video-gen: Supabase Postgres can host a jobs table; a separate worker tier (e.g., Fly.io with FFmpeg) deploys outside Cloudflare when that v3 feature lands.
