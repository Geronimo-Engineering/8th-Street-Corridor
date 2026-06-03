# 8th Street Stormwater Survey Map

Interactive Leaflet.js field map documenting stormwater flooding along the **8th Street corridor in Oakland, CA**. Field photos from January 4, 2026.

## Features

- **Field photo layer** — 48 geo-tagged photos placed as camera markers along the corridor (Laney College → 536 8th St)
- **Lightbox viewer** — tap any marker to open a full-screen photo; swipe down to close, pinch to zoom on mobile
- **Marker clustering** — dense photo groups cluster automatically; zoom in or tap to expand
- **Address / coordinate search** — floating search bar; accepts street addresses or `lat, lng` pairs; drops a pulsing pin and flies to the result
- **Basemap toggle** — CartoDB Voyager (clean, no business clutter) and ESRI Satellite
- **Compass rose** — drag to rotate the map, click to snap back to north
- **Geolocation** — crosshair button tracks your live GPS position
- **Right-click coordinates** — right-click anywhere to copy lat/lng to clipboard
- **Mobile-responsive** — panel collapses to a slide-in drawer on narrow screens; larger touch targets, swipe/pinch gestures throughout

## Running Locally

Requires a local HTTP server (`fetch()` won't work from `file://`).

```bash
# Python — no install needed
python -m http.server 8080

# or Node
npx serve .
```

Open **http://localhost:8080**.

## Deploying to GitHub Pages

1. Push this repo to GitHub
2. Go to **Settings → Pages → Source → Deploy from branch → main / (root)**
3. Live at `https://<your-username>.github.io/<repo-name>/`

## File Structure

```
index.html                        — Single-file static map app
data/
  photos.json                     — GeoJSON FeatureCollection (48 photo points)
photos_01042026/
  01042026/                       — 48 JPGs with GPS address overlays (mapped)
  images without address/         — 12 JPEGs without location data (not plotted)
photos_02042025/
  02042025/                       — 27 JPEGs without location data (not plotted)
```

## Tech Stack

| Library | Version | Purpose |
|---|---|---|
| [Leaflet.js](https://leafletjs.com/) | 1.9.4 | Map rendering |
| [leaflet-rotate](https://github.com/fnicollet/leaflet-rotate) | 0.2.8 | Compass / map rotation |
| [Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster) | 1.5.3 | Photo clustering |
| [CartoDB Voyager](https://carto.com/basemaps/) | — | Default basemap (no API key) |
| [ESRI World Imagery](https://www.esri.com/) | — | Satellite basemap (no API key) |
| [Nominatim](https://nominatim.openstreetmap.org/) | — | Address geocoding (no API key) |

No build step — pure HTML/CSS/JS.

## Notes

- Photo coordinates are geocoded from the address text burned into each image by the iPhone camera app
- Live GPS tracking requires **HTTPS** in production (works on localhost without it)
