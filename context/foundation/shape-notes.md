---
project: "TrailTrack"
context_type: greenfield
created: 2026-05-25
updated: 2026-05-25
product_type: web-app
target_scale:
  users: small
  qps: low
  data_volume: small
timeline_budget:
  mvp_weeks: 3
  hard_deadline: null
  after_hours_only: true
checkpoint:
  current_phase: 8
  phases_completed: [1, 2, 3, 4, 5, 6, 7]
  gray_areas_resolved:
    - topic: "Primary use case (review vs. planning)"
      decision: "Unified — both treated identically as path-only animations. The .gpx is parsed as a sequence of GPS points; timing/movement data from sports watches is intentionally ignored. This collapses 'review past activity' and 'preview planned route' into the same product."
    - topic: "Core pain shape"
      decision: "Cost — Strava's 3D preview is paywalled. A free / accessible alternative is the wedge."
    - topic: "Primary persona scope"
      decision: "Individual hobbyist athletes — yourself and peers (hikers, runners, cyclists). Web app fits this audience."
    - topic: "Auth shape"
      decision: "Email + password. Lowest external dependency; matches the README's 'create an account and log in' verbatim."
    - topic: "Sharing model (v1)"
      decision: "Deferred to v2. v1 ships private-preview only — no share links, no anonymous viewers. (Originally planned for v1; dropped during Socrates Batch 4 because deferring removes the public-viewer access path and a meaningful chunk of scope from a 3-week budget.)"
    - topic: "Roles (v1)"
      decision: "Single role — account-owner. Public viewer role removed for v1 because sharing is deferred to v2."
    - topic: "v1 scope ceiling"
      decision: "Auth + upload + render 3D scene + continuous forward auto-play of a highlighted point along the trail + scrub control for positioning + free camera with default 'follow' viewpoint and a reset-to-default action. No play/pause buttons, no forward/backward step buttons (scrub covers it), no camera presets (free camera replaces them), no sharing, no elevation chart, no project delete."
    - topic: "Timeline"
      decision: "~3 weeks of after-hours / weekend work. Standard course-project budget."
    - topic: "Project count per account"
      decision: "Multiple projects per account; the project list itself is the launcher (no separate 'open' action — clicking a list entry opens its preview)."
    - topic: "Project delete (v1)"
      decision: "Deferred to v2 — for a 3-week MVP, 'keep everything' is acceptable."
    - topic: "Playback model"
      decision: "Continuous forward auto-play. There is no play/pause button and no explicit forward/backward step button. The user controls position via a scrub slider on a timeline; releasing the scrub resumes forward auto-play from the new position."
    - topic: "Camera model"
      decision: "A single free 3D camera. On load, the camera is positioned at the default 'follow' viewpoint — behind and slightly above the moving highlighted point with the upcoming trail visible — and tracks the point. The user can freely reposition the camera via direct manipulation (drag/orbit/zoom); doing so detaches the camera from the follow behavior and leaves it static at the chosen viewpoint while the highlighted point continues to move. A 'reset view' control re-engages the default follow behavior."
    - topic: "Elevation chart (v1)"
      decision: "Deferred to v2 — explicitly dropped during Socrates round to keep v1 focused on the core 3D-replay value."
    - topic: "Path order vs. recording metadata"
      decision: "The sequential order of GPS points in the .gpx is essential and preserved — it defines the path itself. What is discarded is per-point timestamps and any recorded telemetry (speed, pace, heart rate, cadence, power, temperature, etc.). 'Order' is part of the path; 'when each point was recorded' is not."
  frs_drafted: 8
  quality_check_status: accepted
---

# TrailTrack — Shape Notes

## Vision & Problem Statement

Individual hobbyist athletes — hikers, runners, cyclists — who want to relive a past activity or preview a planned route in an immersive way today face a paywall: Strava offers a 3D route preview only behind a costly subscription. The free alternative is a flat 2D map that does not communicate the shape of the route, its turns, or its elevation profile in a way that feels like "being there." For athletes who want to revisit where they went, and for athletes planning a route in a tool like mapa-turystyczna.pl who want to show its shape to fellow hikers, runners, or cyclists, the choice today is either to pay a recurring subscription or to settle for a representation that loses what made the route interesting.

