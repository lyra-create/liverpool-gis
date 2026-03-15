# AGENTS.md — Liverpool GIS

## Project
Single-file HTML GIS dashboard for Liverpool, UK. Open data telemetry + business intelligence.

## Stack
- MapLibre GL JS 4.7.1 (base map, vector layers, popups)
- deck.gl 9.1.0 (heatmaps, scatterplots, 3D extrusions via MapboxOverlay)
- Turf.js 7.0.0 (spatial calculations, buffers, composite scoring)
- Chart.js 4.4 (side panel charts)
- Inter + JetBrains Mono fonts (Google Fonts CDN)
- All via CDN — no bundler, no build step

## Output
Single file: `src/index.html`

## Validation
```bash
# Check file exists and is valid HTML
[ -f src/index.html ] && echo "OK: file exists" || echo "MISSING"
# Check no placeholder text
grep -i "TODO\|PLACEHOLDER\|FIXME" src/index.html && echo "WARNING: placeholders found" || echo "OK: no placeholders"
# Check key libraries loaded
grep -c "maplibre-gl\|deck.gl\|chart.js\|turf" src/index.html
```

## Design System (Gulf Watch pattern)
```css
--bg: #0a0c0f
--surface: #12151a
--surface-2: #1a1f28
--border: rgba(255,255,255,0.08)
--text: #e8eaf0
--text-muted: #8892a4
--accent-red: #ff3b3b
--accent-orange: #ff8c00
--accent-yellow: #ffd700
--accent-green: #22c55e
--accent-blue: #3b82f6
--accent-cyan: #06b6d4
--accent-purple: #a855f7
```
Fonts: Inter (UI), JetBrains Mono (data/coordinates)

## Map Config
- Center: [-2.9916, 53.4084] (Liverpool)
- Default zoom: 12
- Min zoom: 10, Max zoom: 18
- Base: CartoDB dark_all tiles (free, no API key)

## Data Sources (all free, no API key unless noted)
- **Crime:** https://data.police.uk/api/crimes-street/all-crime?lat=53.4084&lng=-2.9916&date=YYYY-MM
- **Air quality:** https://environment.data.gov.uk/air-quality/so/AURN_Liverpool (DEFRA AURN)
- **Deprivation:** ONS IMD 2019 — use embedded GeoJSON (neighbourhood polygons)
- **OSM/Places:** Overpass API — https://overpass-api.de/api/interpreter
- **Transport:** https://transportapi.com/v3/uk/train/station/LIV (free tier) or hardcoded stops
- **Planning apps:** Liverpool Open Data — https://liverpool.gov.uk/planning (scrape or embed)

## Data Strategy
- Fetch live where free APIs allow (crime, air quality)
- Embed static GeoJSON for deprivation / neighbourhood boundaries (avoids CORS, fast load)
- Show loading spinners per layer; graceful fallback if API fails
- Cache fetched data in sessionStorage to avoid re-fetching on layer toggle

## Commit Convention
- feat: new feature
- fix: bug fix
- data: data layer / source changes
- style: visual/CSS only
