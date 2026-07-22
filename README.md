# Spatio-Temporal Machine Learning for Global Drought Forecasting

## Overview
This project was developed for a global data science challenge organized by ITU and the UN. The business objective is critical: to forecast the Earth's **Total Water Storage** (how much water is stored above and below ground) one month into the future. 

Early detection of water depletion allows governments and organizations to proactively manage drought crises before they cause severe socio-economic damage. As a Data Scientist, my goal was to process satellite and climate data to build a highly robust, production-ready Machine Learning forecasting pipeline.

## The Challenge & The Data
The dataset contains over 2.4 million geospatial-temporal records. Instead of viewing this simply as a table of numbers, it's important to understand the physical reality it represents:
- **Spatial Data:** Geographic coordinates mapping the entire globe.
- **Water Storage:** The current amount of water in a specific location (the target we want to predict for the future).
- **Climate Indicators:** Indexes measuring historical precipitation and surface soil moisture over various timeframes (e.g., the last month, the last 6 months).

## Methodology: 
When building models for time-series forecasting, especially those involving geographic data, standard techniques often fail due to data leakage or a lack of physical context. Here is the strategic approach that I applied to this project:

### 1. Handling Real-World Missing Data
In production environments, sensor data is frequently missing or delayed. During my Exploratory Data Analysis, I identified significant gaps in the most recent water storage readings.
- **The Solution:** Instead of naive imputations (like replacing missing values with an average), I implemented a **Spatio-Temporal Forward Fill**. By grouping the data by exact geographic coordinates and sorting chronologically, I propagated the last known valid water level forward. This respects the laws of physics: large bodies of water and deep soil moisture deplete gradually over time, not instantly.

### 2. Contextual Feature Engineering
Machine Learning models learn better when domain knowledge is translated into explicit features. Rather than feeding raw data blindly, I engineered features to provide context:
- **Drought Momentum:** By subtracting long-term precipitation trends from short-term precipitation trends, I created a feature that measures "momentum." This tells the model not just how dry an area is, but *whether it is actively getting drier or recovering*.
- **Surface vs. Groundwater Interaction:** By calculating the difference between total water storage and surface soil moisture, the model can better estimate deep groundwater reserves, a critical factor for long-term drought resilience.
- **Seasonality:** Extracted the specific month from the timestamps to allow the model to recognize annual cycles (e.g., monsoon seasons vs. dry seasons).

### 3. Out-of-Time Validation Strategy (Preventing Data Leakage)
A common mistake is using a random split (e.g., 80/20) for time-series data. This causes "data leakage" because the model might learn from the future to predict the past.
- **The Solution:** I implemented an **Out-of-Time Cross-Validation (GroupKFold)**. I grouped the dataset by the temporal dimension (`Year-Month`). In every training fold, an entire month of data is held out completely. This forces the model to prove it can genuinely predict the future, perfectly simulating the real-world deployment scenario.

### 4. Algorithm Selection: LightGBM
For the predictive engine, I utilized a **Gradient Boosting Machine (LightGBM)**.
- **Why LightGBM?** It was specifically chosen because it offers the optimal trade-off between high predictive power and computational efficiency. It trains exponentially faster than deep neural networks while consuming a fraction of the memory, making it the perfect choice for processing tabular datasets with millions of rows Furthermore, tree-based models naturally partition spatial coordinates (Latitude and Longitude) to form an internal "rough map" of the world, identifying region-specific climate behaviors (e.g., the Sahara vs. the Amazon) without requiring complex spatial transformations.

## Results & Impact
This approach achieved an Out-of-Time Validation RMSE of 0.52, while a score of 0.72 on the public leaderboard of the competition. To put this into context, standard baseline models typically score around 0.80, while the competition's top public leaderboard benchmark hovers around 0.65. By outperforming the standard baseline and securing a competitive score close to the top tier, this pipeline proves its value. It achieves these results purely through smart validation and domain-specific feature engineering rather than convoluted ensembles, making it highly reliable, easy to explain, and fully ready for real-world deployment.