The insight is that the path itself — the ordered sequence of GPS points — is the only thing that matters for an immersive route preview. The movement information that sports watches record (timing, pace, biometrics) is irrelevant to a 3D camera animation that follows the trail. Treating the .gpx file as a path rather than as an activity log collapses two superficially different use cases — reviewing what you did, and previewing what you plan to do — into a single product: a 3D, navigation-style flythrough of any track, recorded or planned, with no vendor lock-in.

## User & Persona

**Primary persona — Hobbyist Athlete.** A hiker, runner, or cyclist who records their activities on a sport watch (e.g., Garmin) or who plans routes in third-party planning tools (e.g., mapa-turystyczna.pl). They want to relive a completed track or share the shape of a planned route with peers in the same hobby community. They reach for TrailTrack in two moments:

- After an activity, when looking at their flat 2D map and wishing they could "fly through" the route again.
- During or after route planning, when wanting to convey to fellow athletes what a route actually looks like before committing to it.

They are not professional content creators; video export is not their goal. They are not training analysts; performance data is not their goal. They care about the shape of the path.

## Access Control

TrailTrack v1 has a single user role:

- **Account-owner.** Authenticates with email + password. Can upload `.gpx` files, view their list of preview projects, and open any of their own projects to view the 3D preview. All content is private to the account-owner. Account-owners have authority only over content they themselves created.

Unauthenticated access to any application route (project list, upload, preview) redirects to sign-in. Sharing previews with non-account-holders is deferred to v2.

## Success Criteria

### Primary

- An account-owner can sign up, sign in, upload a `.gpx` file, and — within the same session — see a 3D preview of the route in which a highlighted point auto-animates forward along the trail. The 3D camera at first load is positioned at the default "follow" viewpoint (behind and slightly above the moving point, with the upcoming trail visible) and tracks the point.
- A user can scrub the playback timeline to position the highlighted point anywhere along the track, and can freely reposition the camera by direct manipulation; a "reset view" control re-engages the default follow behavior.

### Secondary

- (None for v1. All nice-to-have outcomes — elevation profile chart, sharing, camera presets, keyframe authoring — are deferred to v2.)

### Guardrails

- A user's uploaded tracks and projects remain private to that account; there is no shared/anonymous viewing path in v1.
- Playback of the 3D preview is visually smooth at default speed on a mainstream modern desktop browser — no stutter or freezes during normal scrubbing or camera repositioning.

## Functional Requirements

### Authentication & Account

- FR-001: A new user can register an account using an email address and a password. Priority: must-have
  > Socrates: Counter considered — "skip accounts entirely; share-link IS the project ID." Resolution: kept. Accounts are an explicit README success criterion and the share-link model is deferred to v2 anyway, leaving accounts as the persistence anchor.

- FR-002: A registered user can sign in with their email address and password. Priority: must-have
  > Socrates: No counter raised — sign-in is the natural pair of sign-up under the chosen auth shape.

- FR-003: A signed-in user can sign out. Priority: must-have
  > Socrates: No counter raised — table-stakes affordance once accounts exist.

### Project Management

- FR-004: A signed-in user can upload a `.gpx` file from their device to create a new preview project. Priority: must-have
  > Socrates: No counter raised — URL import and paste-as-text alternatives were considered and deferred as post-MVP nice-to-haves.

- FR-005: A signed-in user can view a list of their own preview projects, and clicking any entry opens that project's 3D preview. Priority: must-have
  > Socrates: Counter considered — "FR-006 (separate 'open project' action) was redundant with the list." Resolution: folded the open action into FR-005; the list itself is the launcher. Project deletion was also raised and deferred (former FR-007 → v2; see Non-Goals during Phase 6).

### Preview Rendering & Playback

- FR-006: The system renders the uploaded GPX path as a 3D scene containing the route curve and a highlighted point that auto-animates forward along the trail continuously. Priority: must-have
  > Socrates: No counter raised — auto-animation IS the product; without it, TrailTrack is just a map.

