# Capstone Project – Predicting EB-2 / EB-3 India Visa Bulletin Movement  
UC Berkeley AI/ML Program – Module 20.1 (Initial Report & EDA)

## Overview
The goal of this project is to analyze historical U.S. Visa Bulletin data and develop a baseline machine-learning model to predict **month-to-month movement in cutoff dates** for the **EB-2 and EB-3 India employment-based green card categories**.

Because the Visa Bulletin exhibits irregular behavior—forward jumps, retrogressions, and long periods of stagnation—our first objective in Module 20.1 is to complete:

- Data cleaning and preprocessing  
- Exploratory Data Analysis (EDA)  
- Feature engineering  
- Baseline predictive modeling  
- Evaluation and interpretation  

This work establishes the analytical foundation for more advanced modeling in Module 24 (e.g., XGBoost and LSTM).

---

## Dataset

### **Source**
The dataset used in this project is a **historical compilation of Final Action Dates** for EB-2 and EB-3 India.  
It includes the following columns:

- `Year`  
- `Month`  
- `Bulletin` (e.g., `"1-Oct-20"` indicating Visa Bulletin month)  
- `EB2-FA` – EB-2 India Final Action Date  
- `EB3-FA` – EB-3 India Final Action Date  
- Additional columns such as Date of Filing (DOF) and AOS deltas were present but not used in this baseline model.

### **Data Preparation**
The dataset required the following transformations:

1. **Convert the Billboard-style bulletin date** (`"1-Oct-20"`) into a standard `datetime` format.  
2. **Reshape the dataset** from wide format (`EB2-FA`, `EB3-FA`) to long/tidy format using `pandas.melt()`, producing:
   - `bulletin_date`  
   - `category` (`EB2` or `EB3`)  
   - `cutoff_date`  
3. **Parse cutoff dates** from values such as `"1-Sep-09"` into `datetime`.  
4. **Filter out invalid or missing cutoff values** (e.g., `"C"`, `"U"`).

The final cleaned structure used for modeling:

| bulletin_date | category | cutoff_date | country |
|---------------|----------|-------------|---------|
| 2020-10-01    | EB2      | 2009-09-01  | India   |
| 2020-10-01    | EB3      | 2010-01-15  | India   |
| …             | …        | …           | …       |

---

## Exploratory Data Analysis (EDA)

EDA focused on understanding historical cutoff date trends and movement patterns for EB-2 and EB-3 India.

Key visualizations included:

- **Time-series plots** of cutoff dates over multiple years  
- **Distribution plots** of monthly movement (`movement_days`)  
- **Boxplots** showing outlier patterns  
- **Correlation heatmaps** assessing relationships among engineered movement features  

Observations:

- EB-3 often experiences larger jumps and retrogressions compared to EB-2.  
- Visa Bulletin movements are non-linear and influenced by policy decisions rather than smooth patterns.  
- Lagged movement features reveal autocorrelation effects important for time-series modeling.

---

## Feature Engineering

To prepare the dataset for machine-learning modeling, the following features were created:

### **Movement Features**
- `movement_days` = difference in cutoff date (in days) from previous bulletin  
- `movement_lag1` = previous month's movement  
- `movement_lag2` = movement from two months prior  
- `movement_roll3` = 3-month rolling average of movement  

### **Calendar Features**
- `year`  
- `month`

### **Category Encoding**
- `EB2` = 0  
- `EB3` = 1  

These features were selected to allow simple models to capture short-term temporal dependencies and seasonality effects.

---

## Baseline Model

### **Model Used**
`RandomForestRegressor`

### **Target Variable**
`movement_days` — month-over-month movement in cutoff date.

### **Train/Test Split**
A time-ordered 80/20 split was used to avoid data leakage.

### **Evaluation Metric**
**MAE (Mean Absolute Error)**  
Chosen because:

- It provides an intuitive measure of average error in **days**  
- It is robust to extreme jumps/retrogressions  
- It is more stable on non-stationary visa movement patterns than RMSE  

### **Baseline Model Performance**

- **MAE (days):** 133.74  
- **MAE (approx. months):** 4.46  

### **Interpretation**

The baseline model captures short-term movement patterns but struggles with:

- Policy-driven changes  
- Sudden retrogressions  
- Large forward jumps  

This performance is expected for a simple model given the irregular nature of Visa Bulletin behavior.

---

## Repository Structure
├── data/
│ └── EB2_EB3_India_visa_bulletin.csv # Cleaned dataset
│
├── notebooks/
│ └── capstone_20_1_eda.ipynb # EDA + Baseline model notebook
│
└── README.md # Project summary & documentation


---

## Next Steps (Module 24)

The next phase of the capstone will extend this work by:

- Implementing **XGBoost regression** for tabular time-series features  
- Implementing an **LSTM neural network** for sequential pattern learning  
- Comparing models using MAE and additional metrics  
- Enhancing feature engineering with:
  - visa bulletin regime shifts  
  - backlog indicators  
  - category-specific time series decomposition  

The final model will provide deeper insight into EB-2 and EB-3 India visa movement trends and forecasting behavior.

---

## Author
**Soumyamol Vijayamma Surendran**  
UC Berkeley Artificial Intelligence & Machine Learning Professional Program  
2025
