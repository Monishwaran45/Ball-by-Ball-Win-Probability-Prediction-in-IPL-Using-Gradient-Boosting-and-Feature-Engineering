# Ball-by-Ball-Win-Probability-Prediction-in-IPL-Using-Gradient-Boosting-and-Feature-Engineering
# 🏏 IPL Win Probability Model

## 🧠 Objective

Predict the probability of the chasing team winning an IPL match at any ball in the second innings using historical ball-by-ball data.

---

## 📊 Dataset

* Source: IPL ball-by-ball dataset (2008–2025)
* Total samples: ~133,000 (second innings only)
* Each row represents a single delivery

---

## 🎯 Target Variable

```text
win = 1 → batting team wins  
win = 0 → batting team loses
```

---

## ⚙️ Features

### Base Features

* `runs_left`
* `balls_remaining`
* `wickets_left`
* `current_rr`
* `required_rr`

---

### Engineered Features

* `pressure = required_rr - current_rr`
* `runs_per_ball = runs_left / balls_remaining`
* `wicket_pressure = (10 - wickets_left) / 10`
* `balls_per_wicket = balls_remaining / wickets_left`
* `progress = 1 - (balls_remaining / 120)`
* `pressure_ratio = required_rr / current_rr`

These features capture match difficulty, risk, and game progression.

---

## 🤖 Model

* Algorithm: XGBoost Classifier
* Reason: Handles non-linear relationships and feature interactions effectively

---

## 🔧 Training Setup

* Train/Test Split: 80/20
* Hyperparameter tuning: RandomizedSearchCV
* Sample weighting:
  Higher weight assigned to early match states using:

  ```text
  sample_weight = sqrt(balls_remaining)
  ```

---

## 📈 Calibration

* Method: Isotonic Regression
* Purpose: Improve probability reliability
* Result: Predictions align closely with actual outcomes

---

## 📊 Performance Metrics

| Metric   | Value     |
| -------- | --------- |
| Accuracy | ~0.80     |
| Log Loss | ~0.41     |
| ROC AUC  | **0.90+** |

---

## 📉 Model Behavior

* Strong dependency on:

  * `wickets_left`
  * `required_rr`
* Captures match pressure effectively
* Produces realistic probability estimates after calibration

---

## ⚠️ Limitations

* No player-level data (batsman/bowler skill)
* No venue or pitch conditions
* Slight overconfidence in early match phases

---

## 🧠 Key Insights

* Wickets remaining is the most influential factor
* Required run rate dominates match outcome prediction
* Feature engineering contributes more than model choice
* Calibration is essential for meaningful probability outputs

---

## 🔚 Summary

A well-calibrated XGBoost model can accurately predict IPL match outcomes at ball-level with high reliability (AUC > 0.90), using only match state features.
