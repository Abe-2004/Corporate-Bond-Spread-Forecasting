# Corporate Bond Spread Forecasting via High-Yield Macro Metrics

## Executive Summary
This project constructs an institutional-grade quantitative forecasting pipeline to predict daily changes in the ICE BofA US High Yield Index Option-Adjusted Spread ($Y$). By leveraging ensemble machine learning methods (Random Forest) mapped against daily macroeconomic policy and cross-asset volatility metrics, the framework extracts non-linear structural risks in credit markets over a 24-hour forecasting horizon.

## Econometric & Data Architecture
To eliminate the risk of **Spurious Regression** and address the non-stationary nature of raw financial time series data, rigorous statistical treatments were applied:
* **Target Variable ($Y$):** ICE BofA US High Yield Spread (Daily). Borderline non-stationary ($p$-value: 0.0719), transformed via **First-Differencing** ($\Delta Y_t = Y_t - Y_{t-1}$) to force mean-reversion ($p$-value: 0.0000).
* **Feature 1 ($X_1$):** CBOE Volatility Index (VIX). Stationary level baseline ($p$-value: 0.0000), lagged by 1 day ($t-1$) to isolate causal predictive signaling.
* **Feature 2 ($X_2$):** Effective Federal Funds Rate. Severely non-stationary ($p$-value: 0.9655), transformed via first-differencing ($\Delta X_2$) and lagged by 1 day ($t-1$) to model immediate monetary policy transmission.

## Infrastructure & Leakage Prevention
* **Chronological Train/Test Split:** Standard randomized cross-validation was strictly avoided to prevent look-ahead bias. The dataset was split sequentially (Past 80% for training, Future 20% for out-of-sample testing), ensuring clean temporal handoffs.
* **Algorithmic Engine:** A Random Forest Regressor (100 estimators) was trained to map non-linear regimes and cascade effects during periods of severe volatility contraction/expansion.

## Performance & Theoretical Insights
* **Mean Absolute Error (MAE):** Out-of-sample predictions achieved a tight margin of **0.0450 percentage points** (4.5 basis points).
* **Feature Importance:** Permutation metrics revealed **VIX Levels ($64.35\%$)** dominates daily forecasting power over the lagging Federal Funds changes ($0.33\%$), matching the economic theory of structural transmission delays in monetary policy.
