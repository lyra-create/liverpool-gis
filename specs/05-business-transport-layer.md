# Spec: Business Density + Transport Nodes Layer

Two complementary layers showing commercial activity and connectivity.

## Business Density (OSM via Overpass API)

### Data Source
- Overpass API: `https://overpass-api.de/api/interpreter`
- Query for Liverpool commercial POIs:
```
[out:json][timeout:25];
(
  node["shop"](53.35,-3.05,53.47,-2.87);
  node["amenity"~"restaurant|cafe|bar|pub|bank|pharmacy"](53.35,-3.05,53.47,-2.87);
  node["office"](53.35,-3.05,53.47,-2.87);
);
out body;
```
- Cache in sessionStorage key `osm_business`

### Rendering
- deck.gl HexagonLayer: aggregate business counts into hex bins
- Coverage: 200m radius hexagons
- Color: blue (#3b82f6) low → cyan (#06b6d4) medium → white high
- Elevation: 3D extrusion based on count (max 2000 units)
- Click hex: popup with top business types in that cell + count

## Transport Nodes

### Data Source
- Embed static GeoJSON: Liverpool Lime Street, Central, James Street, Moorfields, Liverpool South Parkway rail stations
- Merseyrail stations: embed ~68 stations on the Merseyrail network within Liverpool bounds
- Major bus interchange nodes: embed Lime Street, Queen Square, Sir Thomas Street stops
- No live API needed — static data is fine, locations don't change

### Rendering
- MapLibre symbol layer with custom circle markers (CSS-styled HTML markers)
- Rail stations: 🚉 icon, cyan colour
- Bus interchanges: 🚌 icon, blue colour
- Click popup: station name, lines served, approx weekly boardings (if known)

## Panel Content (when either layer active)
- Business layer: Top 5 business categories in current map view
- Transport layer: Nearest stations to map center with walking time (Turf.js distance calc)
- Combined insight: "Connectivity score" — areas with high transport + high business density

## Acceptance
- Hex bins render with 3D extrusion
- Transport markers visible and clickable
- Panel shows business breakdown
- No Overpass API rate limit issues (single fetch, cached)
