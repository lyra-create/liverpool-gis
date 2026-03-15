# Spec: Crime Heatmap Layer

Real crime data from data.police.uk (Merseyside Police).

## Data Source
- API: `https://data.police.uk/api/crimes-street/all-crime?lat=53.4084&lng=-2.9916&date=2024-11`
- Free, no API key. Returns JSON array of crimes with lat/lng and category.
- Fetch last 3 available months (API lags ~2 months). Try current month - 2, fallback to - 3.
- Cache in sessionStorage with key `crime_YYYY-MM`.

## Rendering
- deck.gl HeatmapLayer (not MapLibre native) — better for 1000+ points
- Color scale: low (blue) → medium (yellow) → high (red)
- Intensity: 1, radiusPixels: 40, threshold: 0.05
- On toggle off: remove deck layer cleanly

## Layer Toggle Behaviour
- Button shows spinner while fetching
- Shows crime count in button label once loaded: "🔴 Crime (4,231)"
- Error state: button turns amber + "Crime (unavailable)"

## Panel Content (when crime layer active)
- Crime stats card: total incidents, top 3 categories with counts + bar charts
- Month selector: dropdown for last 6 months (refetches on change)
- Category filter: checkboxes for crime types (violence, theft, ASB, etc.)
- Hotspot callout: "Highest density: [street/area name]"

## Acceptance
- Crime points render as heatmap
- Count shown in layer button
- Panel shows breakdown by category
- Month switching works