- FR-007: A user can scrub a playback timeline to position the highlighted point anywhere on the track; releasing the scrub resumes continuous forward auto-play from the new position. There is no explicit play/pause button and no explicit forward/backward step button — the scrub control covers all positioning needs. Priority: must-have
  > Socrates: Counter accepted on former FR-009 (play/pause): dropped in favor of continuous auto-play. Counter accepted on former FR-010 (forward/backward buttons): dropped in favor of forward-only auto-play with scrub for any-direction positioning. Resolution: collapsed the two FRs into this single playback-model FR.

### Camera

- FR-008: The 3D camera defaults at first load to a "follow" viewpoint — behind and slightly above the moving highlighted point, with the upcoming trail visible — and tracks the point. A user can freely reposition the camera by direct manipulation (drag/orbit/zoom); this detaches the camera from the follow behavior and leaves it static at the chosen viewpoint while the highlighted point continues to move along the trail. A "reset view" control re-engages the default follow behavior. Priority: must-have
  > Socrates: Counter accepted on former FR-011 (preset switching between "follow" and "orbit"): replaced with a single free camera whose default position is the "follow" viewpoint and which can be reset to it on demand. Orbit preset deferred to v2.

## User Stories

### US-01: Upload a track and watch its 3D preview

- **Given** an account-owner who has signed in
- **When** they upload a valid `.gpx` file from their device
- **Then** the system parses the file into a path, persists the new project to the user's account, and presents the 3D preview screen with the route rendered as a 3D curve and a highlighted point auto-animating forward along it under the default "follow" camera

#### Acceptance Criteria

- The 3D preview screen appears in the same browser session as the upload — no asynchronous wait that requires the user to return later.
- On the preview screen, the user can scrub the playback timeline to any position on the track; releasing the scrub resumes forward auto-play from the new position.
- On the preview screen, the user can drag the camera to reposition it freely (detaching from the follow behavior); a "reset view" control re-engages the default follow viewpoint.
- The newly created project appears in the user's "My Projects" list after upload; clicking it later reopens the same 3D preview.
- Handling of malformed or pathless `.gpx` files (e.g., files containing only timing/biometric data with no coordinate sequence) is captured in `## Open Questions`.

## Business Logic

TrailTrack treats a `.gpx` file as a pure path — the ordered sequence of GPS coordinates with elevation, in the order they appear in the file — discarding all per-point timestamps and any recorded telemetry (speed, pace, heart rate, cadence, power, temperature, etc.), and computes from that path a 3D scene containing the route as a 3D curve plus a highlighted point that auto-animates forward along the curve under a camera positioned by default to trail the point from behind and slightly above.

The user provides a single `.gpx` file uploaded from their device. From that file, the rule extracts only the ordered sequence of points and their elevation values; everything else is dropped.

What the user encounters as the rule's output is a navigable 3D preview screen: the route is drawn as a 3D curve in space, a highlighted point auto-animates forward along that curve continuously, and the camera at first load sits at the default "follow" viewpoint that conveys a navigation-app perspective on the next stretch of trail. The same rule applies regardless of whether the `.gpx` came from a recorded activity (sports watch) or from a route-planning tool — that distinction never enters the computation.

What the rule deliberately ignores is the temporal information about when each point was recorded (or whether it was recorded at all, vs. drawn in a planner) and any per-point telemetry. Discarding this is the rule, not an oversight; it is what unifies the "review past activity" and "preview planned route" use cases into one product surface.

## Non-Functional Requirements

- The highlighted point's auto-animation along the trail and any user-initiated camera repositioning are visually smooth without visible stutter at default playback speed, on a mainstream modern desktop browser.
- A user's uploaded `.gpx` files, derived path data, and any project state created from them are never visible to other accounts or to unauthenticated traffic.
- The product remains usable on the latest two major versions of the four mainstream desktop browsers (Chrome, Edge, Firefox, Safari).

## Non-Goals

The following are explicitly NOT in v1. Each carries a one-line rationale tying it to a prior decision or the README.

