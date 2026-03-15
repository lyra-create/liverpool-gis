# Spec: Deprivation Index Layer

ONS Index of Multiple Deprivation (IMD 2019) choropleth for Liverpool LSOAs.

## Data Source
- ONS IMD 2019 — embed Liverpool LSOA GeoJSON with IMD scores inline
- Liverpool has ~298 LSOAs — small enough to embed (~300KB GeoJSON)
- IMD decile 1 = most deprived, 10 = least deprived
- Source data: https://data-communities.opendata.arcgis.com/datasets/imd-2019

## GeoJSON Strategy
- Fetch from: https://services3.arcgis.com/ivmBBrHfQfDnDf8Q/arcgis/rest/services/Indices_of_Multiple_Deprivation_(IMD)_2019/FeatureServer/0/query?where=LAD19NM%3D'Liverpool'&outFields=*&outSR=4326&f=geojson
- If that fails, use a simplified embedded version with just the top 20 most deprived LSOAs as fallback polygons
- Cache in sessionStorage key `deprivation_lsoa`

## Rendering
- MapLibre fill layer (native — polygon choropleth)
- Color scale (5 classes):
  - Decile 1-2: #dc2626 (deep red — most deprived)
  - Decile 3-4: #f97316 (orange)
  - Decile 5-6: #eab308 (yellow)
  - Decile 7-8: #84cc16 (lime)
  - Decile 9-10: #22c55e (green — least deprived)
- Fill opacity: 0.6
- Stroke: rgba(255,255,255,0.15) 1px
- Hover: highlight with 0.85 opacity + cursor pointer
- Click popup: LSOA name, IMD score, national rank, decile

## Panel Content (when deprivation layer active)
- Distribution chart: histogram of Liverpool LSOAs by decile
- Context card: "X% of Liverpool LSOAs are in the most deprived 10% nationally"
- Business insight: "High deprivation areas may indicate opportunity for essential services, value retail, and social enterprise"
- Click-to-analyse: clicking an LSOA shows its breakdown (income, employment, health sub-domains if available)

## Acceptance
- Choropleth renders with correct colour scale
- Hover highlights LSOA
- Click shows popup + panel updates
- Legend updates to show deprivation scale
