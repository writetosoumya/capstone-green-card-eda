# Predicting Priority Date Movement for EB-2 and EB-3 India

## Project Overview

This capstone project explores whether we can use historical U.S. Department of State Visa Bulletin data to **predict monthly movement of employment-based (EB) green card priority dates** for:

- **EB-2 India**
- **EB-3 India**

The goal is to understand patterns in Final Action Date / Dates for Filing movement and to build a **baseline machine learning model** that predicts how far the cutoff date will move (forward, stagnate, or retrogress) month-to-month. This assignment (Module 20.1) focuses on:

- Data cleaning and feature engineering  
- Exploratory Data Analysis (EDA)  
- Building a **baseline regression model** for monthly movement  

A more refined modeling and communication layer will be completed in Module 24.

---

## Data

The primary dataset for this phase is a **publicly available historical dataset for EB-2 and EB-3 India** derived from the U.S. Visa Bulletin. The dataset contains:

- Historical **Visa Bulletin months**
- **Cutoff dates** for EB-2 and EB-3 India
- **Movement size** between consecutive bulletins (in months/days)
- Category information (EB-2 vs EB-3)

Source example: historical EB-2 / EB-3 India movement data published via publicly shared CSV (e.g., Kaggle / community-curated datasets based on U.S. Visa Bulletins).

For this assignment, the dataset file is stored locally as:

- `data/EB2_EB3_DoF_FinalAction_Movement_Historical.csv`

> Note: The raw data originates from the monthly U.S. Department of State Visa Bulletins and has been preprocessed into a tabular CSV format.

---

## Research Question

> **Can we use historical Visa Bulletin movements to predict the next month’s movement (in months) for EB-2 and EB-3 India priority date cutoffs?**

Sub-questions:

1. What are the historical patterns of forward movement, stagnation, and retrogression for EB-2 and EB-3 India?
2. Are there noticeable seasonal or regime-like behaviors (e.g., spurts of movement vs. long stagnation)?
3. How well can a simple baseline ML model predict the **magnitude of monthly movement**?

---

## Methods

In this module, I performed:

### 1. Data Cleaning

- Loaded the CSV into pandas
- Converted date-like columns into `datetime` format
- Removed duplicates
- Identified and handled missing values
- Standardized category labels (EB-2 / EB-3)
- Created a clean time-series index by Visa Bulletin month

### 2. Feature Engineering

Key engineered features include (details in notebook):

- **Movement magnitude** between consecutive months (e.g., in months or days)
- **Lag features**, such as:
  - Previous month’s movement
  - Rolling mean / volatility of movement over the last *k* months
- Calendar/time features:
  - Year
  - Month
  - Fiscal-year mapping (where applicable)
- Category flags:
  - EB-2 vs EB-3 binary / one-hot encoding

### 3. Exploratory Data Analysis (EDA)

I used `pandas`, `matplotlib`, and `seaborn` to:

- Visualize the time series of cutoff dates for EB-2 and EB-3 India
- Plot the distribution of monthly movements
- Compare average movement and volatility across categories
- Investigate outliers and retrogression events
- Check correlations between engineered features and target movement

### 4. Baseline Modeling

For this assignment, I implemented a **baseline regression model**:

- **Model type:** `RandomForestRegressor` (or another simple regressor such as Linear Regression)  
- **Target:** movement in months (numeric) for the next Visa Bulletin  
- **Features:** lagged movements, time features, category, and any engineered features

**Train/Test strategy:**

- Chronological split: earlier years as **train**, later years as **test** to respect time ordering.
- Evaluation on the held-out test set.

**Evaluation metric:**

- **Mean Absolute Error (MAE)** on the test set  
- Rationale:
  - MAE is intuitive and directly interprets as "average absolute error in months of movement."
  - It is robust to occasional large jumps or retrogression events compared to squared-error emphasis.

---

## Results (Initial – EDA + Baseline Model)

> 💡 Replace the placeholder values (XX, YY, etc.) after you run the notebook.

### EDA Highlights

- EB-2 and EB-3 India show **long stagnation periods** followed by **sporadic bursts of forward movement**.
- Movement is often **clustered around certain fiscal years** and policy/regulatory changes.
- EB-3 India tends to show [your observation here; e.g., “slightly more frequent small forward movements compared to EB-2”].
- There are clear **outlier months** with unusually large movement (both forward and retrogressive).

### Baseline Model Performance

Using a baseline regression model on the engineered features:

- **Evaluation metric:** Mean Absolute Error (MAE)  
- **Test MAE:** approximately **XX months** (to be updated)  
- The model captures the overall magnitude of movement reasonably, but:
  - It can struggle with **rare, extreme jumps**.
  - Direction (forward vs. retrogression) is generally captured better than exact movement size.

### Interpretation

- Historical patterns and lag features **do contain predictive signal**, but they are heavily influenced by **policy decisions and visa supply**, which are not fully encoded in the dataset.
- The baseline model provides a **starting point** for forecasting and comparison.
- In Module 24, I plan to:
  - Introduce more advanced models (e.g., **XGBoost and LSTM**)
  - Incorporate additional contextual features
  - Improve evaluation and calibration of predictions

---

## Files in This Repository

- `README.md` – this file, including summary of findings  
- `notebooks/capstone_20_1_eda.ipynb` – main notebook used for EDA and baseline modeling  
- `data/EB2_EB3_DoF_FinalAction_Movement_Historical.csv` – local copy of historical EB-2/EB-3 India movement dataset  

---

## How to Run

1. Clone this repository:
   ```bash
   git clone <your-repo-url>.git
   cd capstone-green-card-eda
