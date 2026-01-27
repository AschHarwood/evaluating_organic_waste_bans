# Evaluating the Effectiveness of Organic Waste Disposal Bans

This repository contains the data and analysis code for the paper "Evaluating the Effectiveness of Organic Waste Disposal Bans" (Harwood, 2026). The study evaluates whether U.S. state-level organic waste disposal bans reduce the amount of food waste sent to landfills using Bayesian hierarchical regression on waste characterization study data.

## Overview

The analysis examines the relationship between policy strength scores and landfilled food waste across 22 U.S. states from 1999-2023. The study finds that each one-unit increase in policy strength reduces landfilled food waste by 1-3%.

## Repository Structure

```
evaluating_organic_waste_bans/
├── final_paper_analysis/
│   ├── data.csv                                    
│   ├── analysis_evaluating_waste_bans.ipynb   
│   └── models/
│       ├── uninformative_prior_model.rds  # model (uninformative prior)
│       └── strong_prior_model.rds  # Fitted model (informative prior)
├── LICENSE
└── README.md
```

### File Descriptions

- **`data.csv`**: Contains 64 observations from waste characterization studies across 22 states (1999-2023). Key variables include:
  - `state`: U.S. state name
  - `year`: Year of observation
  - `sector_name`: Waste sector (residential, commercial/industrial/institutional, or combined)
  - `log_food_tons`: Natural log of annual tons of food waste sent to landfills
  - `adjusted_quant_score_linear`: Policy strength score (0-16)
  - `inflation_adjusted_tipping_fee`: Regional landfill tipping fees
  - `pce_scaled`: Log inflation-adjusted personal consumption expenditure for food
  - `centered_search`: Google Trends search volume for "compost" (centered)

- **`analysis_evaluating_waste_bans.ipynb`**: R Jupyter notebook containing:
  - Data loading and preparation
  - Bayesian hierarchical regression models using `brms`
  - Model diagnostics and posterior analysis
  - Results visualization

- **`models/`**: Directory containing fitted Bayesian models saved as `.rds` files:
  - `strong_prior_model.rds`: Model with informative priors (preferred specification)
  - `uninformative_prior_model.rds`: Model with uninformative priors (robustness check)

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
   ```r
   install.packages(c("brms", "tidyverse", "ggplot2", "dplyr", "here"))
   ```

3. Open the notebook:
   - Navigate to `final_paper_analysis/`
   - Open `analysis_evaluating.ipynb` 

### Running the Notebook

The notebook uses relative file paths and will automatically set the working directory to `final_paper_analysis/`. Simply run all cells sequentially:

1. **Load libraries**: Installs and loads required R packages
2. **Set working directory**: Ensures relative paths work correctly
3. **Load data**: Reads `data.csv` from the same directory
4. **Load models**: Loads pre-fitted Bayesian models from `models/`


**Note**: The notebook includes pre-fitted models (`.rds` files).

## Methodology

The analysis employs a Bayesian hierarchical regression model to estimate the relationship between organic waste disposal ban strength and landfilled food waste. Key methodological features:

- **Hierarchical structure**: Random intercepts at the state level account for unobserved heterogeneity across states
- **Policy strength scoring**: Continuous 0-16 scale based on generator coverage, exemptions, and enforcement provisions
- **Controls**: Personal consumption expenditure for food, regional landfill tipping fees, and Google Trends compost search volume
- **Prior specification**: Informative priors derived from state-level evaluations in California, Vermont, and Massachusetts, with a constraint preventing positive policy effects

The dependent variable is the natural log of annual tons of food waste


## Citation

If you use this code or data, please cite:

TBD

## License

See LICENSE file for details.

## Contact

For questions about the analysis or data, please email asch.harwood@refed.com
