# Forecasting Notebook

This repository contains a Jupyter Notebook that demonstrates time series forecasting using multiple approaches, including Random Forest, LSTM with attention, and Transformer-based architectures. The notebook covers data preparation, feature engineering, model training, iterative forecasting, inversion of scaled predictions, and evaluation & visualization of both continuous and categorical forecasts.

## Table of Contents

- [Overview](#overview)
- [Data Preparation](#data-preparation)
- [Feature Engineering](#feature-engineering)
- [Models](#models)
  - [Random Forest Forecasting](#random-forest-forecasting)
  - [LSTM Forecasting with Attention](#lstm-forecasting-with-attention)
  - [Transformer Forecasting with Iterative Approach](#transformer-forecasting-with-iterative-approach)
- [Evaluation and Visualization](#evaluation-and-visualization)


## Overview

This notebook demonstrates how to forecast daily time series data using multiple models. It includes:
- Data scaling with different scalers for various feature types.
- Feature engineering by creating lag features, time-based features, and encoding categorical variables.
- Building sequences for deep learning models.
- Training forecasting models such as Random Forest, LSTM (with attention), and Transformer.
- Implementing iterative forecasting by updating the input window with previous predictions.
- Inverting (unscaling) predictions to their original scale.
- Evaluating forecasts with metrics such as RMSE, MAE, and MAPE.
- Visualizing both continuous and categorical forecast performance.

## Data Preparation

The raw data (`daily_df`) includes columns such as:
- **Date** (not scaled)
- **Revenue**, **Units Sold**, **Unit Price**
- Categorical features (e.g., Product Category, Product Name, Region, Payment Method)
- Engineered lag features (e.g., `Revenue_lag_1`, `UnitsSold_lag_1`, etc.)

Different scaling methods are applied:
- **RobustScaler** is used for features like **Revenue** and **Unit Price**.
- **MinMaxScaler** (or alternatives) is used for **Units Sold**.

## Feature Engineering

Lag features are generated over a defined window (e.g., 60 days) in an interleaved manner:
[Revenue_lag_1, UnitsSold_lag_1, Revenue_lag_2, UnitsSold_lag_2, …]
Exogenous features include time components (e.g., Month, DayOfWeek, IsWeekend), the current unit price, and one-hot encoded categorical features. These are concatenated to form the complete feature vector for model training.

## Models

### Random Forest Forecasting

- **Training:**  
  A Random Forest model is trained on a feature vector that combines interleaved lag features and exogenous features.
- **Iterative Forecasting:**  
  The forecasting procedure starts with the last training window. Predictions are made day-by-day, and the forecasted continuous values update the lag vector for subsequent predictions.
- **Feature Importance:**  
  Feature importances are extracted (excluding lag features) and visualized.

### LSTM Forecasting with Attention

- **Architecture:**  
  A multi-layer LSTM model (with dropout and dense layers) is built, with an added attention mechanism to focus on the most relevant parts of the sequence.
- **Iterative Forecasting:**  
  The LSTM model is applied in an iterative loop where the window is updated with predicted values.

### Transformer Forecasting with Iterative Approach

- **Architecture:**  
  A Transformer model is implemented with positional encoding, multi-head attention, and feed-forward blocks.
- **Iterative Forecasting:**  
  The model predicts one day at a time, and the output is used to update the input sequence for subsequent forecasts.

## Evaluation and Visualization

Evaluation metrics are computed on the **unscaled** (inverted) predictions:
- **Continuous Metrics:**  
  RMSE, MAE, and MAPE are computed on the inverted values for Revenue and Units Sold.
- **Categorical Metrics:**  
  Categorical predictions are decoded using argmax and compared against true values.
- **Visualization:**  
  The notebook provides plots comparing actual vs. predicted values and bar charts for categorical accuracies, as well as a comparison table for the first several test days.

