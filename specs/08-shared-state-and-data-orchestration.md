# Spec: Shared State, Caching, Fallback Data, and Panel Orchestration

Cross-cutting requirements for making Liverpool Intel work as a single-file HTML app without a build step.

## Single-File Constraint
- The entire application ships as `src/index.html`.
- CSS, JavaScript, SVG icons, and embedded fallback datasets must live inside that file.
- No runtime dependency on local JSON files, bundlers, or module loaders.
- External network requests are allowed only for CDN libraries, map tiles, fonts, and live data APIs defined in the layer specs.

## App State Model
Maintain a single in-memory state object that tracks at minimum:
- `map`: MapLibre instance
- `deckOverlay`: shared deck.gl `MapboxOverlay`
- `activeLayers`: toggle state for crime, air, business, deprivation, transport, planning, flood
- `loadingLayers`: per-layer loading booleans for button spinners
- `layerErrors`: per-layer error state/messages for button + banner rendering
- `cacheMeta`: timestamps / keys for sessionStorage-backed datasets
- `panel`: current tab (`overview`, `data`, `about`), open/closed state, active detail view
- `selection`: currently selected feature or clicked map location for Intel Score
- `comparePins`: up to 2 pinned comparison locations
- `charts`: active Chart.js instances so they can be destroyed/recreated cleanly
- `lastUpdated`: latest successful fetch/render timestamp shown in the header

## Fetch + Cache Behaviour
- Wrap live fetches in a shared helper with timeout, JSON/GeoJSON parsing, and consistent error objects.
- Use `sessionStorage` for the caches named in the layer specs.
- Add cache timestamps so the UI can show when data is stale.
- Reuse cached data when toggling a layer back on during the same session.
- If a live fetch fails and fallback data exists, render fallback data and surface the source as `fallback` in the panel.
- If no fallback exists, keep the layer toggle visible, show its unavailable state, and do not break the rest of the app.
- Any critical uncaught data failure should also raise the top error banner described in `01-map-foundation.md`.

## Fallback Data Bundles
`src/index.html` must embed fallback datasets for the layers that require them:
- Last-known air quality readings for Liverpool stations
- Simplified Liverpool deprivation polygons (minimum: top 20 most deprived LSOAs, preferred: all Liverpool LSOAs if size remains reasonable)
- Static Merseyrail + major bus interchange transport nodes
- Sample recent planning applications
- Key River Mersey flood risk polygons

## Layer Contract
Each layer implementation must provide a consistent contract:
- `load()` — fetch or resolve cached/fallback data
- `render()` — add/update MapLibre or deck.gl layers/markers
- `hide()` — remove visual artifacts cleanly when toggled off
- `destroyPanelArtifacts()` — destroy charts, listeners, and temporary UI owned by that layer
- `getLegendItems()` — return legend entries for the dynamic legend
- `getPanelContent()` — populate Overview/Data tab content when that layer or feature is active

## Panel + Chart Behaviour
- The right-side panel is the single host for charts, detail cards, compare mode, and data provenance.
- Chart.js instances must be destroyed before replacement to avoid duplicate canvases or memory leaks.
- Overview tab shows synthesized takeaways and score cards.
- Data tab shows raw counts, provenance, timestamps, and filters for the active layer.
- About tab explains methodology, score weights, source caveats, and fallback behaviour.
- When multiple layers are active, the panel should prioritize the currently selected feature/layer while still summarizing other active layers.

## Legend Orchestration
- The legend is dynamic and only shows entries for currently visible layers.
- Legend entries must match the exact colours/symbols used on the map.
- deck.gl layers with continuous ramps (crime heatmap, business hex bins) need gradient legend swatches.
- Polygon and point layers need labelled discrete legend chips.

## Interaction Rules
- Clicking a map feature shows that feature's popup/details first.
- Clicking bare map space triggers the Business Insight Engine score flow.
- Compare mode must not break normal single-click analysis; it temporarily routes map clicks into pin placement until two pins are set or compare mode is cancelled.
- Toggling a layer off must also remove its popup, hover state, markers, temporary filters, and legend entries.

## Acceptance
- A single `src/index.html` contains the full app and embedded fallback data.
- Layers can be toggled on/off repeatedly without duplicate markers, charts, or listeners.
- sessionStorage caches are reused within the session.
- If one data source fails, the rest of the app keeps working.
- Header timestamp, error banner, legend, and panel remain consistent as layers load and unload.
