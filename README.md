# Evaluating the Effectiveness of Organic Waste Disposal Bans

This repository contains the data and analysis code for the paper "Evaluating the Effectiveness of Organic Waste Disposal Bans" (Harwood, 2026). The study evaluates whether U.S. state-level organic waste disposal bans reduce the amount of food waste sent to landfills using Bayesian hierarchical regression on waste characterization study data.

## Overview

The analysis examines the relationship between policy strength scores and landfilled food waste across 22 U.S. states from 1999-2023. The study finds that each one-unit increase in policy strength reduces landfilled food waste by 1-3%.

## Repository Structure

```
evaluating_organic_waste_bans/
├── final_paper_analysis/
│   ├── food_waste_by_generator.csv     # Sector-level data (64 observations)
│   ├── total_landfill_data.csv         # Aggregated total landfill data (43 observations)
│   ├── analysis_evaluating_waste_bans.ipynb   
│   └── models/
│       ├── strong_prior_model.rds              # Generator-level model (informative prior)
│       ├── uninformative_prior_model.rds       # Generator-level model (uninformative prior)
│       ├── fit_loglog_strong_combined.rds      # Total landfill model (informative prior)
│       └── combined_model_no_prior.rds         # Total landfill model (uninformative prior)
├── LICENSE
└── README.md
```

### Datasets

The analysis uses two datasets, each supporting a different model specification:

- **`food_waste_by_generator.csv`**: Contains 64 observations from waste characterization studies across 22 states (1999-2023), broken out by waste generator sector. Key variables include:
  - `state`: U.S. state name
  - `year`: Year of observation
  - `sector_name`: Waste sector (residential, commercial/industrial/institutional, or combined)
  - `log_food_tons`: Natural log of annual tons of food waste sent to landfills
  - `adjusted_quant_score_linear`: Policy strength score (0-16)
  - `inflation_adjusted_tipping_fee`: Regional landfill tipping fees
  - `pce_scaled`: Log inflation-adjusted personal consumption expenditure for food
  - `centered_search`: Google Trends search volume for "compost" (centered)

- **`total_landfill_data.csv`**: Contains 43 observations of aggregated total landfill food waste (one per state-year). This smaller dataset does not include sector-level breakdowns, so the corresponding models omit the sector control. Key variables include:
  - `state`, `year`, `log_food_tons`, `adjusted_quant_score_linear`, `inflation_adjusted_tipping_fee`, `pce_scaled`
  - `centered_search_roll`: Google Trends search volume for "compost" (centered)
  - `food_tons_clean`: Raw annual tons of food waste 

### Models

- **`analysis_evaluating_waste_bans.ipynb`**: R Jupyter notebook containing:
  - Data loading for both datasets
  - Bayesian hierarchical regression models using `brms`
  - Model summaries and diagnostics

- **`models/`**: Directory containing fitted Bayesian models saved as `.rds` files:
  - **Generator-level models** (use `food_waste_by_generator.csv`, include sector control):
    - `strong_prior_model.rds`: Model with informative priors (preferred specification)
    - `uninformative_prior_model.rds`: Model with uninformative priors (robustness check)
  - **Total landfill models** (use `total_landfill_data.csv`, no sector control):
    - `fit_loglog_strong_combined.rds`: Model with informative priors
    - `combined_model_no_prior.rds`: Model with uninformative priors (robustness check)

## How to Run the Analysis

### Prerequisites

1. **R** 
2. **R packages** 
   - `brms` - Bayesian regression models
   - `tidyverse` - Data manipulation and visualization
   - `ggplot2` - Plotting
   - `dplyr` - Data manipulation
   - `here` - Path management

### Setup

1. Clone this repository:
   ```bash
   git clone <repository-url>
   cd evaluating_organic_waste_bans
   ```

2. Install required R packages:


3. Open the notebook:
   - Navigate to `final_paper_analysis/`
   - Open `analysis_evaluating_waste_bans.ipynb` 

### Running the Notebook

The notebook uses relative file paths and will automatically set the working directory to `final_paper_analysis/`. Simply run all cells sequentially:

1. **Load libraries**: Installs and loads required R packages
2. **Set working directory**: Ensures relative paths work correctly
3. **Load data**: Reads `food_waste_by_generator.csv` for the sector-level analysis and `total_landfill_data.csv` for the aggregated total landfill analysis
4. **Load models**: Loads pre-fitted Bayesian models from `models/` (both generator-level and total landfill specifications)


**Note**: The notebook includes pre-fitted models (`.rds` files).

## Methodology

The analysis employs Bayesian hierarchical regression models to estimate the relationship between organic waste disposal ban strength and landfilled food waste. Key methodological features:

- **Hierarchical structure**: Random intercepts at the state level account for unobserved heterogeneity across states
- **Policy strength scoring**: Continuous 0-16 scale based on generator coverage, exemptions, and enforcement provisions
- **Controls**: Personal consumption expenditure for food, regional landfill tipping fees, and Google Trends compost search volume
- **Prior specification**: Informative priors derived from state-level evaluations in California, Vermont, and Massachusetts, with a constraint preventing positive policy effects

**Two model specifications**:
- **Generator-level**: Uses sector-disaggregated data (`food_waste_by_generator.csv`) and includes sector fixed effects. Dependent variable: natural log of annual tons of food waste.
- **Total landfill**: Uses aggregated state-year data (`total_landfill_data.csv`) with no sector control. Dependent variable: natural log of annual tons of food waste.


## Citation

If you use this code or data, please cite:

TBD

## License

See LICENSE file for details.

## Contact

For questions about the analysis or data, please email asch.harwood@refed.com
