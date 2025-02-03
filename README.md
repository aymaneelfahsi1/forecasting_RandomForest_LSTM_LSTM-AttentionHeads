# Forecasting Notebook

This repository contains a Jupyter Notebook that demonstrates iterative time series forecasting using three different models:  
- **Random Forest**,  
- **LSTM**, and  
- **LSTM with Attention**.

Each model is applied in an iterative forecasting setup where the most recent window of data is updated with each new forecast to predict future days. The notebook covers data preparation, feature engineering, model training, inversion (unscaling) of predictions, and evaluation & visualization of both continuous and categorical outputs.

## Table of Contents

- [Overview](#overview)
- [Data Preparation](#data-preparation)
- [Feature Engineering](#feature-engineering)
- [Models](#models)
  - [Random Forest Forecasting](#random-forest-forecasting)
  - [LSTM Forecasting](#lstm-forecasting)
  - [LSTM Forecasting with Attention](#lstm-forecasting-with-attention)
- [Iterative Forecasting Approach](#iterative-forecasting-approach)
- [Evaluation and Visualization](#evaluation-and-visualization)


## Overview

This notebook demonstrates iterative forecasting on daily time series data using three different approaches. For each method, the forecasting process starts with the last window of training data and updates that window day by day with the model's predictions. The models predict multiple outputs (both continuous and categorical targets), and the predictions are later inverted (unscaled) back to the original scale for evaluation.

## Data Preparation

- **Input Data:**  
  The raw data is contained in a DataFrame (`daily_df`) with columns such as Date, Revenue, Units Sold, Unit Price, and various categorical features.
  
- **Scaling:**  
  Different scaling methods are applied based on the feature type. For example, RobustScaler is used for Revenue and Unit Price, while MinMaxScaler (or alternatives) is applied for Units Sold.

## Feature Engineering

- **Lag Features:**  
  Lag features are created over a specified window (e.g., 60 days) in an interleaved manner (e.g., `[Revenue_lag_1, UnitsSold_lag_1, Revenue_lag_2, UnitsSold_lag_2, …]`).
  
- **Exogenous Features:**  
  In addition to lag features, exogenous features such as time components (Month, Day, DayOfWeek, IsWeekend), the current Unit Price, and one-hot encoded categorical features are included.
  
- **Target Columns:**  
  The target columns are defined as continuous targets (Revenue_target, UnitsSold_target) followed by categorical targets (e.g., for Product Category, Product Name, Region, and Payment Method).

## Models

### Random Forest Forecasting

- **Description:**  
  A Random Forest model is trained on the full feature vector (lag features combined with exogenous features).  
- **Iterative Forecasting:**  
  Starting from the last training window, the model forecasts one day at a time. After each forecast, the continuous predictions update the lag vector for the next day.

### LSTM Forecasting

- **Description:**  
  A multi-layer LSTM network is used to forecast the targets.  
- **Iterative Forecasting:**  
  The LSTM model is deployed in an iterative loop where the forecasted continuous outputs are used to update the input window for subsequent predictions.

### LSTM Forecasting with Attention

- **Description:**  
  An LSTM model enhanced with an attention mechanism is implemented to help the model focus on the most relevant parts of the sequence.  
- **Iterative Forecasting:**  
  Like the other models, the LSTM with attention is used in an iterative forecasting loop, updating the window with each new prediction.

## Iterative Forecasting Approach

For all three models, the iterative forecasting procedure works as follows:
1. **Initialization:**  
   The forecast starts with the last window of training data (which includes interleaved lag features).
2. **Iteration:**  
   For each day in the test set:
   - Extract exogenous features from the test day.
   - Construct a full feature vector by concatenating the current lag vector and the exogenous features.
   - Generate a forecast for the next day.
   - Update the lag vector with the newly forecasted continuous targets.
3. **Output:**  
   This process repeats for each day in the test set, yielding a series of iterative predictions.

## Evaluation and Visualization

- **Continuous Evaluation:**  
  The scaled continuous predictions (Revenue and Units Sold) are inverted (unscaled) back to their original scale. Evaluation metrics such as RMSE, MAE, and MAPE are then computed.
  
- **Categorical Evaluation:**  
  Categorical outputs are decoded using argmax, and prediction accuracy is computed.
  
- **Visualization:**  
  The notebook includes plots comparing actual vs. predicted continuous targets and bar charts for categorical prediction accuracies. A comparison table for the first several test days is also provided.

