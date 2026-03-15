# Spec: Map Foundation

The base map and app shell for Liverpool GIS.

## Requirements

- Full-viewport dark map (CartoDB dark_all, MapLibre GL JS 4.7.1)
- Fixed header bar: app name "Liverpool Intel" with red accent, live pulsing dot, last-updated timestamp
- Layer control panel (top-left): toggle buttons per data layer, each with a coloured dot indicator
- Info panel (right side, 380px): collapsible, tabbed (Overview / Data / About), slides in/out
- Panel toggle button (top-right)
- Legend (bottom-left): dynamic, updates based on active layers
- Footer bar: data attribution + version
- Responsive: panel slides from bottom on mobile (<768px), right side on desktop
- Map center: Liverpool [-2.9916, 53.4084], zoom 12
- Dark glassmorphism UI: same design tokens as Gulf Watch (see AGENTS.md)
- Loading state: per-layer spinner in layer button while fetching
- Error banner: fixed top banner (red) if any critical data source fails

## Layer Buttons (initial set, toggleable)
1. 🔴 Crime Heatmap
2. 🟡 Air Quality
3. 🟢 Business Density
4. 🔵 Deprivation Index
5. 🟣 Transport Nodes
6. ⚪ Planning Applications
7. 🌊 Flood Risk

## Acceptance
- Map renders on load, centered on Liverpool
- All 7 layer buttons visible and clickable
- Panel opens/closes smoothly
- Mobile layout works (panel from bottom)
- No console errors on load
