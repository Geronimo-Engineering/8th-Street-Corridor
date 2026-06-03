# 8th Street — Agent Briefing

## Project
Leaflet.js interactive web map for a stormwater/drainage field survey along the 8th Street corridor in Oakland, CA. Forked from a prior project (Redwood Creek) — all Redwood Creek GIS data files in `data/` can be ignored or deleted.

**Map center:** 37.7993458754034, -122.2774721734447  
**Live file:** `index.html` — single-file static site, open via local server (`npx serve .` or `python -m http.server 8080`)

## Current State
The map currently has **basemaps only** (OpenStreetMap + ESRI Satellite toggle, compass rose, geolocation, right-click coordinates). No data layers yet.

## Your Task: Build the Photo/Camera Layer

### What it should do
- Place a camera icon marker on the map for each field photo
- Clicking a marker opens a popup showing the photo
- Markers should be grouped/clustered if photos are close together

### Photo folders
Two batches of field photos (flooding documentation, Jan 4 2026):

```
photos_01042026/01042026/   ← 12 photos (IMG_7261–IMG_7302)
photos_02042025/02042025/   ← 27 photos (IMG_7197–IMG_7223)
```

### How to get coordinates for each photo
Each photo has a **text overlay burned into the top-right corner** showing timestamp and location (e.g. `"Jan 4, 2026 at 1:35:25 PM / CA, Oakland, 8th St"`).

**Workflow:**
1. Read each photo using the Read tool (you can view images)
2. Extract the location text from the overlay
3. Geocode it via Nominatim — example PowerShell call:
   ```powershell
   Invoke-RestMethod -Uri "https://nominatim.openstreetmap.org/search?q=8th+St+Oakland+CA&format=json&limit=1" -Headers @{"User-Agent"="GeronimoEng/1.0"}
   ```
4. Build a `data/photos.json` GeoJSON FeatureCollection with each photo as a point feature:
   ```json
   {
     "type": "Feature",
     "geometry": { "type": "Point", "coordinates": [-122.277, 37.799] },
     "properties": {
       "file": "photos_01042026/01042026/IMG_7261.JPEG",
       "caption": "8th St & Jefferson, Jan 4 2026",
       "timestamp": "Jan 4, 2026 at 1:35:25 PM"
     }
   }
   ```
5. Load the GeoJSON in `index.html` and render as camera markers with photo popups

### Popup design
Each popup should show:
- The photo (as `<img>` tag, ~300px wide)
- Caption / timestamp from the overlay text
- Keep it simple — no lightbox needed

### Camera marker
Use a simple SVG camera icon or a colored circle marker with a 📷 label. Leaflet's `L.divIcon` works well for custom icons.

### Marker clustering
Use `leaflet.markercluster` CDN — photos are dense along the corridor so clustering avoids overlap:
```html
<link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css" />
<link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css" />
<script src="https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js"></script>
```

## Tech Stack
- Leaflet.js 1.9.4 (already in index.html)
- leaflet-rotate 0.2.8 (already in index.html)
- No build step — pure HTML/JS/CSS static site
- Git repo initialized, commit changes as you go

## Commit Style
```
git config user.email "andrew3000@berkeley.edu"
git config user.name "Andrew Glaros"
```

## Notes
- All Redwood Creek GeoJSON files in `data/` are legacy — ignore them
- The `tiles/` folder contains Redwood Creek elevation tiles — also ignore
- Keep `index.html` as a single file (no separate JS/CSS files)
- Photos are served as static files — paths in the GeoJSON should be relative to `index.html`
