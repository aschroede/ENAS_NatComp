# 🧪 Experiment Plan

> Purpose: Define what we're going to try, why we're trying it, and how we'll evaluate success.

---

## 📌 Objective
Clearly state what you're trying to achieve.
- **Example:** Improve model AUC on validation set by testing different encoding strategies.

---

## ❓ Research Question
What specific question is this experiment trying to answer?
- **Example:** Does target encoding outperform one-hot encoding for high-cardinality categorical features?

---

## 💡 Hypothesis
Make an educated guess about the expected result.
- **Example:** Target encoding will improve AUC by 1-2% due to reduced dimensionality and less sparsity.

---

## 🧰 Model(s)
List the model(s) involved:
- LightGBM (baseline)
- LightGBM + target encoding

---

## 🔧 Parameters to Search
List hyperparameters, their ranges, and method:
| Parameter       | Range            | Search Method |
|----------------|------------------|---------------|
| learning_rate  | 0.01 - 0.1       | Grid          |
| max_depth      | 3 - 10           | Random        |
| num_leaves     | 15 - 255         | Grid          |

---

## 🧪 Datasets / Splits
- `train.csv`
- `val.csv` (fold 0 of 5-fold CV)
- Test on: `val.csv`

---

## 📊 Metrics / Benchmarks
| Metric       | Target Value | Current Baseline |
|--------------|--------------|------------------|
| AUC (val)    | > 0.85       | 0.83             |
| Log Loss     | < 0.45       | 0.48             |

---

## 📈 Visualizations
- Feature importance comparison (baseline vs target encoding)
- ROC curves for both models
- Calibration plot

---

## 🧍‍♂️ Owner
- Name: Priya
- Status: 🟡 In Progress
- ETA: April 22

