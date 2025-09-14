# Transport & Income Analysis

A lightweight exploratory project linking London transport station characteristics to local income or deprivation metrics.

## Data

Primary dataset: `data/stations.csv` (sample fields)
- geometry (WGS84 POINT long lat)
- fare_zone
- line (e.g. Piccadilly; Northern; District)
- name
- wheelchair (yes / no / limited)
- wikidata / wikipedia
- operator / network
- addr:city / addr:postcode
- accessibility notes (wheelchair:description)
- interchange attributes (alt_name, old_name, platforms)

Supplementary income / deprivation data (to be added): expected at LSOA / MSOA level.

## Objectives

- Standardise station attributes.
- Geo-join stations to socioeconomic polygons.
- Derive accessibility & connectivity indicators.
- Model relationships between transport accessibility and income metrics.

## Quick Start

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python scripts/clean_stations.py
python scripts/join_income.py
```

## Cleaning Steps (planned)

1. Parse geometry to latitude / longitude.
2. Explode multi-line fields (line) to tidy form.
3. Normalize accessibility flags (wheelchair, wheelchair:description).
4. Infer categorical indicators (has_step_free, has_ramp).
5. Attach area codes via spatial join (GeoPandas).
6. Aggregate income metrics per station catchment.

## Feature Ideas

- distance_to_zone1
- num_lines
- is_step_free (yes / limited mapping)
- operator_type (Underground / National Rail / DLR / Overground)
- fare_zone_numeric
- wifi_available flag (internet_access fields)
- accessibility_score (rule-based)

## Modeling Sketch

- Target: median household income or IMD decile.
- Models: baseline OLS, regularized regression, tree ensemble.
- Evaluation: cross-validated R², permutation feature importance, SHAP.

## Example (pseudocode)

```python
import pandas as pd
import geopandas as gpd

stations = pd.read_csv("data/raw/stations.csv")
# parse coords
stations[["lon","lat"]] = stations["geometry"].str.extract(r'POINT \((-?\d+\.\d+) (\d+\.\d+)\)').astype(float)
stations["num_lines"] = stations["line"].fillna("").str.split(";").apply(lambda x: 0 if x==[""] else len(x))
```
