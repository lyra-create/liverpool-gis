# Spec: Planning Applications + Flood Risk Layers

Two additional intelligence layers for property/development decisions.

## Planning Applications

### Data Source
- Liverpool City Council open data: https://opendata.liverpool.gov.uk/
- Planning API: try `https://api.opendata.esriuk.com/arcgis/rest/services/Liverpool/Planning_Applications/FeatureServer/0/query?where=1%3D1&outFields=*&outSR=4326&f=geojson`
- Fallback: embed 20 sample recent planning applications as static GeoJSON
- Cache in sessionStorage key `planning_apps`

### Rendering
- MapLibre circle layer
- Colour by status:
  - Pending: yellow (#ffd700)
  - Approved: green (#22c55e)
  - Refused: red (#ff3b3b)
  - Withdrawn: grey (#6b7280)
- Circle radius: 8px, stroke 2px white
- Click popup: application ref, address, proposal description, status, decision date

### Panel Content
- Count by status (approved/pending/refused/withdrawn)
- Recent applications list: last 10 sorted by date
- Filter: by status, by type (residential/commercial/change of use)

## Flood Risk

### Data Source
- Environment Agency Flood Risk API: `https://environment.data.gov.uk/flood-monitoring/id/floodAreas?county=Merseyside`
- Flood zones GeoJSON: `https://environment.data.gov.uk/arcgis/rest/services/EA/FloodMapForPlanningRiversAndSea/MapServer/0/query?where=1%3D1&geometry=-3.05,53.35,-2.87,53.47&geometryType=esriGeometryEnvelope&outSR=4326&f=geojson`
- Fallback: embed key flood zone polygons for River Mersey frontage

### Rendering
- MapLibre fill layer
- Flood Zone 2 (medium risk): rgba(59,130,246,0.3) blue fill
- Flood Zone 3 (high risk): rgba(255,59,59,0.4) red fill
- Stroke: 1px solid matching colour at 0.8 opacity
- Click popup: zone type, risk description, link to EA flood map

### Panel Content
- Risk summary: "X properties in high flood risk zones"
- Business advisory: "Avoid Zone 3 for ground-floor retail/storage without flood resilience measures"

## Acceptance
- Planning circles render with correct status colours
- Flood zones render as semi-transparent polygons
- Both have working click popups
- Panel shows relevant breakdowns
