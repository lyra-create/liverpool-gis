# Spec: Business Insight Engine

Composite scoring and actionable intelligence layer — the "so what" of the GIS.

## Concept
An "Area Intel" panel that synthesises all active data layers into a business-readable score and narrative for any clicked point or selected neighbourhood.

## Composite Score
When user clicks a point on the map, calculate:
- **Safety Score** (0-10): inverse of crime density at that point (high crime = low score)
- **Air Quality Score** (0-10): from nearest DAQI reading (DAQI 1 = 10, DAQI 10 = 1)
- **Connectivity Score** (0-10): based on distance to nearest transport node (Turf.js)
- **Deprivation Score** (0-10): IMD decile value (decile 10 = 10, decile 1 = 1)
- **Business Density Score** (0-10): relative hex bin count for that cell

**Overall Intel Score** = weighted average:
- Safety: 25%
- Deprivation: 25%
- Connectivity: 20%
- Business: 20%
- Air Quality: 10%

## UI
- Triggered by map click anywhere (when no specific layer feature is clicked)
- Shows a score card in the panel: circular gauge (SVG) with overall score
- Sub-scores shown as horizontal bars
- Narrative summary auto-generated based on scores:
  - High Safety + Low Business → "Underserved residential area — potential for retail/services"
  - High Business + Good Connectivity → "Commercial hub — high footfall opportunity"
  - High Deprivation + Low Business → "Social enterprise opportunity zone"
  - etc. (cover ~8 scenario combinations)

## Search
- Postcode search bar in header: enter a Liverpool postcode → map flies to it
- Use `https://api.postcodes.io/postcodes/{postcode}` (free, no key) to resolve to lat/lng
- After fly-to, auto-trigger the Intel Score for that location

## Layer Comparison Mode
- Button "Compare Areas" in panel
- User can pin 2 locations; side-by-side score comparison appears in panel
- Each pinned location shown as a numbered marker (1 / 2)

## Acceptance
- Click anywhere on map → Intel Score card appears
- All 5 sub-scores calculate correctly
- Narrative matches score profile
- Postcode search flies to location + shows score
- Compare mode pins 2 markers and shows side-by-side
