# Modelling the Mobility of the Wealthy Few: A Data‑Driven Study of Luxury Private Aviation and Business Applications
MSc IT for Business Data Analytics — Business Data Analytics Project  
International Business School (IBS), Budapest

---

## Project Overview

This project investigates Luxury Private Aviation (LPA) traffic patterns across European airspace using flight-level operational data from Eurocontrol's Open Performance Data Initiative (OPDI), covering January 2022 to December 2025.

A custom LPA identification methodology was developed to isolate LPA flights from broader air traffic using ICAO type codes and model keyword matching — addressing a gap in public aviation analytics where LPA traffic is rarely disaggregated at scale.

The project has two primary goals:
1. Conduct extensive EDA of LPA traffic to uncover behavioral patterns of the wealthy few customer segment (WFCS), providing actionable insights for luxury retail, charter, and investment companies.
2. Develop a 21-day horizon daily forecasting model for non-domestic LPA arrivals to France, with event-effect analysis — aligned with the operational planning cycle of private charter operators and luxury ground service providers.

---

## Business Problem

LPA traffic is proposed as a viable behavioral proxy for the wealthy few customer segment (WFCS). Their behavioral data is otherwise scarce due to privacy and access constraints, while LPA traffic is central to how this segment manages travel, business, and leisure. Despite growing environmental research on private aviation, business and economic dimensions remain underexplored.

The forecasting component supports:
- Airport operations and resource allocation
- Aircraft positioning and staffing decisions
- Inventory procurement for luxury ground services
- Event-driven traffic trend monitoring

---

## Dataset and Data Sources

Large raw datasets are excluded from the repository due to file size. **External enrichment datasets are included in `data/` and can be used directly.**

### 1. Eurocontrol OPDI Flight Data
Monthly Parquet files downloaded via the Eurocontrol public API on 13.03.2026, covering 48 months (Jan 2022 – Dec 2025). Retained fields: `id`, `icao24`, `dof`, `adep`, `ades`, `model`, `typecode`, `first_seen`, `last_seen`.

Source: `https://www.eurocontrol.int/performance/data/download/OPDI/v002/flight_list/`

### 2. Aircraft Reference Dataset
**File:** `data/ext_aircraft-database-complete-2025-08.csv`

OpenSky Network aircraft metadata database, accessed 13.03.2026. Joined to Eurocontrol data on `icao24` to fill missing `typecode` and `model` values, and to add `ext_owner`. Reduced missing typecodes by 32.6% and missing model values by 14.7%.

### 3. Airport Reference Dataset
**File:** `data/ext_airports.csv`

Combined file from ourairports.com (airports.csv + countries.csv + regions.csv), accessed 13.03.2026. Used to enrich flights with: airport name, municipality, country, region, latitude, longitude. 16 unmatched aerodrome codes were resolved via manual research and appended before merging.

### Final LPA Dataset
- **1,824,045 flights**
- **8,674 LPA aircraft**
- **61 countries**
- **886 municipalities**
- **936 aerodromes**
---

## LPA Identification Methodology

Custom classification using ICAO typecode matching (TM) and model keyword matching (MKM):

| Aircraft Family | Method |
|---|---|
| Gulfstream Classic | TM: G100, G150, G159, G280, ASTR, GALX, GLF2–GLF6 |
| Gulfstream Ultra-Long Range | TM: GA3C–GA8C |
| Bombardier Learjet | TM: LJ23–LJ75 |
| Bombardier Challenger | TM: CL30–CL65 |
| Bombardier Global | TM: GLEX, GL5T–GL8T |
| Dassault Falcon | TM: FA6X, FA7X, FA8X, F10X, FA10–F900 |
| Cessna Citation | TM: C500–C750 series |
| Embraer Phenom/Praetor | TM: E50P, E55P, E545, E550, P500, P600 |
| Airbus Corporate Jet (ACJ) | MKM: ACJ, AIRBUS CORPORATE JET |
| Boeing Corporate Jet (BCJ) | MKM: BCJ, BBJ, BOEING CORPORATE JET |

---

## Repository Structure

```text
luxury-private-aviation-forecasting/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── Modelling the Mobility of the Wealthy Few A Data‑Driven Study of Luxury Private Aviation and Business Applications.ipynb
├── data/
│   ├── README.md
│   ├── ext_aircraft-database-complete-2025-08.csv
│   └── ext_airports.csv
└── outputs/
    ├── figures/
    └── tables/
```

