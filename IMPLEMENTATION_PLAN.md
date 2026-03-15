# IMPLEMENTATION_PLAN.md

## Confirmed current state (2026-03-15)
- ✓ `src/index.html` exists: ~2415 lines, single-file GIS shell + deprivation layer + orchestration infrastructure
- ✓ No placeholders (TODO/FIXME/PLACEHOLDER), all required libraries loaded (MapLibre 4.7.1, deck.gl 9.1.0, Turf.js 7.0.0, Chart.js 4.4)
- ✓ `LayerRegistry` with lifecycle contracts: `load`, `hide`, `destroyPanelArtifacts`, `getLegendItems`, `getPanelContent`, `afterPanelRender`, `getDeckLayers`
- ✓ Shared `cachedFetch()` wraps `fetchJsonWithTimeout` + sessionStorage with `{ payload, ts }` metadata and TTL
- ✓ Popup tracking per layer key (`addLayerPopup`/`removeLayerPopups`/`removeAllPopups`), cleaned on deactivate
- ✓ Shared deck overlay management via `updateDeckOverlay()` collecting active registry module deck layers
- ✓ Chart cleanup helpers: `destroyLayerCharts(key)` targets layer-owned Chart.js instances
- ✓ `toggleLayer()` routes through registry: activate → `mod.load()`, deactivate → `mod.hide()` + `mod.destroyPanelArtifacts()` + `removeLayerPopups(key)`
- ✓ `renderLegend()` composes from registry `getLegendItems()`, falls back to `LAYER_DEFINITIONS[key].legends`
- ✓ `renderPanel()` tries registry `getPanelContent(tab)` first, then falls through to Overview/Data/About
- ✓ Deprivation layer refactored to register via `LayerRegistry.register('deprivation', {...})` — no monkey-patching
- ✓ 6 remaining layers (crime, air, business, transport, planning, flood) have stub registrations with lifecycle + panel content
- ✓ All prior monkey-patching (`baseToggleLayer`, `baseRenderPanel`, `baseRenderLegend`) eliminated
- ✓ Version: `0.2.0-orchestration`

## Priority plan
- [x] **P0 — Build the single-file app shell in `src/index.html`.** MapLibre map, header bar, layer controls, panel (Overview/Data/About tabs), legend, footer, error banner, postcode search, map-click capture, responsive layout. **DONE (2156 lines, no placeholders).**

- [x] **P0 — Establish shared application orchestration.** LayerRegistry with lifecycle contracts (`load`/`hide`/`destroyPanelArtifacts`/`getLegendItems`/`getPanelContent`/`afterPanelRender`/`getDeckLayers`), shared `cachedFetch` with sessionStorage metadata, popup tracking per layer, deck overlay management, chart cleanup helpers. Toggle/legend/panel all route through registry contracts. Monkey-patching eliminated. All 7 layers registered (deprivation fully, 6 stubs). **DONE (v0.2.0-orchestration).**

- [x] **P1 — Implement the deprivation choropleth layer end-to-end.** Fetch ONS IMD GeoJSON, render MapLibre fill/stroke, hover highlight, click popup, panel drill-down, histogram chart, legend. **DONE:** Fully functional, handles fetch/cache errors, integrates cleanly.**

- [ ] **P1 — Implement the air quality layer.** Fetch DEFRA AURN readings, render DAQI markers (colour by score), click popups, panel gauges/advisory, timestamps.

- [ ] **P1 — Implement the crime heatmap layer.** Fetch data.police.uk crime street API (monthly cache), render deck.gl HeatmapLayer, category filter checkbox UI, month switcher dropdown, button label with counts, panel totals/top categories.

- [ ] **P1 — Implement business density + transport nodes.** Fetch/aggregate Overpass POIs into deck.gl HexagonLayer (3D extrusion), embed static rail/bus nodes, click popups, panel top categories + nearest stations.

- [ ] **P1 — Implement the Business Insight Engine.** Composite score calculation (safety/air/connectivity/deprivation/business weights), Intel Score gauge, narrative generation, postcode search trigger, compare mode with 2-pin comparison.

- [ ] **P0 (Blocking) — Embed fallback datasets.** Static air readings, deprivation GeoJSON fallback, rail/bus nodes, sample planning apps, flood zone polygons. (Defer until next layer to refine schema contract.)

- [ ] **P2 — Planning + flood layers.** Planning app fetch/status styling, flood zone polygons, both with popups and legend entries.

- [ ] **P2 — QA and polish.** Validate toggle on/off cleanup, popup precedence, legend accuracy, error states, mobile layout, version footer, final AGENTS.md acceptance checks.
