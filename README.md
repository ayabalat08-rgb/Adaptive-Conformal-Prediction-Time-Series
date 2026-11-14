# Adaptive Conformal Inference (ACI) for Solar PV Power Forecasting

End-to-end online conformal prediction pipeline for utility-scale solar photovoltaic generation (Plant 2), combining machine learning point forecasts, online adaptive calibration (ACI), physical power boundaries, and extreme-event stress testing.

This repository demonstrates that standard split-conformal intervals degrade severely during extreme weather ramps (e.g., dust/cloud transients), and implements adaptive uncertainty quantification to maintain valid coverage.

## 🚀 Key Features & Methodology
1. **Feature Engineering:** Extracts cyclic temporal features (sine/cosine hour transformations) and historical lags (1-step, 2-step, daily seasonal lags, and rolling averages).
2. **Base Forecasting:** Employs a robust `HistGradientBoostingRegressor` trained on historical SCADA and weather data, clipped to physical plant capacity ($P_{rated}$).
3. **Online Adaptive Conformal Inference (ACI):** Implements the Gibbs & Candès (2021) update rule to dynamically adjust the target miscoverage level ($\alpha_t$) in response to recent errors, balancing adaptation speed and calm-period stability.
4. **Physical Boundary Clipping:** Intersects statistical prediction intervals with hard physical limits ($0 \le P \le P_{99.9\%}$) to eliminate physically implausible bounds and tighten interval width.
5. **Extreme-Event Stress Testing:** Evaluates coverage conditional on extreme weather events defined independently via irradiance sensor ramps ($\Delta \text{Irradiation}$), exposing the gap between overall and tail coverage.

## 📊 Evaluation Metrics
The pipeline tracks and compares **ACI + Physical Bounds** against a standard **Split Conformal Baseline** across:
* Overall empirical coverage vs. target level ($1 - \alpha = 90\%$).
* Conditional coverage during top 5% extreme irradiance ramps.
* Mean interval width (evaluating sharpness with and without physical clipping).

## 🛠️ Tech Stack
* **Python** (NumPy, Pandas, Scikit-learn, Matplotlib)
* **Data Sources:** Plant 2 Generation SCADA Data & Weather Sensor Data (Irradiation logs)

## 🗺️ Project Roadmap & Ph.D. Research Integration
* [x] **Phase 1:** Baseline ML model, feature engineering, and chronological data splitting.
* [x] **Phase 2:** Online ACI implementation, step-size optimization ($\gamma$), and physical boundary clipping.
* [x] **Phase 3:** Extreme-event evaluation framework using weather sensor irradiance ramps.
* [ ] **Phase 4 (Next Step): Extreme Value Theory (EVT) Tail Calibration** 
  * Integrating Peaks-Over-Threshold (POT) and Generalized Pareto Distribution (GPD) to model conformity score tails.
  * Replacing empirical quantiles with EVT-calibrated quantiles to improve event-conditional coverage without losing sharpness.
* [ ] **Phase 5:** Multi-climate cross-validation (arid vs. humid datasets) and open-source Python package release.
