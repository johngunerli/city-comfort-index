# Weather Mapping

This workspace analyzes how many "comfortable" days (daily mean temperature between 65°F and 75°F) each of 210 U.S. cities experience between 2000 and 2019. The notebook pulls daily observations from NOAA Global Historical Climatology Network (GHCN) station files, computes comfort metrics, and produces interactive Plotly visualizations and a CSV summary ranked by average comfortable days per year.

## Repository Layout
- `Untitled-1.ipynb` – main analysis notebook; it loads the dataset, builds the comfort-day summary table, and renders the charts.
- `comfortable_days_summary.csv` – latest generated metrics for every city, including average comfortable days, data coverage, and helper comfort scores.
- `data/` – extracted dataset shipped with the project (`city_info.csv` plus one `<CITY_ID>.csv` per city). The original archive is kept as `7890488.zip`.
- `data/README.txt` – provenance and structure notes supplied with the dataset.

## Data Source
The meteorological records come from the "Compiled historical daily temperature and precipitation data for selected 210 U.S. cities" collection curated by Yuchuan Lai and David A. Dzombak (Carnegie Mellon University). The repository includes the raw NOAA GHCN station CSVs and the accompanying metadata file (`city_info.csv`). See `data/README.txt` for full attribution and variable descriptions.

## Major Outcomes (2000–2019)
- Pacific-coast metros dominate the comfort index; San Diego leads decisively thanks to frequent 65–75 °F days and low temperature variability.
- Northern California and coastal Oregon/Washington score well, trading fewer warm days for minimal swings relative to 70 °F.
- Maritime influences create the only high-scoring eastern outliers (Cape Hatteras, Pensacola, Gulfport, Galveston); purely continental cities rarely reach the top tier.
- Interior West, Midwest, and Northeast cities cluster at the low end of the map because sharp seasonal extremes keep them outside the comfort band most of the year.
- Florida and Gulf Coast metros land mid-pack: they hit the comfort band often but humidity and heat spikes suppress their composite scores, while desert Southwest cities score lowest due to persistent hot departures.

## Requirements
- Python 3.9+
- Python packages: `pandas`, `numpy`, `plotly`

You can install the dependencies into a virtual environment with:

```bash
python -m venv .venv
source .venv/bin/activate
pip install pandas numpy plotly
```

Plotly's Mapbox-based tiles require a Mapbox access token for high-resolution basemaps. Set it before running the notebook if you have one:

```python
import plotly.express as px
px.set_mapbox_access_token("YOUR_TOKEN")
```

The figures will fall back to Plotly's public tiles when no token is supplied, but the "satellite-streets" style may not render without it.

## Running the Analysis
1. Launch Jupyter:
   ```bash
   jupyter lab
   ```
2. Open `Untitled-1.ipynb` and run the single code cell. The notebook will:
   - Load city metadata and daily weather files.
   - Compute the average number of comfortable days per year for each city.
   - Save the refreshed `comfortable_days_summary.csv` to the repository root.
   - Render an interactive horizontal bar chart of the top cities and a satellite scatter map labeling each city with its state.

### Batch Execution
To automate the run without opening Jupyter, execute the notebook headlessly:

```bash
jupyter nbconvert --to notebook --execute Untitled-1.ipynb --output Untitled-1.out.ipynb
```

The command writes an executed copy of the notebook and refreshes the summary CSV and plots as when run interactively.

## Output Files
- `comfortable_days_summary.csv` – table keyed by city and state with calculated comfort-day metrics and auxiliary statistics.
- Interactive Plotly figures – displayed inline when the notebook runs. Save them manually via the Plotly mode bar if you need static exports.

## Customization Notes
- The comfort threshold (`LOW`, `HIGH`) and time window (`START_YEAR`, `END_YEAR`) are defined near the top of the notebook.
- State abbreviations are assigned via a `CITY_STATE_MAP` dictionary. If you add new cities or rename existing ones, update the mapping to keep labels accurate.
- The map uses `scatter_mapbox`; switch the `mapbox_style` or adjust `textfont` in `plot_satellite_map` to change the look or label handling.
