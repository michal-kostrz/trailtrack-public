---
bootstrapped_at: 2026-05-25T20:22:19Z
starter_id: 10x-astro-starter
starter_name: 10x Astro Starter (Astro + Supabase + Cloudflare)
project_name: trailtrack
language_family: js
package_manager: npm
cwd_strategy: git-clone
bootstrapper_confidence: first-class
phase_3_status: ok
audit_command: npm audit --json
---

## Hand-off

Verbatim copy of `context/foundation/tech-stack.md` frontmatter and rationale.

```yaml
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
```

### Why this stack

Solo, after-hours, 3-week MVP with email+password auth (FR-001/002/003) and a 3D route-preview core ahead (FR-006/007/008), plus deferred-but-anticipated 3D-terrain map-provider integration and future video-generation enqueueing. 10x-astro-starter — Astro 6 + React 19 + TypeScript + Tailwind 4 + Supabase + Cloudflare — was picked from a constrained candidate set (Next, T3, React Router, Angular all surviving the agent-friendly gates) for three load-bearing reasons. First, opinionated full-stack: auth, Postgres, storage, and edge deploy are wired out of the box, so the 3-week budget goes to the 3D rendering work rather than infrastructure assembly. Second, the islands model isolates heavy client-only components — the Three.js / react-three-fiber scene and the future map-provider integration become opt-in client islands without polluting the rest of the app. Third, the React binding ecosystem (react-three-fiber, react-map-gl, resium) is the most mature for the upcoming 3D + map work. Forward compatibility for video-gen: Supabase Postgres can host a jobs table; a separate worker tier (e.g., Fly.io with FFmpeg) deploys outside Cloudflare when that v3 feature lands.

## Pre-scaffold verification

| Signal      | Value                                                                  | Severity | Notes                                                          |
| ----------- | ---------------------------------------------------------------------- | -------- | -------------------------------------------------------------- |
| npm package | not run                                                                | n/a      | cmd_template starts with `git clone`, no npm CLI to query      |
| GitHub repo | przeprogramowani/10x-astro-starter last pushed 2026-05-17T10:33:39Z    | fresh    | from card.docs_url; 8 days before bootstrap                    |

## Scaffold log