| Folder/File | Description |
|---|---|
| `README.md` | Main project documentation |
| `requirements.txt` | Python dependencies |
| `.gitignore` | Excludes large/generated files from Git |
| `notebooks/` | Main Jupyter notebook with the full workflow |
| `data/ext_aircraft-database-complete-2025-08.csv` | Aircraft metadata for LPA enrichment |
| `data/ext_airports.csv` | Airport reference data for geospatial enrichment |
| `outputs/figures/` | Exported visualizations |
| `outputs/tables/` | Generated analytical tables |

---

## Methodology

End-to-end analytics workflow:

1. Data acquisition — Eurocontrol OPDI monthly Parquet files via API
2. Data loading and preprocessing — column selection, datetime casting, format normalization
3. Aircraft enrichment — left join with OpenSky metadata on `icao24`
4. LPA identification — typecode + model keyword matching across 10 aircraft families
5. Airport enrichment — join with ourairports reference, manual resolution of 16 unmatched codes
6. Exploratory Data Analysis — traffic volume, seasonality, fleet analysis, ownership, WFCS mobilities
7. Time-series preparation — daily aggregation, linear interpolation for source data gaps, train/valid/test split
8. Forecasting model development — baseline, Prophet variants, LightGBM, Hybrid
9. Walk-forward validation — 21-day horizon, 9-step increments, out-of-sample evaluation
10. SHAP analysis — feature importance for LightGBM model
11. Final model validation on held-out test set

---

## Forecasting Approach

**Target:** Daily non-domestic LPA arrivals to France  
**Horizon:** 21 days  
**Validation:** Walk-forward cross-validation (9-step increments)  
**Split:** Train / Validation / Test

### Models Compared

| Model | Notes |
|---|---|
| Naive | Last observed value |
| Mean | Historical mean |
| Seasonal Naive (365d) | Same day last year — strong baseline given high seasonality |
| Prophet (multiple variants) | Default → multiplicative → tuned → with events → with event categories |
| LightGBM | Default and tuned; features: lags 21–27, rolling mean/std, calendar, event flags, `is_saturday` |
| Hybrid (Prophet + LightGBM) | Prophet handles trend/seasonality; LightGBM corrects residuals — did not outperform Prophet |

**Selected model: Prophet with Events (tuned multiplicative)**  
Seasonal Naive performed well as baseline, confirming strong seasonality. Prophet with events captured weekly and yearly seasonality with near-zero residual mean. LightGBM and the hybrid did not outperform it on this dataset.

### Events Feature Engineering
Built on Murphy & Mackley (2025) recommendations. Events grouped into categories (`cat_sport`, `cat_yachting`, etc.). Lower window: 3 days before event start (approximating early arrivals). Upper window: typical event duration (arrivals focus, not departures).

---

## Key EDA Findings

- LPA represents **4.16% of average daily European aviation traffic**
- Peak weeks: 22–30 (late May–late July) and 35–40 (late August–early October)
- Highest traffic days: Monday, Thursday, Friday, Sunday; lowest: Saturday, Tuesday
- Departure IQR: 8:00–15:00, median 12:00 — business hour travel dominates
- 31% of turnarounds under 2 hours — consistent with charter/repositioning operations
- 13.3% of aircraft account for the bulk of flights (500+ flights over the period); the majority are privately-owned occasional-use jets

---

## Technologies Used

Python · Google Colab · Pandas · NumPy · Matplotlib · Plotly · Scikit-learn · Statsmodels · Prophet · LightGBM · SHAP · PyArrow

---

## Reproducibility

### 1. Clone the repository
```bash
git clone <repository-url>
```

### 2. Create and activate a Python environment
```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# Mac/Linux
source .venv/bin/activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Update BASE to your local storage path
The notebook contains a data download section that fetches raw Eurocontrol Parquet files (~X GB total). Set the BASE variable at the top of the notebook to your local storage path before running due to large size.

### 5. Open the notebook
```text
notebooks/Modelling the Mobility of the Wealthy Few A Data‑Driven Study of Luxury Private Aviation and Business Applications.ipynb
```
Run cells sequentially.

---

## Notes on Data and Notebook

Large raw Eurocontrol Parquet files are excluded via `.gitignore`. External enrichment datasets are included in `data/` for convenience. The notebook handles all joining and preprocessing steps.

Notebook outputs were cleared intentionally to meet the size limits for git upload. 

---

## Academic Context

MSc IT for Business Data Analytics capstone project — International Business School (IBS), Budapest.  
Intended for academic assessment, reproducibility, and demonstration of analytical methodology.

---

## AI Usage Disclosure

AI-assisted tools were used during parts of the coding and workflow refinement process, in accordance with the Business Data Analytics Project Handbook guidelines. All generated code, outputs, interpretations, and analytical decisions were reviewed, modified, validated, and integrated by the author.

---

## Author

ABDUBALIEVA, Akmaral  
MSc IT for Business Data Analytics  
International Business School (IBS), Budapest
