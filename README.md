# LILA BLACK — Player Journey Visualizer

A browser-based visualization tool that lets Level Designers explore player behavior across LILA BLACK's three maps using 5 days of production telemetry data.

## Live Demo

🔗 **[Open the tool →](https://connect-vijay.github.io/lila-black-visualizer/)*

## Features

- **Player path rendering** — Human and bot journeys plotted on accurate minimaps with correct world-to-pixel coordinate mapping
- **Bot vs Human distinction** — Humans shown as solid blue paths, bots as dimmed dashed gray paths
- **Event markers** — Kills (red diamond), deaths (gray X), loot (gold dot), storm deaths (purple bolt)
- **Filters** — Filter by map, date, and specific match
- **Timeline playback** — Play/pause with 1×/2×/5×/10× speed control, scrub through match progression
- **Heatmap overlays** — Toggle kill zones, death zones, loot hotspots, and traffic density across all matches
- **Match stats** — Per-match and per-map statistics panel
- **Hover tooltips** — Mouse over events to see player ID and timestamp

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML + CSS + JavaScript |
| Rendering | HTML5 Canvas (2D context) |
| Data | Pre-processed JSON (converted from Parquet via Python) |
| Hosting | Static files — GitHub Pages / Netlify / Vercel |

## Project Structure

```
lila-viz/
├── index.html          # Complete application (HTML + CSS + JS)
├── data.js             # Pre-processed game data (paths, events, matches, minimaps)
├── ARCHITECTURE.md     # Technical architecture document
├── INSIGHTS.md         # Three gameplay insights with evidence
└── README.md           # This file
```

## Setup

**No build step required.** This is a static site.

### Local development
```bash
# Option 1: Python
python3 -m http.server 8000
# Open http://localhost:8000

# Option 2: Node
npx serve .
# Open http://localhost:3000
```

### Deploy
Push to GitHub and enable GitHub Pages, or drag-and-drop to Netlify/Vercel.

### Regenerating data from source parquet files
If you need to rebuild `data.js` from the original parquet files:

```bash
pip install pyarrow pandas pillow
python3 build_data.py   # (conversion script — see ARCHITECTURE.md for the pipeline)
```

## Environment Variables

None required. All data is embedded in `data.js`.

## Browser Support

Tested on Chrome 120+, Firefox 120+, Safari 17+. Requires a modern browser with Canvas 2D support.
