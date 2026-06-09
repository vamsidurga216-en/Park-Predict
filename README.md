# Smart Parking: Dynamic Pricing System with ML

## Overview

This project implements a **dynamic pricing system for urban parking lots** using historical and real-time demand signals. Built in three progressive stages — from a simple rule-based baseline to a fully trained ML pipeline with explainability — the system predicts optimal parking prices based on occupancy, traffic, vehicle type, queue length, and time-of-day patterns.

---

## Tech Stack

- **Python 3**
- **Google Colab / VS Code** – Notebook-based development
- **Pandas** – Data preprocessing and feature engineering
- **Scikit-learn** – Linear Regression, Random Forest, train/test split, evaluation metrics
- **XGBoost** – Gradient boosted model for price prediction
- **SHAP** – Model explainability and feature importance
- **Bokeh** – Interactive time-series and scatter visualizations
- **Pathway** – Stream simulation and real-time pipeline execution (Models 1 & 2)
- **Matplotlib** – SHAP plots and bar charts

---

## Project Workflow

### Data Preprocessing

- Cleaned and merged date/time into a unified `Timestamp` field
- Mapped categorical variables (`VehicleType`, `TrafficCondition`) to numeric weights
- Engineered `OccupancyRatio`, `DemandScore`, `Hour`, and `IsWeekend` as model features

---

## Pricing Models

### Model 1: Occupancy-Based Linear Pricing (`model1_FINAL.ipynb`)

A baseline model that computes price directly from occupancy ratio using a simple linear formula, executed through a Pathway streaming pipeline.

```python
Price = 10 + 2.0 * (Occupancy / Capacity)
# Bounded between $5 and $20
```

**Model 1 Output:**

![Model 1 Prediction](https://github.com/ar-yansingh/images/blob/main/Dynamic%20parking%20price%20over%20time(model1).png)

---

### Model 2: Context-Aware Demand Score Pricing (`Final_Model2.ipynb`)

Incorporates five demand signals — occupancy ratio, queue length, traffic condition, special day flag, and vehicle type — into a weighted demand score that drives pricing.

```python
DemandScore = α*OccRatio + β*(QueueLength/10) + δ*IsSpecialDay + ε*VehicleWeight - γ*TrafficNum
Price = BASE_PRICE * (1 + λ * NormDemand)
# Bounded between $5 and $20
```

**Model 2 Output:**

![Model 2 Prediction](visuals/model2_price.png)

---

### Model 3: ML-Based Price Prediction (`Model_3.ipynb`)

Uses Model 2's computed prices as training labels and trains three ML models to learn the pricing pattern from 8 engineered features. Adds two new time-based features on top of Model 2.

**Features:** `OccRatio`, `QueueLength`, `TrafficNum`, `VehicleWeight`, `IsSpecialDay`, `DemandScore`, `Hour`, `IsWeekend`

**Model Comparison Results:**

| Model | RMSE | MAE | R² |
|---|---|---|---|
| Linear Regression | 0.5942 | 0.2400 | 0.9267 |
| Random Forest | 0.0041 | 0.0005 | 1.0000 |
| XGBoost | 0.0133 | 0.0089 | 1.0000 |

**Best model: Random Forest (R² = 1.00, RMSE = 0.004)**

**SHAP Feature Importance:**

![SHAP Summary](visuals/shap_summary.png)

![SHAP Bar Chart](visuals/shap_bar.png)

**Top pricing drivers (SHAP):** DemandScore → VehicleWeight → OccRatio

---

## Lot-wise Pricing Visualization

**Price Comparison Across Lots (Model 2):**

![Historical Lot Prices](visuals/price_comparison.png)

---

## Pathway Integration (Models 1 & 2)

- **Used for**: Defining streaming schema, loading data into Pathway tables, applying transformations using `pw.apply`, and executing the full pipeline via `pw.run()`
- **Benefit**: Enables a stream-processing mindset — the same pipeline logic can extend to live sensor data with minimal changes

---

## Project Structure

```bash
.
├── model1_FINAL.ipynb          # Baseline occupancy-based pricing (Pathway)
├── Final_Model2.ipynb          # Context-aware demand score pricing (Pathway)
├── Model_3.ipynb               # ML models + SHAP explainability
├── dataset.csv                 # Raw parking dataset
├── visuals/
│   ├── actual_vs_predicted.png
│   ├── shap_summary.png
│   └── shap_bar.png
└── README.md
```

---

## System Extensibility

- **Real-Time Sensor Integration**: Replace CSV input with live parking sensor APIs
- **Streamlit Dashboard**: Deploy pricing predictions as an interactive web app
- **LSTM/ARIMA Forecasting**: Add time-series demand forecasting on top of the ML layer

## Future Enhancements

- Real-time pricing using live sensor APIs
- Competitive pricing model comparing nearby lots
- Integration into a mobile or dashboard app

---

## Author

- Ajith S — IIT Madras
- GitHub: [srajit1204-del](https://github.com/srajit1204-del)