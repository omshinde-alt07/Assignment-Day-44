# Predictive Maintenance using Sensor Data

Github Link : https://github.com/omshinde-alt07/Assignment-Day-44

## Project Overview

This project builds an end-to-end predictive maintenance system using industrial sensor data. The objective is to clean raw sensor readings, train a model to predict equipment failure risk in the next 24 hours, and optimize alert thresholds based on real business cost.

The final system is designed for large-scale deployment across 100,000 sensors.

---

## Project Steps

### 1. Data Cleaning

The raw dataset contained multiple quality issues that could break sequence models.

#### Issues Found

* Extra index column (`Unnamed: 0`)
* Fully empty sensor column (`sensor_15`)
* Missing values across multiple sensors
* Timestamp format needed conversion

#### Fixes Applied

* Removed useless columns
* Converted timestamp to datetime
* Sorted by time
* Filled missing values using linear interpolation + forward/backward fill

Output file:

* `sensor_clean.csv`

---

### 2. Failure Prediction Model

A binary classification model was built to predict whether the machine will fail in the next 24 hours.

#### Target Engineering

Created:

* `failure_next_24h = 1` if any failure occurs in next 24 rows
* `0` otherwise

Used `machine_status` to generate labels.

#### Model Used

* XGBoost Classifier

Why XGBoost:

* Strong performance on tabular data
* Handles non-linear patterns
* Fast training
* Supports feature importance

---

## Evaluation Strategy

Accuracy was not used because failure events are rare.

### Main Metric: Recall

Recall is important because missing a true failure causes expensive emergency repair.

Also evaluated:

* F1 Score
* Confusion Matrix
* Classification Report

---

## Business Cost Optimization

Each model alert triggers a maintenance inspection.
Each missed failure triggers emergency repair.

### Example Costs

* False Positive (inspection): ₹500
* False Negative (missed failure): ₹20,000

### Cost Formula

Total Cost = (FP × 500) + (FN × 20000)

Different probability thresholds were tested to find the minimum daily business cost.

### Key Insight

The threshold with lowest cost was not always the same as the threshold with highest F1 score.

This shows that business KPIs should guide production decisions, not only ML metrics.

---

## Files

* `sensor_data.csv` → raw dataset
* `sensor_clean.csv` → cleaned dataset
* `main.ipynb` → notebook with full workflow
* `README.md` → project documentation

---

## How to Run

1. Install dependencies:

   * pandas
   * numpy
   * scikit-learn
   * xgboost

2. Open Jupyter Notebook:

```bash
jupyter notebook main.ipynb
```

3. Run all cells in order.

---

## Final Conclusion

This project shows how machine learning can reduce downtime and maintenance cost by predicting failures early. It also highlights that the best production threshold should be based on financial impact, not only F1 score.
