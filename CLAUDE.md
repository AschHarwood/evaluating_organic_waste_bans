# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains data and analysis code for an academic paper evaluating U.S. state-level organic waste disposal bans. The analysis uses Bayesian hierarchical regression to examine the relationship between policy strength scores and landfilled food waste across 22 U.S. states (1999-2023).

## Tech Stack

- **Language**: R (via Jupyter notebook with R kernel)
- **Key packages**: `brms` (Bayesian regression), `tidyverse`, `ggplot2`, `dplyr`, `here`
- **Models**: Pre-fitted Bayesian models stored as `.rds` files

## Repository Structure

- `final_paper_analysis/analysis_evaluating_waste_bans.ipynb` - Main analysis notebook
- `final_paper_analysis/data.csv` - Dataset (64 observations, 8 variables)
- `final_paper_analysis/models/` - Pre-fitted model files (`.rds`)

## Running the Analysis

The notebook uses the `here` package for path management. Run cells sequentially:
1. Install/load packages
2. Set working directory via `setwd(here("final_paper_analysis"))`
3. Load data from `data.csv`
4. Load pre-fitted models from `models/` directory

Pre-fitted models are provided because Bayesian model fitting is computationally expensive. Use `readRDS()` to load models rather than refitting.

## Data Variables

Key variables in `data.csv`:
- `log_food_tons`: Dependent variable (log of annual tons of food waste to landfills)
- `adjusted_quant_score_linear`: Policy strength score (0-16 scale)
- `sector_name`: Factor with 3 levels (sector_combined, sector_res, sector_ici)
- `state`: Used for random intercepts in hierarchical model

## Model Specification

Two models are provided:
- `strong_prior_model.rds`: Informative prior on policy coefficient `normal(-0.025, 0.006)`
- `uninformative_prior_model.rds`: Default brms priors (robustness check)

Formula: `log_food_tons ~ adjusted_quant_score_linear + log(pce_scaled) + centered_search + inflation_adjusted_tipping_fee + sector_name + (1 | state)`
