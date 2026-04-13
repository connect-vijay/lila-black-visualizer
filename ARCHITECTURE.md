# Architecture

## Tech Stack

| Layer | Choice | Rationale |
|-------|--------|-----------|
| Frontend | Vanilla HTML + CSS + JS | Zero build step, instant load, no framework overhead. Evaluators can open `index.html` and it works. |
| Rendering | HTML5 Canvas (2D) | Native browser API, handles thousands of path points at 60fps, full control over draw order and styling. |
| Data | Pre-processed JSON (`data.js`) | Parquet → JSON conversion done at build time via Python/PyArrow. Browser loads a single JS file — no WASM runtime, no async parsing. |
| Heatmap | Custom grid-based with Gaussian blur | Lightweight, no external library. 48×48 grid → blur → alpha-mapped fill gives smooth density visualization. |
| Hosting | GitHub Pages (or any static host) | Static files only — no server, no API, no database. Deploy by pushing to `main`. |

## Why This Stack Over Alternatives

**Why not React/Next.js?** The tool is a single view with a canvas and filters. React adds 40KB+ of runtime for component lifecycle management we don't need. Vanilla JS keeps the bundle small and the mental model simple.

**Why not deck.gl/Leaflet?** Both are excellent for geo-mapping, but our minimaps are game images with custom coordinate systems, not geographic tiles. A raw Canvas gives us direct pixel control over the UV→pixel transform without fighting a map library's projection system.

**Why not DuckDB-WASM?** The full dataset is ~5MB as JSON. At this size, in-memory JS filtering (`Array.filter`) runs in <1ms. DuckDB-WASM adds ~4MB of WASM binary for SQL capabilities we don't need.

## Data Flow

```
Parquet files (1,243 files, 89K events)
    ↓  Python + PyArrow (build-time)
    ↓  Extract, decode bytes→string, compute UV coordinates
    ↓
data.js (~5MB)
├── PATHS[]     → 1,242 player journeys, each with [u, v, t] coordinate arrays
├── EVENTS[]    → 16,045 discrete events (kills, deaths, loot, storm)
├── MATCHES[]   → 796 match summaries (map, date, player counts, duration)
└── MINIMAPS{}  → Base64-encoded JPEG minimap images (3 maps, ~120KB each)
    ↓  Browser loads via <script src="data.js">
    ↓
Canvas rendering
├── Map layer (bottom canvas) → minimap image
└── Draw layer (top canvas)   → paths, events, heatmap overlays
```

## Coordinate Mapping

This was the trickiest part. The README provides explicit conversion formulas:

**Step 1 — World to UV (0–1 normalized):**
```
u = (x - origin_x) / scale
v = (z - origin_z) / scale
```

**Step 2 — UV to minimap pixel:**
```
pixel_x = u * canvas_size + offset_x
pixel_y = (1 - v) * canvas_size + offset_y    ← Y-axis flipped (image origin is top-left)
```

**Per-map configuration:**

| Map | Scale | Origin X | Origin Z |
|-----|-------|----------|----------|
| AmbroseValley | 900 | -370 | -473 |
| GrandRift | 581 | -290 | -290 |
| Lockdown | 1000 | -500 | -500 |

**Key insight:** The `y` column in the data is elevation (vertical height in 3D), NOT a 2D coordinate. For minimap plotting, we use `x` and `z` only.

**Validation:** I verified coordinate accuracy by checking that kill events land on traversable map areas (roads, buildings, open ground) — not in walls or off-map.

## Assumptions

1. **Timestamps are match-relative ticks, not wall-clock time.** The `ts` column values are sequential integers incrementing by 5 per position sample. I treat these as abstract time units for timeline playback, not literal seconds.

2. **Bot detection uses filename/user_id pattern.** UUIDs = human, numeric IDs = bots, as specified in the README. I propagate this flag from the filename parsing during data conversion.

3. **February 14 is partial data.** The README notes this. No special handling needed — the tool simply shows fewer matches for that date.

4. **Heatmaps aggregate across all matches for the selected map.** This gives Level Designers a holistic view of where activity concentrates, rather than per-match noise.

## Tradeoffs

| Decision | Alternative considered | Why I chose this |
|----------|----------------------|------------------|
| All data in one `data.js` file | Lazy-load per match | 5MB loads in <1s on broadband; lazy loading adds complexity and latency per match switch |
| Pre-computed UV coordinates | Compute in browser | Saves repeated calculation; data conversion runs once at build time |
| Grid-based heatmap (48×48) | Canvas ImageData pixel-level | Grid is 10× faster to compute and the visual fidelity at 48 cells is sufficient for Level Designer use |
| Bot paths sampled every 2nd point | Full resolution | Bots are background context, not the focus. Halving their data saves ~15% payload with no visual loss |
| Single HTML + data.js (no build) | Webpack/Vite bundle | Evaluators can `git clone` → open `index.html` → it works. No `npm install`, no build step |
