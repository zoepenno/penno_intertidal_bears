# Tides and Terrain Mediate Intertidal Foraging by Black Bears (*Ursus americanus*)

## Overview

This repository contains scripts and data for analyzing black bear (*Ursus americanus*) foraging patterns in intertidal habitat using camera trap data, environmental covariates, and tidal information. The analysis combines generalized linear models (GLMs) and generalized additive models (GAMs) to identify landscape and temporal factors mediating bear use of intertidal zones.

## Project Structure

### Data

**Raw Data** (`data/raw_data/`)
- `images_public.csv` — Camera trap image metadata including species, behavior, timestamp, and station ID
- `stations_public.csv` — Spatial and environmental data for each camera station
- `tide_data.csv` — Hourly tide height measurements

**Processed Data** (`data/processed_data/`)
- `bear_count_motion.rds` — Site-level aggregated data with bear foraging days, camera effort, and environmental covariates (used in scripts 01–04)
- `detections_with_tide.rds` — Detection-level data with joined hourly tide information (used in script 05)
- `independent_detections.csv` — Temporally independent bear detections
- `prepared_data.csv` — Initial prepared dataset
- `glmm_best_model.rds` — Fitted best-performing GLMM model

### Scripts

All scripts are R Markdown files located in `scripts/`:

1. **00_data_preparation.Rmd**
   - Loads three raw data files (images, stations, tide)
   - Cleans image metadata and joins with station information
   - Aggregates data into site-level counts and detection-level datasets
   - Produces `bear_count_motion.rds` and `detections_with_tide.rds`

2. **01_data_exploration.Rmd**
   - Exploratory data analysis of processed data
   - Summarizes detection counts and camera effort across stations
   - Visualizes distributions of environmental covariates (substrate, development, freshwater, forest cover, salmon distance)
   - Checks for collinearity among predictors (VIF, correlation analysis)
   - Documents camera operability by site and time period

3. **02_glm_model_selection.Rmd**
   - Fits competing negative binomial generalized linear mixed models
   - Tests hypotheses about environmental and landscape factors driving bear use
   - Compares candidate models using AIC
   - Summarizes the best-fitting model and visualizes parameter estimates
   - Performs model cross-validation for predictive assessment

4. **03_glm_model_diagnostics.Rmd**
   - Checks GLMM assumptions using simulated residuals (DHARMa package)
   - Evaluates residuals vs. fitted, Q-Q plots, and uniformity
   - Tests for dispersion and zero-inflation
   - Assesses spatial autocorrelation

5. **04_glm_figures.Rmd**
   - Creates publication-quality figures for GLM results
   - Visualizes parameter estimates with confidence intervals
   - Generates predicted response curves for key environmental variables
   - Produces spatial predictions and maps

6. **05_gam_models_and_figures.Rmd**
   - Fits two separate GAM analyses:
     - **Seasonal GAM**: Temporal trends in bear foraging across stations (May–October)
     - **Tide GAM**: Effects of tidal height and season on bear presence/detection rates
   - Produces smooth functions for seasonal and tidal effects
   - Generates associated visualizations and model diagnostics

## Requirements

### R Packages

Key packages used across all scripts:
- `tidyverse` — Data wrangling and visualization
- `lubridate` — Date/time handling
- `glmmTMB` — Generalized linear mixed models
- `mgcv` — Generalized additive models
- `broom.mixed` — Model output tidying
- `DHARMa` — Residual diagnostics
- `AICcmodavg` — Model comparison and averaging
- `gratia` — GAM visualization
- `spdep` — Spatial statistics
- `patchwork` — Figure composition
- `here` — File path management

### Usage

1. Ensure all raw data files are in `data/raw_data/`
2. Run scripts in order (00 → 05) to reproduce the full analysis pipeline
3. Each script can be rendered independently if upstream processed data exists
4. R Markdown files can be knitted to HTML or PDF using the `rmarkdown` package

## Key Variables

### Site-Level (from `bear_count_motion.rds`)
- **Response**: `bear_days` — Count of unique days with bear foraging detections
- **Predictors**: 
  - `substrate_type` — Substrate composition (categorical)
  - `freshwater` — Presence/absence of freshwater (binary)
  - `prop_coastal_coniferous_forest` — Proportion of coniferous forest cover
  - `human_development` — Index of human development
  - `dist_to_salmon_km` — Distance to nearest salmon spawning habitat (km)
  - `total_days` — Camera effort (days operational)

### Detection-Level (from `detections_with_tide.rds`)
- **Response**: Bear presence/behavior
- **Predictors**: 
  - `tide_height_m` — Hourly tide height (meters)
  - `season` — Month or season during deployment
  - Site-level environmental variables

## Reproducibility

All scripts include `set.seed(123)` for reproducibility of stochastic processes. The analysis is designed to be fully reproducible from raw data through all figures and model outputs.

## Contact

**Zoe P. Penno** – University of Victoria – zoeppenno@gmail.com

## Citation

Please cite the forthcoming manuscript (details to be added upon publication).
