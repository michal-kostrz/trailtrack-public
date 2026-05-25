# TrailTrack

TrailTrack is a training project from the 10xdevs course.

The app is designed to generate 3D route previews from `.gpx` files exported by sport watches (for example Garmin). Users upload GPS tracks, process them, and customize how the camera moves along the route. The camera is going to follow a highlighted point that travels along the trail, and orbits around it. By default, the camera should follow the point, like in car navigation app.
The preview is displayed in an interactive preview component, with UI controls allowing to navigate forwards and backwards along the track, establishing keyframes for the camera movement, and using presets for 2 or 3 camera behaviors.
The application is accessible from a web browser.

## What It Does

- Allows users to create an account and log in
- Import `.gpx` tracks from sport watch activities
- Build a 3D visualization of the route
- Animate camera movement along the path
- Let users tune camera behavior (speed, angle, rotation, timing)
- Stores the project details in a format that allows easy integration with a 3D rendering library used in the interactive preview
- Nice to have: a chart showing height above sea level on the course of the track, synchronized with the 3D preview

## What it does not (in the early development phase)

- Include the points of interest or other details of the track other than the route itself
- Analyze the performance or fitness ratings reported by the sport watch
- Integrate with external systems in order to fetch additional information about the users and their activities
- Render the 3D model of terrain

## Typical Flow

1. Upload a `.gpx` file
2. Parse and process GPS track data
3. Render a 3D preview of the route
4. Configure camera movement settings
5. Export or share the final preview

## Open ends

- It should not be impossible to create a twin version in form of a mobile application
- It should be possible to add a 3D model of the terrain later
- It should be possible to use the output directly, or convert it to a format that can be an input for a video renderer (a video composed from the 3D terrain model, the track and the camera movement customizations)

## TODO: Decide Tech Stack

- [ ] Frontend framework
- [ ] 3D engine
- [ ] GPX parsing strategy/library
- [ ] Backend runtime
- [ ] Storage for uploads and processed projects
- [ ] Queue/background jobs for processing
- [ ] Authentication approach
- [ ] Deployment target

## Criteria of success

- A user is able to access the application on the internet, create an account and log in
- A user can upload a .gpx file and see a preview of the route
- A user can navigate the camera along the route forwards and backwards, seeing a highlighted point moving along it
- The basic experience is like when using a car navigation application, seeing a the highlighted point from behind and above (with some part of the trail ahead also visible)
