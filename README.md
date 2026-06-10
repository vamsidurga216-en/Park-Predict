# Smart Parking: Dynamic Pricing System

## Overview

This project implements a **dynamic pricing system for urban parking lots** using historical and real-time demand signals. Built in two progressive stages — from a simple rule-based baseline to a context-aware demand scoring model — the system computes optimal parking prices based on occupancy, traffic, vehicle type, queue length, and time-of-day patterns.

---

## Tech Stack

- **Python 3**
- **Google Colab ** – Notebook-based development
- **Pandas** – Data preprocessing and feature engineering
- **Pathway** – Stream simulation and real-time pipeline execution
- **Bokeh** – Interactive time-series and scatter visualizations
- **Matplotlib** – Bar charts and pricing plots

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

## Lot-wise Pricing Visualization

**Price Comparison Across Lots (Model 2):**

![Historical Lot Prices](visuals/price_comparison.png)

---

## Pathway Integration

- **Used for**: Defining streaming schema, loading data into Pathway tables, applying transformations using `pw.apply`, and executing the full pipeline via `pw.run()`
- **Benefit**: Enables a stream-processing mindset — the same pipeline logic can extend to live sensor data with minimal changes

---

## Project Structure

```bash
.
├── model1_FINAL.ipynb          # Baseline occupancy-based pricing (Pathway)
├── Final_Model2.ipynb          # Context-aware demand score pricing (Pathway)
├── dataset.csv                 # Raw parking dataset
├── visuals/
│   ├── model2_price.png
│   └── price_comparison.png
└── README.md
```

---

## System Extensibility

- **Real-Time Sensor Integration**: Replace CSV input with live parking sensor APIs
- **Streamlit Dashboard**: Deploy pricing predictions as an interactive web app

## Future Enhancements

- Real-time pricing using live sensor APIs
- Competitive pricing model comparing nearby lots
- Integration into a mobile or dashboard app

# Smart Parking: Dynamic Pricing System

**Author:** Vamsi D
