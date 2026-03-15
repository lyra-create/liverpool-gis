# Spec: Air Quality Layer

Real sensor data from DEFRA AURN network (Liverpool monitoring stations).

## Data Source
- DEFRA Air Quality API: `https://uk-air.defra.gov.uk/sos-ukair/api`
- Liverpool stations: AURN Liverpool Centre (LIN3), Merseyside Salford (MSL)
- Fallback: embed static readings from last known values if API unavailable
- Known station coords to hardcode:
  - Liverpool Centre: [-2.969, 53.404]
  - Liverpool Speke: [-2.850, 53.352]
  - Wirral Tranmere: [-3.009, 53.380]

## Readings
- PM2.5, PM10, NO2, O3 — show latest hourly reading
- UK Air Quality Index (DAQI) 1–10 scale

## Rendering
- MapLibre circle layer (not deck.gl — few points, need click interaction)
- Circle radius: 30px, colour by DAQI score:
  - 1-3 (Low): green (#22c55e)
  - 4-6 (Moderate): yellow (#ffd700)
  - 7-9 (High): orange (#ff8c00)
  - 10 (Very High): red (#ff3b3b)
- Pulsing animation on marker (CSS keyframe via MapLibre HTML marker)
- Popup on click: station name, DAQI score, PM2.5, PM10, NO2 values, last updated time

## Panel Content (when air quality layer active)
- DAQI gauge for each station
- Trend sparkline (last 24h if available, or last 7 days if daily)
- Health advisory: auto text based on current DAQI ("Air quality is currently LOW — suitable for outdoor exercise")

## Acceptance
- Markers visible on map
- Click shows popup with readings
- Panel shows DAQI gauges
- Graceful fallback if API down
