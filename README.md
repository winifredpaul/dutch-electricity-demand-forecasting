# Dutch Electricity Demand Forecasting to 2030

## Project overview

This project explores historical electricity demand in the Netherlands and uses time-series forecasting methods to estimate how average demand could develop towards 2030. It also examines electricity production, renewable generation, and data-centre electricity consumption, and includes an interactive scenario dashboard.

The aim is to compare forecasting approaches, understand their performance on unseen data, and explore a range of possible future outcomes. The 2030 figures are **exploratory estimates**, not official forecasts or guaranteed outcomes.

## Project contents

```text
.
├── Dutch_Electricity_Demand_Forecasting_2030_Final.pdf
├── Power BI/
│   └── Dashboard_Final.pbix
├── data/
│   ├── raw/
│   │   ├── 82610ENG_SelectieMetStatSymbol_20261019150220.csv
│   │   ├── 84575ENG_SelectieMetStatSymbol_20261013102003.csv
│   │   ├── 84672ENG_SelectieMetStatSymbol_20261019150733.csv
│   │   ├── MHLV_data-2015-2019.xlsx
│   │   ├── Monthly-hourly-load-values_2006-2015.xlsx
│   │   ├── datacenters_elektriciteitsleveringen_2017_2024.xlsx
│   │   ├── monthly_hourly_load_values_2019.csv
│   │   ├── monthly_hourly_load_values_2020.csv
│   │   ├── monthly_hourly_load_values_2021.csv
│   │   ├── monthly_hourly_load_values_2022.csv
│   │   ├── monthly_hourly_load_values_2023.csv
│   │   ├── monthly_hourly_load_values_2024.csv
│   │   ├── monthly_hourly_load_values_2025.csv
│   │   └── nrg_cb_e__custom_23127275_spreadsheet.xlsx
│   └── processed/
│       └── entsoe_nl_hourly_2006_2025.csv
└── notebooks/
    ├── 01_data_exploration_cleaning_preparation_revised.ipynb
    └── 02_electricity_demand_forecasting_final.ipynb
```

## Data

The analysis brings together several types of data:

- **Hourly electricity system load:** ENTSO-E Netherlands data covering 2006–2025.
- **Electricity production and energy balance:** CBS and Eurostat datasets used to explore electricity production, consumption, and the electricity mix.
- **Data-centre electricity consumption:** CBS data on electricity supplied to data centres from 2017–2024.
- **Renewable generation:** data used to examine the development of solar and wind generation.
- **Annual comparison data:** Eurostat annual electricity statistics used as an independent comparison reference.

The raw input files are stored in `data/raw/`. The prepared hourly demand dataset is stored in `data/processed/`.

Some of the most recent official statistics may be provisional. Please consult the source publications and metadata when reusing the data. The repository includes the downloaded data files; source URLs, licensing terms, and detailed dataset metadata should be added here if they are available from the original downloads.

## Methodology

### 1. Data preparation

The preparation notebook checks and cleans the source data before modelling. The work includes:

- Reviewing missing and unusual observations.
- Handling ten missing 02:00 readings associated with spring daylight-saving-time changes between 2006 and 2015, rather than treating them as ordinary missing observations.
- Replacing two anomalous zero-demand readings in 2006 using the median demand for the same hour over a surrounding seven-day period.
- Creating calendar and time-based features.
- Creating lag features for the previous hour, previous day, and previous week.
- Creating rolling averages using past observations only, to avoid using future information.

### 2. Forecasting and evaluation

The forecasting notebook compares the following approaches:

- Weekly-naive baseline.
- Linear Regression.
- Random Forest.
- XGBoost.

The models are trained on historical data from **2006–2024**, with **2025 held out for testing**. Model settings are tuned using time-series cross-validation, including `TimeSeriesSplit` and `RandomizedSearchCV`.

Performance is evaluated using:

- **MAE (Mean Absolute Error):** the average absolute difference between predictions and actual demand.
- **RMSE (Root Mean Squared Error):** an error measure that gives greater weight to larger errors.

### 3. Exploring 2030

The project uses more than one approach to explore average electricity demand in 2030. These estimates are compared rather than treated as a single definitive prediction. Additional exploratory projections consider data-centre electricity consumption and renewable generation.

The Power BI report also includes a scenario page that allows users to compare a baseline estimate with illustrative demand scenarios 5% lower and 5% higher.

## Selected results

On the 2025 test set, the tuned XGBoost model achieved the lowest error among the models tested:

| Model | MAE |
|---|---:|
| Weekly-naive baseline | approximately 540 MW |
| Linear Regression | approximately 308 MW |
| Tuned XGBoost | approximately 147 MW |

The tuned XGBoost model's RMSE was approximately **199 MW**.

The exploratory 2030 average-demand estimates include:

| Approach / reference | Average demand |
|---|---:|
| Recursive hourly XGBoost estimate | approximately 11,876 MW |
| 2021–2025 average annual demand reference | approximately 12,502 MW |
| Direct annual Linear Regression estimate | approximately 12,764 MW |

These figures are model-based exploratory estimates. The XGBoost approach showed an underestimation tendency in the most recent historical years, so the 2030 outputs should be interpreted with care. The different approaches produce a range of possible outcomes rather than one certain future value.

## Power BI dashboard

Open `Power BI/Dashboard_Final.pbix` in **Microsoft Power BI Desktop**.

The report contains pages covering:

1. **What Powers the Netherlands** — electricity production and consumption, the electricity mix, and renewable generation.
2. **Electricity Demand Forecasting (2025)** — comparison of model predictions and actual demand.
3. **Looking Ahead to 2030** — exploratory estimates for demand, data centres, and renewable generation.
4. **What If? Exploring Dutch Electricity Demand in 2030** — interactive lower-demand, baseline, and higher-demand scenarios.

The final presentation is available as `Dutch_Electricity_Demand_Forecasting_2030_Final.pdf`.

## How to use the notebooks

1. Clone or download this repository.
2. Open the project in Jupyter Notebook or JupyterLab.
3. Start with `notebooks/01_data_exploration_cleaning_preparation_revised.ipynb`.
4. Continue with `notebooks/02_electricity_demand_forecasting_final.ipynb`.

The notebooks use relative paths to access the project data. For the paths to work as written, launch Jupyter from the project root or adjust the working directory as needed.

Install the Python libraries imported by the notebooks in your environment before running them. The exact environment and package versions have not been pinned in this repository, so results may vary slightly across software versions.

## Limitations and interpretation

- The 2030 results are exploratory model outputs, not official forecasts.
- Electricity demand can be affected by factors not fully represented in the historical features, including weather, prices, economic activity, electrification, policy, and changes in generation and storage.
- The 2022 demand decline illustrates that historical trends can be disrupted by unusual conditions.
- The data-centre and renewable-generation projections depend on assumptions and historical trends; they should not be interpreted as guaranteed outcomes.
- Recent statistical observations may be provisional.
- Model performance on the 2025 test period does not guarantee the same accuracy in future years.

## Suggested next steps

Potential extensions include adding weather, electricity prices, economic indicators, and more detailed renewable-generation data; regularly retraining and evaluating models as new observations become available; and testing a wider range of demand scenarios to support grid-planning discussions.

## Author

**Winifred Paul**

## Disclaimer

This repository was created as a data analytics project. The analysis is intended for educational and exploratory purposes and should not be used as the sole basis for operational, financial, or infrastructure investment decisions.