- **No camera tuning, keyframe authoring, or multiple camera presets.** The README's "speed, angle, rotation, timing" controls and the 2–3 preset selector are deferred. v1 ships a single free camera (default "follow" + reset). Picked during Phase 6 Non-Goals.
- **No 3D terrain rendering.** v1 renders only the route curve in 3D; surrounding terrain, mountains, and landscape are out. The README lists terrain as a future open-end. Picked during Phase 6 Non-Goals.
- **No performance, fitness, or biometric analysis.** Speed, pace, heart rate, cadence, training metrics — all out. README explicitly excludes; reaffirmed during Phase 6 Non-Goals.
- **No integration with external systems.** No OAuth-import-from-Strava, no Garmin Connect fetch, no third-party activity sync. The user uploads `.gpx` files directly. README explicitly excludes; reaffirmed during Phase 6 Non-Goals.
- **No sharing previews with non-account-holders.** No share links, no anonymous read-only viewer, no public-viewer role. Deferred to v2 during Socrates Batch 4.
- **No project delete action.** A signed-in user cannot remove their own projects in v1; "keep everything" is acceptable for the course-project budget. Deferred to v2 during Socrates Batch 2.
- **No elevation profile chart.** A chart of height-above-sea-level synced with playback was the README's stated nice-to-have; dropped during Socrates Batch 4 to keep v1 focused on the core 3D-replay value. Deferred to v2.
- **No video export.** Generating a renderable video from the 3D preview is excluded by the user from initial goals (content-creator use case deferred).
- **No mobile-app surface.** v1 is web-only on desktop browsers. The README lists a mobile twin as a future open-end.
- **No points of interest, waypoint annotations, photo pins, or any per-point detail beyond the bare route.** The README explicitly excludes all per-track metadata beyond the route itself.
- **No reverse auto-play and no explicit play/pause or step buttons.** Auto-play is continuous forward-only; the scrub control is the sole positioning affordance. Reshaped during Socrates Batch 3.

## Forward: technical-roadmap

These are forward-looking architectural constraints raised by the README's "Open ends" section. They are NOT v1 features and they are NOT part of PRD; they are informational notes for the downstream stack-selection and implementation-planning steps to consider so that v1 does not paint itself into a corner.

- **Mobile twin compatibility.** A future mobile-app version (iOS/Android) should be feasible without rewriting the core. Implication for downstream: prefer rendering/data-model choices that have a credible mobile path (e.g., a 3D library with mobile-capable runtimes) over choices that lock v1 to desktop-only technologies.
- **Terrain rendering extensibility.** Adding a 3D terrain model later should not require redesigning the scene. Implication for downstream: the scene-graph and project-data format should leave a seam where a terrain layer can be inserted under the route curve without invalidating existing projects.
- **Video-export-friendly project format.** The persisted project data (path + camera trajectory + any timing controls added later) should be either directly consumable by a video renderer or trivially convertible. Implication for downstream: prefer an open, declarative serialization (positions over time, camera keyframes if/when they exist) over an opaque internal format.

## Open Questions

1. **Handling of malformed or pathless `.gpx` files.** A `.gpx` that parses but contains no `<trkpt>` sequence (e.g., metadata-only or biometric-only exports) cannot be rendered. The expected behavior (clear error to the user vs. silent skip vs. partial-render attempt) is not yet specified. — Owner: user. Resolve before implementation begins.
2. **GPS-point density limits.** A typical recorded activity may have a few thousand points; an aggressively-sampled multi-hour activity could have tens of thousands. The point count at which rendering performance degrades, and the strategy for handling oversized tracks (downsample? reject? warn?), is not specified. — Owner: user / downstream stack selection.
3. **"Upcoming trail visible" quantification.** The default follow viewpoint specifies that "some of the upcoming trail" is visible from behind and above the highlighted point. The exact distance or angle has not been fixed. — Owner: user; resolvable during implementation by feel.
4. **Camera-reset transition.** When the user invokes "reset view" from a freely-positioned camera, the transition can be either instantaneous or animated. Not specified. — Owner: user; resolvable during implementation.



