<div align="center">

<img src="banner.jpg" alt="Spatio-Temporal Machine Learning Banner" width="100%"/>

# Spatio-Temporal Machine Learning  
## Global Drought Forecasting

**A Step Ahead of Drought: Forecasting Global Water Storage Challenge**  
*Zindi Competition hosted in partnership with ITU and the United Nations*

<br>

<img src="https://img.shields.io/badge/Python-111111?style=for-the-badge&logo=python&logoColor=white"/>
<img src="https://img.shields.io/badge/LightGBM-111111?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Geospatial%20ML-111111?style=for-the-badge&logo=googleearth&logoColor=white"/>
<img src="https://img.shields.io/badge/Time%20Series-111111?style=for-the-badge"/>

<br><br>

<a href="https://zindi.africa/competitions/one-step-ahead-of-drought-forecasting-global-water-storage-challenge">
<b>View Competition on Zindi ↗</b>
</a>

</div>

---

# Project Overview

The objective of this project was to forecast **Global Total Water Storage (TWS)** one month ahead using satellite-derived geospatial and climate data.

Accurate drought forecasting enables governments and organizations to detect early water depletion patterns and implement preventive strategies before severe socio-economic impacts occur.

---

# Dataset

The dataset contains more than **2.4 million spatio-temporal observations** covering global locations.

| Data Component | Description |
|---|---|
| Spatial Information | Latitude and longitude coordinates representing global locations |
| Total Water Storage | Target variable representing surface and groundwater availability |
| Climate Indicators | Historical precipitation and soil moisture indexes across different time windows |
| Temporal Features | Monthly observations capturing seasonal and long-term climate patterns |

---

# Machine Learning Pipeline

The main challenge was combining **geospatial information** with **time-dependent forecasting** while preventing future information leakage.

```mermaid
flowchart LR

A[Satellite & Climate Data<br>2.4M+ Records]
--> B[Spatio-Temporal Missing Data Handling]

B --> C[Domain-Based Feature Engineering]

C --> D[Out-of-Time Validation<br>Grouped by Year-Month]

D --> E[LightGBM Model]

E --> F[Global Water Storage Forecast]

style E fill:#2563EB,color:#ffffff
```

---

# Methodology

## Spatio-Temporal Missing Data Handling

Real-world satellite datasets frequently contain missing observations due to sensor limitations and acquisition gaps.

To address this challenge, I implemented a **spatio-temporal forward fill strategy**:

- Data was grouped by exact geographic coordinates.
- Observations were sorted chronologically.
- Missing values were replaced using the latest available measurement.

This approach reflects the physical behavior of water systems, where storage variations generally occur progressively rather than through abrupt random changes.

---

## Domain-Specific Feature Engineering

Additional features were engineered to capture drought dynamics and improve the model's understanding of environmental processes.

### Drought Momentum

Measures whether a region is becoming progressively drier by comparing short-term and long-term precipitation trends:

```text
Short-term precipitation trend - Long-term precipitation trend
```

### Surface Water vs Groundwater Interaction

Estimated deeper water storage variations by comparing:

```text
Total Water Storage - Surface Soil Moisture
```

### Seasonal Patterns

Extracted temporal characteristics:

- Month
- Seasonal cycles
- Regional climate behavior

---

# Validation Strategy

## Preventing Temporal Leakage

A traditional random train/test split is unsuitable for forecasting tasks because it allows the model to learn information from future observations.

To replicate a real deployment scenario, I implemented **Out-of-Time Cross Validation**:

- Data grouped by `Year-Month`
- Entire months held out during validation
- Model evaluated on unseen future periods

This ensures that evaluation reflects the model's ability to forecast future water storage conditions.

---

# Model Selection

## LightGBM Gradient Boosting

LightGBM was selected due to its strong balance between:

- Predictive performance
- Training efficiency
- Memory efficiency
- Ability to capture nonlinear relationships

Tree-based models can naturally learn interactions between:

- Geographic coordinates
- Climate variables
- Seasonal patterns
- Regional drought behaviors

without requiring complex spatial transformations.

---

# Results

| Evaluation | RMSE | Description |
|---|---:|---|
| Out-of-Time Validation | **0.52** | Realistic simulation of future forecasting performance |
| Public Leaderboard | **0.72** | Competitive performance on the Zindi benchmark |

The final solution achieved competitive results by combining:

- Robust temporal validation
- Domain-driven feature engineering
- Efficient gradient boosting modeling

---

# Technology Stack

| Category | Tools |
|---|---|
| Programming Language | Python |
| Machine Learning | LightGBM |
| Data Processing | Pandas, NumPy |
| Geospatial Analysis | GeoPandas, Satellite Data |
| Validation | Time-based Cross Validation |
| Competition Platform | Zindi |
