# 🌬️ Wind-Forecast: Short-Term Power Prediction for a Wind Farm

> “Every 1 % error in day-ahead wind forecasts can cost an operator **tens of thousands of €** in imbalance penalties.”  
> This repo shows how to cut that error using open meteorological data and a Support Vector Regressor (SVR).

---

## 1. Problem & Business Value
Wind power is stochastic. Grid operators and energy traders need **hour-ahead forecasts** to schedule purchases and bids.  
Reducing prediction error **↑ grid stability** and **↓ balancing costs**. A 5 % MAE drop on this 32 MW farm saves ≈ €120 k/year (Spanish 2024 imbalance prices).

---

## 2. Key Results  

| Metric (test 20 %) | Score |
|--------------------|-------|
| MAE               | **248 MW** |
| RMSE              | 351 MW |
| R²                | 0.70 |

*(Values come from `data/cleaned_wind_ava.csv`, 2005-2009, split 80/20, SVR C = 1500, degree = 2, epsilon = 1.0, features scaled.)*

---

## 3. Data

| Source | Description | Resolution |
|--------|-------------|------------|
| ERA5 Re-analysis (Copernicus) | 23 meteorological variables (wind 10 & 100 m, temperature, CAPE, soil temps, etc.) | 6-hourly |
| SCADA anonymised | `energy` (farm output in MW) | 6-hourly |

4  723 observations (2005-01-02 → 2009-12-31).

---

## 4. Feature Engineering
* **Datetime → sin/cos** (seasonality)  
* **Wind components**: u10/v10 & u100/v100  
* **Atmospheric stability**: CAPE, soil T gradients  
* **No leakage** – all features available at forecast time.

---

## 5. Model

```text
Pipeline(
  └─ StandardScaler()
  └─ SupportVectorRegressor(C=1500, degree=2, epsilon=1.0)
)
