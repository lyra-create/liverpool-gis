# IMPLEMENTATION_PLAN.md

## Confirmed current state (2026-03-15)
- ✓ `src/index.html` exists: 2156 lines, single-file GIS shell + deprivation layer module
- ✓ No placeholders, all required libraries loaded (MapLibre, deck.gl, Turf.js, Chart.js)
- ✓ App state model fully wired: appState tracks map, deckOverlay, layers, errors, cache, panels, selection, charts
- ✓ Layer orchestration pattern established via `layerModules` registry + function patching
- ✓ Deprivation layer live: ONS IMD GeoJSON fetch/cache, 5-class choropleth, hover/click popups, histogram, legend

## Priority plan
- [x] **P0 — Build the single-file app shell in `src/index.html`.** MapLibre map, header bar, layer controls, panel (Overview/Data/About tabs), legend, footer, error banner, postcode search, map-click capture, responsive layout. **DONE (2156 lines, no placeholders).**

- [x] **P0 — Establish shared application orchestration.** appState model, per-layer loading/error state, sessionStorage cache metadata, layer lifecycle hooks (load/hide), panel routing, legend composition. **DONE:** Patterns set; deprivation module demonstrates the contract.**

- [x] **P1 — Implement the deprivation choropleth layer end-to-end.** Fetch ONS IMD GeoJSON, render MapLibre fill/stroke, hover highlight, click popup, panel drill-down, histogram chart, legend. **DONE:** Fully functional, handles fetch/cache errors, integrates cleanly.**

- [ ] **P1 — Implement the air quality layer.** Fetch DEFRA AURN readings, render DAQI markers (colour by score), click popups, panel gauges/advisory, timestamps.

- [ ] **P1 — Implement the crime heatmap layer.** Fetch data.police.uk crime street API (monthly cache), render deck.gl HeatmapLayer, category filter checkbox UI, month switcher dropdown, button label with counts, panel totals/top categories.

- [ ] **P1 — Implement business density + transport nodes.** Fetch/aggregate Overpass POIs into deck.gl HexagonLayer (3D extrusion), embed static rail/bus nodes, click popups, panel top categories + nearest stations.

- [ ] **P1 — Implement the Business Insight Engine.** Composite score calculation (safety/air/connectivity/deprivation/business weights), Intel Score gauge, narrative generation, postcode search trigger, compare mode with 2-pin comparison.

- [ ] **P0 (Blocking) — Embed fallback datasets.** Static air readings, deprivation GeoJSON fallback, rail/bus nodes, sample planning apps, flood zone polygons. (Defer until next layer to refine schema contract.)

- [ ] **P2 — Planning + flood layers.** Planning app fetch/status styling, flood zone polygons, both with popups and legend entries.

- [ ] **P2 — QA and polish.** Validate toggle on/off cleanup, popup precedence, legend accuracy, error states, mobile layout, version footer, final AGENTS.md acceptance checks.
