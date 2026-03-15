0a. Study `specs/*` using up to 10 parallel subagents to learn the application specifications.
0b. Study `IMPLEMENTATION_PLAN.md` (if present) to understand the current state.
0c. Study `src/` using up to 10 parallel subagents to understand existing code and utilities.

1. Compare specs against existing source code (gap analysis). Identify what is missing or incomplete. Use an Opus or high-reasoning subagent to prioritize tasks and create/update `IMPLEMENTATION_PLAN.md` as a bullet-point list sorted by priority. Ultrathink. Look for: TODOs, minimal implementations, placeholders, skipped tests, inconsistent patterns, missing features.

IMPORTANT: Plan only. Do NOT implement anything. Do NOT assume functionality is missing — confirm with code search first.

ULTIMATE GOAL: Build "Liverpool Intel" — a fully-featured, single-file HTML GIS dashboard (src/index.html) using MapLibre GL JS 4.7.1 + deck.gl 9.1.0 + Turf.js 7.0.0 + Chart.js 4.4. Dark, slick, Gulf Watch aesthetic. Shows real open data telemetry over Liverpool for business intelligence: crime heatmap, air quality sensors, deprivation choropleth, business density hexbins, transport nodes, planning applications, flood risk zones, and a composite Business Intel Score engine. All layers toggleable, panel-driven, with live API fetching and graceful fallbacks. If any specs are missing for required functionality, author them at `specs/FILENAME.md` first, then document the plan to implement them in `IMPLEMENTATION_PLAN.md`.

99999. Keep `IMPLEMENTATION_PLAN.md` current — future work depends on this to avoid duplicating effort.
999999. Do not add progress notes or status updates to `AGENTS.md` — operational guide only.
9999999. Output "PLANNING COMPLETE" on the final line when done.
