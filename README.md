# GDP Evolution Explorer (D3.js)

## File structure

- `index.html` — fully self-contained single-page app (HTML/CSS/JS) using D3 v7 from CDN.
- `README.md` — run instructions + implementation notes.

## Run locally

Because the app fetches data from the World Bank API, run via a local web server:

```bash
python3 -m http.server 4173
```

Then open:

- `http://localhost:4173`

## How it works

### Data source

- Fetches annual GDP (`NY.GDP.MKTP.CD`) from the World Bank API for:
  - Spain, France, Germany, Italy, United States, United Kingdom, China, Japan.
- Time range is filtered from 1990 onward and extends to the latest year returned by the API.
- Per-country requests run concurrently (`Promise.all`) and are cached in-memory (`Map`) to avoid refetching.

### Scale and index toggles

- **Linear** (default): GDP in current USD on a linear y-axis.
- **Log**: GDP in current USD on a logarithmic y-axis to compare growth rates across different magnitudes.
- **Indexed to 100 (2000)**: each country is transformed to `(GDP / GDP_2000) * 100`; y-axis becomes index units.

### Zoom and overview brush

- A compact context chart below the main chart includes a brush.
- Drag across the brush window to set a focused year range in the main chart.
- **Reset zoom** clears the selected window and returns to full range.

### Main interactions

- Hover crosshair + tooltip shows yearly value for all selected countries.
- Tooltip includes per-country rank (based on raw GDP among selected countries in that year).
- Interactive legend supports:
  - Click to toggle a country on/off.
  - Shift-click to isolate one country.
- Optional controls:
  - Highlight Spain.
  - Toggle event annotations (2008 crisis, 2020 pandemic).
