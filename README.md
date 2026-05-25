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