**Resolved invocation**: `git clone https://github.com/przeprogramowani/10x-astro-starter .bootstrap-scaffold && cd .bootstrap-scaffold && npm install`
**Strategy**: git-clone
**Exit code**: 0
**Files moved**: 18 top-level paths (10 root files + 6 directories + 2 files merged into existing `.vscode/`)
**Conflicts (.scaffold siblings)**: `CLAUDE.md.scaffold`, `README.md.scaffold`, `.vscode/settings.json.scaffold`
**.gitignore handling**: append-merged (cwd line `/.claude/skills` kept; scaffold lines appended under `# from 10x-astro-starter` separator)
**.bootstrap-scaffold cleanup**: deleted (cloned `.git/` removed before move-up to keep starter history out of the user's repo)

### Move-up detail

Moved silently (no cwd conflict):
- Files: `.env.example`, `.nvmrc`, `.prettierrc.json`, `astro.config.mjs`, `components.json`, `eslint.config.js`, `package-lock.json`, `package.json`, `tsconfig.json`, `wrangler.jsonc`
- Directories: `.github/`, `.husky/`, `node_modules/`, `public/`, `src/`, `supabase/`
- Into existing `.vscode/`: `extensions.json`, `launch.json`

Conflicts surfaced as `.scaffold` siblings (existing cwd version preserved):
- `CLAUDE.md` (cwd version is the 10xDevs Module 1 Lesson 2 guide; scaffold version documents the Astro+Supabase+Cloudflare starter conventions — review and merge manually)
- `README.md` (cwd version is the TrailTrack project README; scaffold version is the starter's default README)
- `.vscode/settings.json` (cwd version preserved; scaffold version sidelined for review)

`context/` was special-cased and untouched — the scaffold did not carry a `context/` tree.

## Post-scaffold audit

**Tool**: `npm audit --json`
**Summary**: 0 CRITICAL, 1 HIGH, 9 MODERATE, 0 LOW (total 10 across 895 dependencies)
**Direct vs transitive**: 0/0/2/0 direct of total 0/1/9/0 — both direct findings are moderate; the single HIGH is transitive

#### CRITICAL findings

None.

#### HIGH findings

- **devalue** (transitive, range `5.6.3 - 5.8.0`) — GHSA-77vg-94rm-hx3p — "Svelte devalue: DoS via sparse array deserialization" (CVSS 7.5). Fix available via `npm audit fix`.

#### MODERATE findings

- **@astrojs/check** (direct, range `>=0.9.3`) — via `@astrojs/language-server`. Fix advertised: downgrade to 0.9.2 (semver-major change).
- **wrangler** (direct, range `3.108.0 - 4.93.0`) — via `miniflare`. Fix available.
- **@astrojs/language-server** (transitive, range `>=2.14.0`) — via `volar-service-yaml`.
- **@cloudflare/vite-plugin** (transitive, range `<=0.0.0-fff677e35 || 0.0.7 - 1.37.2`) — via `miniflare`, `wrangler`, `ws`.
- **miniflare** (transitive) — via `ws`.
- **volar-service-yaml** (transitive, range `<=0.0.70`) — via `yaml-language-server`.
- **ws** (transitive, range `8.0.0 - 8.20.0`) — GHSA-58qx-3vcg-4xpx — "ws: Uninitialized memory disclosure" (CVSS 4.4). Fix available.
- **yaml** (transitive, range `2.0.0 - 2.8.2`) — GHSA-48c2-rrv3-qjmp — "yaml is vulnerable to Stack Overflow via deeply nested YAML collections" (CVSS 4.3).
- **yaml-language-server** (transitive) — via `yaml`.

#### LOW / INFO findings

None.

## Hints recorded but not acted on

| Hint                      | Value                                                                 |
| ------------------------- | --------------------------------------------------------------------- |
| bootstrapper_confidence   | first-class                                                           |
| quality_override          | false                                                                 |
| path_taken                | custom                                                                |
| self_check_answers.typed                | true                                                    |
| self_check_answers.from_official_starter| true                                                    |
| self_check_answers.conventions          | true                                                    |
| self_check_answers.docs_current         | true                                                    |
| self_check_answers.can_judge_agent      | false                                                   |
| team_size                 | solo                                                                  |
| deployment_target         | cloudflare-pages                                                      |
| ci_provider               | github-actions                                                        |
| ci_default_flow           | auto-deploy-on-merge                                                  |
| has_auth                  | true                                                                  |
| has_payments              | false                                                                 |
| has_realtime              | false                                                                 |
| has_ai                    | false                                                                 |
| has_background_jobs       | false                                                                 |

## Next steps

Next: a future skill will set up agent context (CLAUDE.md, AGENTS.md). For now, your project is scaffolded and verified — happy hacking.

Useful manual steps in the meantime:
- `git init` (if you have not already) to start your own repo history.
- Review any `.scaffold` siblings the conflict policy created and decide which version of each file to keep. The most consequential one here is `CLAUDE.md.scaffold`: the scaffold ships rich agent-context conventions for the Astro+Supabase+Cloudflare stack that complement (rather than replace) the existing cwd `CLAUDE.md`.
- Address audit findings per your project's risk tolerance — the full breakdown is in this log. `npm audit fix` will resolve several of the moderate transitives; the @astrojs/check direct moderate requires a semver-major downgrade, so weigh it explicitly.
- The starter ships a GitHub Actions workflow at `.github/workflows/ci.yml` (lint + build on push/PR to master) — wire up `SUPABASE_URL` and `SUPABASE_KEY` as repository secrets before enabling.
