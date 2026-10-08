# Optimal ML model for Inflation Forecasting

---

## Overview

This repository contains an R Markdown pipeline designed to model, backtest, and forecast core inflation using machine learning and time series techniques. The script compares **Machine Learning (ML)** approaches—specifically **Random Forest** and **LASSO**—against a baseline **ARIMA** time series model and benchmark estimates from the **New Keynsian Phillips Curve (NKPC)**

---

## Key Features & Models

1. **Data Preprocessing & Feature Engineering**
   * Log differences to transform economic variables into quarter-over-quarter (`qoq`) growth rates.
   * Creation of 1-quarter and 4-quarter lagged variables for feature modeling.
   * Time-ordered train/test/forecast splitting.

2. **Model Implementations**
   * **Random Forest (`ranger` via `caret`):** Parameter tuning (`mtry`, `min.node.size`) using Time Series Cross-Validation (TSCV) with a rolling origin (`timeslice`).
   * **LASSO / Elastic Net (`glmnet`):** Grid search across `alpha` (Ridge to LASSO) and `lambda` parameters optimized over multiple forecast horizons (1 and 4) using TSCV.
   * **ARIMA (`forecast::auto.arima`):** Non-ML econometric baseline model.
   * **NKPC Benchmark Comparison:** Evaluates ML predictions against NKPC core inflation estimates.

3. **Evaluation & Visualization**
   * Evaluation metrics: Root Mean Squared Error (RMSE).
   * Conversion of quarterly predictions (`qoq`) into year-over-year (`yoy`) metrics.
   * `ggplot2` visualizations comparing actual vs. predicted values via line plots and bar charts.

4. **Multi-Step Rolling Window Forecasting**
   * Out-of-sample iterative forecasting for **2025Q2 – 2026Q4**.

---

## Required R Packages

```R
library(readxl)     # Read Excel files
library(openxlsx)   # Export Excel files
library(glmnet)     # LASSO / Elastic Net regression
library(tidyverse)  # Data manipulation & tidy workflows
library(caret)      # Machine learning framework & Random Forest
library(ggplot2)    # Visualization
library(forecast)   # ARIMA & time series modeling
library(zoo)        # Time series utility functions
```

---

## Workflow & Project Structure

```text
.
├── 1. Data Cleaning & Transformation  # Log-diff QoQ transformations & lagged variables
├── 2. Random Forest Modeling         # Hyperparameter tuning via TSCV & rolling forecast
├── 3. LASSO Regression               # Alpha/Lambda optimization & coefficient evaluation
├── 4. ARIMA Baseline                 # Time-series baseline fitting & evaluation
├── 5. Backtesting & Model Comparison # QoQ to YoY transformations & RMSE comparison vs NKPC
└── 6. Out-of-Sample Forecasting       # 2025Q2–2026Q4 core inflation projections
```

---

## Guide to Updating & Running the Script

1. **Dataset Import:** Update Excel file paths in Line 21 (`July 2025 Inflation Data.xlsx`)[cite: 1].
2. **Data Slicing:** Adjust dataset slice indices (Lines 69–73) to accommodate newly added quarterly data[cite: 1].
3. **Train & Test Windows:** Update training/testing window parameters for ARIMA (Lines 419–420)[cite: 1].
4. **QoQ to YoY Transformation:** Export predicted results to Excel to calculate YoY transformations and squared differences, or load transformed values back into R (Lines 472–493)[cite: 1].
5. **Out-of-Sample Forecasts:** Update forecast target Excel sources to view predictions up to 2026Q4 (Line 568)[cite: 1].
