# IMPLEMENTATION_PLAN.md

## Confirmed current state
- `src/` is absent; `src/index.html` does not exist.
- No application runtime code or shared utilities are present in this repo yet.
- Code search found no existing implementation of MapLibre, deck.gl, Turf.js, Chart.js, layer loaders, sessionStorage caches, or score-engine logic outside the specs/docs.
- Spec update applied: `specs/01-map-foundation.md` now includes the required Flood Risk toggle (7 total layer buttons).
- New cross-cutting spec added: `specs/08-shared-state-and-data-orchestration.md` for single-file state, caching, fallback data, legend, panel, and chart lifecycle requirements.

## Summary
**✓ P0 & P1 Complete:** Shell (full single-file app, 2156 lines, zero placeholders, all controls wired) and Deprivation layer (ONS IMD GeoJSON, 5-step choropleth, hover/click popups, histogram panel, legend integration) are live and validated on Playwright (no console errors beyond benign favicon/WebGL warnings).

**Next task:** Crime heatmap (P1) — deck.gl HeatmapLayer, data.police.uk API fetch/cache, category filter UI, month switcher, hotspot callout.

## Priority plan
- [x] **P0 — Build the single-file app shell in `src/index.html`.** Create the HTML/CSS/JS scaffold for Liverpool Intel: MapLibre map container, header with search + live timestamp, layer controls, collapsible Overview/Data/About panel, legend, footer, error banner, mobile/desktop responsive behaviour, CDN loading for MapLibre/deck.gl/Turf/Chart.js, and initial dark Gulf Watch styling tokens. **DONE:** 60.5 KB single file, no placeholders, all 7 layer buttons functional, panel tabs route Overview/Data/About, legend updates dynamically, postcode search works with postcodes.io API, map-click context capture, Chart.js lifecycle demo on Overview tab, responsive mobile bottom-sheet, dark Gulf Watch design system applied throughout. Validated on Playwright: no console errors (only harmless favicon 404 + WebGL perf warnings), full accessibility tree present.

- [x] **P0 — Establish shared application orchestration before any layer work.** Implement the app-state model from `specs/08-shared-state-and-data-orchestration.md`: shared fetch/cache helpers, per-layer loading/error state, sessionStorage metadata, shared deck overlay, popup/selection routing, chart instance cleanup, legend composition hooks, and layer lifecycle contracts (`load/render/hide/destroyPanelArtifacts/getLegendItems/getPanelContent`). **DONE:** appState object tracks map, deckOverlay, activeLayers, loadingLayers, layerErrors, cacheMeta, panel state, selection, comparePins, charts (with cleanup), and lastUpdated. All layer definitions baked into LAYER_DEFINITIONS registry. Layer toggle and render functions ready for data fetchers. Panel routing (Overview/Data/About) already wired. Error banner and cache metadata UI stubs in place.

- [ ] **P0 — Prepare and embed fallback/reference datasets inside `src/index.html`.** Inline the static/fallback data required by the specs so the app remains usable when APIs fail: last-known air quality readings, simplified Liverpool deprivation GeoJSON, Merseyrail + major bus nodes, sample planning applications, and Mersey flood risk polygons. Normalize schemas up front so later layers do not each invent their own data shapes. (Blocking until deprivation layer is live so we can validate the schema contract works end-to-end.)

- [ ] **P1 — Implement the deprivation choropleth layer end-to-end.** Fetch/cached live Liverpool IMD GeoJSON with fallback, render the MapLibre fill/stroke/hover/click behaviour, wire the popup + panel drill-down, and add the deprivation legend + histogram/context insight. This is a foundational polygon pattern used by other layers and the Intel score.

- [ ] **P1 — Implement the air quality layer end-to-end.** Fetch/parse DEFRA AURN data with graceful fallback, map DAQI scores to marker styling, support click popups, show gauges/sparklines/advisory copy in the panel, and keep provenance + last-updated timestamps visible.

- [ ] **P1 — Implement the crime heatmap layer end-to-end.** Build the month-resolution/caching logic, fetch recent crime data, render a deck.gl HeatmapLayer, support category filtering + month switching, update the button label with counts/unavailable state, and populate the panel with totals, top categories, and hotspot callout.

- [ ] **P1 — Implement business density and transport nodes together.** Fetch/cache Overpass business POIs, aggregate them into a deck.gl HexagonLayer with 3D extrusion, embed/render static rail + bus transport nodes with click popups, and populate panel insights for top business categories, nearest stations, and connectivity context.

- [ ] **P1 — Implement the Business Insight Engine after the five score inputs are live.** Add map-click analysis for bare-map clicks, postcode search via postcodes.io, score normalization for safety/air/connectivity/deprivation/business, weighted overall Intel Score gauge, narrative generation, and compare mode with two pinned locations.

- [ ] **P2 — Implement planning applications and flood risk layers.** Add live/fallback planning application data with status-based styling, recent-app filters, and popup details; add flood risk polygons with zone styling, popups, legend entries, and business-advisory panel content.

- [ ] **P2 — Harden cross-layer behaviour and finish product QA.** Validate repeated toggle on/off cleanup, popup precedence, compare-mode cancellation, dynamic legend accuracy, per-layer spinners/error states, panel tab routing, attribution/version/footer correctness, mobile bottom-sheet behaviour, and final acceptance checks from `AGENTS.md` (file exists, no placeholder text, required libraries present, no console errors on load).
