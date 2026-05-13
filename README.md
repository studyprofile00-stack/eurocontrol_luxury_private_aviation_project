# Luxury Private Aviation Forecasting

MSc IT for Business Data Analytics — Business Data Analytics Project  
International Business School (IBS), Budapest

## Project Overview

This project investigates Luxury Private Aviation (LPA) traffic patterns in Europe using flight-level operational data and forecasting techniques. The main objective is to identify and analyse non-domestic luxury private aviation activity and develop forecasting models capable of predicting future traffic trends.

The project combines data preprocessing, exploratory data analysis (EDA), feature engineering, time-series analysis, and machine learning forecasting approaches within a reproducible Python workflow.

The work was completed as part of the MSc in IT for Business Data Analytics at International Business School (IBS).

---

## Business Problem

Luxury private aviation represents a high-value segment of the aviation industry with growing operational, economic, and environmental importance. However, traffic patterns within this segment are less transparent than those of commercial aviation.

This project aims to:

- Identify Luxury Private Aviation (LPA) flights using operational flight-level data
- Analyse temporal and geographical traffic patterns
- Forecast future non-domestic LPA arrivals to France
- Compare baseline statistical forecasting with machine learning approaches
- Generate business and operational insights from aviation traffic trends

The forecasting component can support decision-making related to:

- airport operations
- resource allocation
- premium aviation services
- infrastructure planning
- traffic trend monitoring

---

## Main Objectives

The project focuses on the following analytical tasks:

- Data collection and integration
- Flight filtering and LPA identification
- Data cleaning and preprocessing
- Exploratory data analysis (EDA)
- Time-series aggregation and trend analysis
- Forecasting model development
- Model evaluation and comparison
- Business interpretation of results

---

## Dataset and Data Sources

The project primarily uses Eurocontrol operational flight-level data.

Main sources include:

- Eurocontrol OPDI monthly flight data
- aircraft reference databases
- airport reference datasets

The repository does not contain large raw datasets due to file size and reproducibility considerations.

---

## Repository Structure

```text
luxury-private-aviation-forecasting/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── luxury_private_aviation_forecasting.ipynb
├── data/
│   └── README.md
└── outputs/
    ├── figures/
    └── tables/
```

## Folder Description

| Folder/File | Description |
|---|---|
| `README.md` | Main project documentation |
| `requirements.txt` | Python dependencies required to run the project |
| `.gitignore` | Excludes unnecessary large/generated files from Git tracking |
| `notebooks/` | Main Jupyter notebook containing the full workflow |
| `data/` | Data-related notes and optional local datasets |
| `outputs/figures/` | Exported visualizations and plots |
| `outputs/tables/` | Generated tables and analytical outputs |

---

## Methodology

The project follows a structured end-to-end analytics workflow:

1. Data acquisition and loading
2. Data preprocessing and cleaning
3. Flight filtering and classification
4. Feature engineering
5. Exploratory data analysis (EDA)
6. Time-series preparation
7. Forecasting model implementation
8. Model validation and evaluation
9. Business interpretation and conclusions

---

## Forecasting Approaches

The project compares multiple forecasting techniques, including:

- baseline forecasting methods
- statistical time-series forecasting
- Prophet forecasting
- LightGBM-based machine learning forecasting
- hybrid forecasting approaches

Model performance is evaluated using appropriate forecasting metrics and out-of-sample validation techniques.

---

## Technologies Used

Main technologies and libraries used in the project include:

- Python
- Jupyter Notebook
- Pandas
- NumPy
- Matplotlib
- Plotly
- Scikit-learn
- Statsmodels
- Prophet
- LightGBM
- SHAP

---

## Reproducibility

To reproduce the project:

### 1. Clone the repository

```bash
git clone <repository-url>
```

### 2. Create a Python environment

Example using venv:

```bash
python -m venv .venv
```

Activate environment:

Windows:

```bash
.venv\Scripts\activate
```

Mac/Linux:

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Open the notebook

Open:

```text
notebooks/luxury_private_aviation_forecasting.ipynb
```

Then run the cells sequentially.

---

## Notes About Data

Large raw datasets are intentionally excluded from the repository through `.gitignore`.

This approach:

- keeps the repository lightweight
- avoids unnecessary storage usage
- improves GitHub usability

If external reference files are required, they should be placed locally inside the `data/` directory before execution.

---

## Outputs

The project generates:

- exploratory visualizations
- trend analysis charts
- forecasting outputs
- evaluation metrics
- business insights related to luxury private aviation traffic

Generated outputs can be stored inside:

```text
outputs/figures/
outputs/tables/
```

---

## Academic Context

This repository supports the MSc IT for Business Data Analytics capstone project submission at International Business School (IBS), Budapest.

The repository is intended for:

- academic assessment
- reproducibility
- documentation
- demonstration of analytical methodology

---

## AI Usage Disclosure

AI-assisted tools were used during parts of the coding and workflow refinement process in accordance with the Business Data Analytics Project Handbook guidelines regarding AI-supported coding assistance.

All generated code, outputs, interpretations, and analytical decisions were reviewed, modified, validated, and integrated by the author.

---

## Author

ABDUBALIEVA, Akmaral
MSc IT for Business Data Analytics  
International Business School (IBS), Budapest
