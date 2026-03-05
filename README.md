# Walkability & Coronary Heart Disease: A County-Level Analysis

## Overview

This project investigates the relationship between neighborhood walkability and coronary heart disease (CVD) prevalence across 2900+ U.S. counties. Using OLS regression with controls for median household income and age demographics, we decompose how much of the raw walkability–CVD association is driven by confounding versus an independent effect.

## Research Question

**Does walkability predict lower rates of coronary heart disease at the county level, and how much of that association persists after controlling for income and age?**

## Key Findings

- A one-unit increase in the walkability index is associated with a **0.44 percentage point decrease** in CVD prevalence in the raw model (R² = 0.317).
- After controlling for log income and percent population 65+, the walkability coefficient shrinks to **-0.14** but remains significant (R² = 0.821).
- Income accounts for roughly **50%** of the raw association, age explains an additional **34%**, and about **33%** of the original effect appears independent of these confounders.
- The most walkable counties (Falls Church VA, San Francisco CA, Denver CO) have CVD rates around 4–5%, while the least walkable counties reach 10%+.

## Data Sources

| Dataset | Source | Description |
|---------|--------|-------------|
| `Health_Data.csv` | CDC PLACES | County-level coronary heart disease prevalence among adults |
| `Walkability_Data.csv` | EPA Smart Location Database | Census block group walkability index (NatWalkInd), aggregated to county level using population weights |
| `Census_Data.json` | U.S. Census Bureau ACS | Median household income (B19013_001E) and age distribution by sex (B01001 table) for computing % population 65+ |

## Methodology

1. **Data Collection** — Merged three datasets on county FIPS codes. Walkability scores were aggregated from block group to county level using population-weighted averages.

2. **Data Cleaning** — Removed two counties with sentinel income values (-666666666). Final dataset contains 2,943 counties.

3. **Exploratory Analysis** — Scatter plots with fitted lines for all variable pairs, distribution histograms, a three-variable scatter (walkability × income × CVD), state-level choropleth map, and tables of most/least walkable counties.

4. **Regression** — Three nested OLS models to isolate walkability's independent effect:
   - **Model 1**: Walkability only (β = -0.437, R² = 0.317)
   - **Model 2**: + Log Income (β = -0.219, R² = 0.626)
   - **Model 3**: + Log Income + % 65+ (β = -0.145, R² = 0.821)

   Models 2 and 3 use HC1 robust standard errors.

## Repo Structure

```
├── README.md
├── Walkability.ipynb          
├── Health_Data.csv
├── Walkability_Data.csv
├── Census_Data.json
```

## Requirements

- Python 3.8+
- pandas
- numpy
- matplotlib
- statsmodels
- plotly

## Limitations

- **Ecological fallacy** — County-level associations don't necessarily reflect individual-level behavior. A walkable county doesn't mean every resident walks.
- **Cross-sectional design** — This is a snapshot, not a causal analysis. We can't determine whether walkability reduces CVD or whether healthier populations sort into walkable areas.
- **Omitted variables** — Diet, healthcare access, smoking rates, education, and other factors that influence CVD are not controlled for.
- **Walkability measurement** — The EPA index captures built-environment features but doesn't account for climate, safety, or actual walking behavior.
