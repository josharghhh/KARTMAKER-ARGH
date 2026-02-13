# KARTMAKER ARGH

Single-file Mario Kart/F1-style track maker with:
- 2D isometric spline editing
- 3D offline preview
- procedural track generation
- terrain + theme dressing
- OBJ export for Blender/Rhino and similar tools

The app runs fully in-browser from `index.html` (no external CDN or build step).

## Project Files

- `index.html`: main app
- `index - Copy.html`: backup/alternate copy

## Quick Start

1. Open `index.html` in a modern desktop browser (Chrome/Edge/Firefox).
2. Click `Generate Track` for a starting layout, or draw your own in 2D.
3. Refine shape/height/bank, then click `Export OBJ`.

Optional local server:

```bash
python -m http.server 8080
```

Then open `http://localhost:8080`.

## Core Workflow

1. Layout:
- Use `Draw`, `Move`, `Erase` in the 2D panel.
- Use `Join`, `Y Split`, `Roundabout` for branching track structure.

2. Shape:
- Select points and edit `Height (Y)` + `Bank (deg)`.
- Adjust Bezier handles in 2D or 3D for corner flow.

3. Generate details:
- Tune `Road width`, `Thickness`, `Spline smoothness`, `Path detail`.
- Enable/tune `Terrain` and `Theme`.

4. Place props:
- Switch to `Fence Place` or `Tree Place`, then click terrain in 3D.

5. Export:
- Click `Export OBJ` to download mesh output.

## Controls and Shortcuts

### 2D Editor

- Click: add/select points
- Drag: move selected point(s)
- `Shift`/`Ctrl` + click: multi-select
- `Space` + drag or `Shift` + drag: pan
- Mouse wheel: zoom
- `Alt` + wheel: adjust selected point height

### Bezier / Node Types

- Drag handle squares to shape curve
- `Alt` + drag handle: rotate-only (keep handle length)
- `1` = Smooth, `2` = Aligned, `3` = Corner

### 3D View

- `WASD` + `Q/Z`: move camera
- RMB or LMB drag: look
- MMB drag: pan
- `Alt` + RMB drag: orbit selected point
- RMB + wheel: camera speed up/down
- MMB click: reset camera speed
- `F`: focus selected point
- View button cycles: `Render` / `Normal` / `Wireframe`

### Editing Hotkeys

- `Ctrl/Cmd + Z`: Undo
- `Ctrl/Cmd + Shift + Z` or `Ctrl/Cmd + Y`: Redo
- `Delete`/`Backspace`: delete selected points/branch mids
- `Esc`: cancel fence segment start (in fence mode), close help modal

## Save / Load / Autosave

- `Save`: downloads `*.mktrack.json` and updates local browser autosave.
- `Load`: normal click loads a project file; `Shift` + click restores local autosave.
- Autosave runs periodically and on page unload.

## Export Details

`Export OBJ` includes:
- road top mesh
- optional road side walls
- generated terrain (if enabled)
- theme meshes (curbs/shoulders)
- placed props (fences/trees)

If browser download is blocked, use `Show OBJ` and copy text into a `.obj` file manually.

## GitHub Setup Guide

### 1) Create repository and push

```bash
git init
git add index.html "index - Copy.html" README.md
git commit -m "Add KARTMAKER ARGH app and docs"
git branch -M main
git remote add origin https://github.com/<your-user>/<your-repo>.git
git push -u origin main
```

### 2) Enable GitHub Pages (optional web hosting)

1. Open your repo on GitHub.
2. Go to `Settings` -> `Pages`.
3. Under "Build and deployment", set Source to `Deploy from a branch`.
4. Set Branch to `main` and Folder to `/ (root)`.
5. Save.
6. After deployment, open `https://<your-user>.github.io/<your-repo>/`.

### 3) Typical update flow

```bash
git add .
git commit -m "Describe your changes"
git push
```

## Tips

- Keep `Track name` clean; it is used as exported OBJ filename.
- Leave `Road iso snap` enabled for cleaner, grid-aligned edits.
- Raise `Path detail` and `Terrain detail` only as needed to keep performance smooth while editing.
