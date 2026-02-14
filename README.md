# KARTMAKER ARGH

![Platform](https://img.shields.io/badge/Platform-Web-blue)
![Stack](https://img.shields.io/badge/Stack-HTML%20%7C%20CSS%20%7C%20JavaScript-1f6feb)
![Rendering](https://img.shields.io/badge/Rendering-2D%20%2B%203D-orange)
![Export](https://img.shields.io/badge/Export-OBJ%20%2B%20MTL-success)
![Build](https://img.shields.io/badge/Build-None-important)

Single-file browser app for building Mario Kart / F1-style tracks with 2D spline editing, 3D preview, procedural generation, and OBJ export with basic materials setup.

## Gallery

![Workspace](Screenshots/1.png)
![Road Editing](Screenshots/3.png)
![3D Preview](Screenshots/7.png)
![Advanced Scene](Screenshots/9.png)

## Why This Repo

- Fast in-browser workflow: draw in 2D, inspect in 3D, export immediately
- Advanced road tooling: Join, Y Split, Roundabout, lane/direction metadata
- Procedural generation with interchange templates and terrain/theming
- No build step, no external runtime dependency required

## Quick Start

1. Open `index.html` in Chrome, Edge, or Firefox.
2. Click **Generate Track** or draw manually.
3. Refine geometry and branching.
4. Click **Export OBJ**.

Optional local server:

```bash
python -m http.server 8080
```

Then open `http://localhost:8080`.

## Key Features

- 2D cubic Bezier road editing
- 3D offline software preview
- Road class, lane, and direction controls
- Lane-level network descriptor output
- Junction metadata and routing hooks
- Terrain, support styles, and prop placement/editing
- OBJ + MTL export

## Repo Layout

- `index.html` - full application
- `README.md` - documentation
- `Screenshots/` - repo gallery images

## Git

Initial push:

```bash
git init
git add index.html README.md Screenshots
git commit -m "Initial KARTMAKER ARGH commit"
git branch -M main
git remote add origin https://github.com/<user>/<repo>.git
git push -u origin main
```

Update flow:

```bash
git add .
git commit -m "Describe your change"
git push
```
