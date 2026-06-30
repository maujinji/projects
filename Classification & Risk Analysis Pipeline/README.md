# Classification & Quantitative Risk Analysis Pipeline
## AI-Driven Layoff Risk Prediction

**Author:** Mauricio Silva López

**Domain:** Quantitative Risk Analysis & Data Science | Independent Professional Portfolio

---

# Overview

A standardized, reusable, production-grade classification pipeline for tabular datasets with binary or multiclass targets.

Built around five core principles:

- Reusability
- Traceability
- Leak-proof preprocessing
- Model interpretability
- Probability calibration

—not just accuracy.

The showcase dataset predicts **AI-driven layoff risk** (Low / Medium / High) from **20,000 synthetic employee and role records**.

The pipeline is domain-agnostic: simply replace the CSV path and target column to apply it to fraud detection, churn prediction, credit default, customer segmentation, or any other tabular classification problem.

---

# Key Results Summary (Out-of-Sample Evaluation)

Contrary to the common assumption that complex ensemble methods always outperform linear models on tabular data, this dataset revealed an almost perfectly linear decision boundary.

The simplest model (**Multinomial Logistic Regression**) achieved both:

- the highest out-of-sample accuracy
- the best calibrated probabilities (lowest Brier Score)

| Model | Test Accuracy | ROC-AUC (OvR) | Brier Score |
|--------|--------------:|--------------:|------------:|
| Logistic Regression | **93.75%** | **0.9939** | **0.0431** |
| Stacking Ensemble | 92.42% | 0.9895 | 0.0536 |
| Gradient Boosting | 91.30% | 0.9838 | 0.0828 |
| XGBoost | 90.58% | 0.9814 | 0.0915 |
| Bagging (Deep Trees) | 90.53% | 0.9819 | 0.0764 |
| Random Forest | 83.08% | 0.9558 | 0.1367 |

---

# Four Analytical Modules

| Module | Objective | Main Finding |
|---------|-----------|--------------|
| **1. Algorithmic Occam's Razor** | Does a Stacking Ensemble outperform a linear baseline? | No. Logistic Regression achieved **93.86% CV Accuracy (±0.13%)**, outperforming Random Forest (82.9%) and XGBoost (89.3%). |
| **2. Probability Calibration** | Can predicted probabilities be trusted? | Logistic Regression closely follows the reliability curve (Brier = 0.043), while Random Forest is substantially miscalibrated (Brier = 0.136). |
| **3. SHAP Interpretability** | Which variables explain Medium and High Risk predictions? | Routine_Task_Percentage and Tasks_Automated_Percentage dominate predictions. Creativity_Requirement and Job_Level_Senior consistently reduce risk. |
| **4. Learning Curves** | Is the model underfitting or overfitting? | Logistic Regression shows an almost perfect bias-variance balance (Gap = 0.003). XGBoost exhibits mild overfitting (Gap = 0.045). |

---

# Pipeline Structure

| Phase | Content |
|-------|---------|
| 1 | Environment setup |
| 2 | Exploratory Data Analysis (EDA) |
| 3 | Leak-proof preprocessing pipeline |
| 4 | Mathematical foundations |
| 5 | Model benchmarking |
| 6 | Final evaluation |
| 7 | Stacking Ensemble |
| 8 | SHAP explainability |
| 9 | Probability calibration |
| 10 | Learning curves |

---

# What's Included

## Phase 1 — Setup

Project configuration and library imports.

## Phase 2 — Exploratory Data Analysis

- Missing value diagnostics
- Feature distributions
- Correlation heatmaps
- Class balance
- Outlier inspection

---

## Phase 3 — Preprocessing

Leak-proof preprocessing built with **ColumnTransformer** and **Pipeline**.

### Numeric features

- Median imputation
- RobustScaler

### Categorical features

- Most frequent imputation
- OneHotEncoder

---

## Phase 4 — Theory

Mathematical foundations behind:

- Logistic Regression
- Random Forest
- Gradient Boosting
- XGBoost
- Stacking Ensembles

---

## Phase 5 — Benchmarking

Comparison using:

- 5-fold Stratified Cross Validation
- Accuracy
- Precision
- Recall
- F1 Score

---

## Phase 6 — Final Evaluation

Evaluation on a completely unseen test set.

Includes:

- Confusion matrices
- ROC-AUC (One-vs-Rest)
- Classification report
- Performance comparison

---

## Phase 7 — Stacking Ensemble

Meta-learning architecture combining the strongest individual models.

---

## Phase 8 — Model Interpretability

Using SHAP:

- Beeswarm summary
- Feature importance
- Individual waterfall explanations

---

## Phase 9 — Probability Calibration

Calibration analysis through:

- Reliability diagrams
- Brier Score

Evaluates whether predicted probabilities correspond to observed frequencies.

---

## Phase 10 — Learning Curves

Bias-variance diagnostics across increasing training set sizes.

---

# Why This Pipeline Goes Beyond Standard Templates

Most machine learning repositories stop after reporting cross-validation accuracy.

This project adds three components that are essential for quantitative risk analysis:

### Probability Calibration

A classifier that predicts an 80% probability should be correct approximately 80% of the time.

Calibration is explicitly evaluated using:

- Reliability diagrams
- Brier Score

rather than assumed.

---

### Individual-Level Explainability

Beyond global feature importance, SHAP waterfall plots explain **why a single prediction was made**, supporting:

- Regulatory transparency
- Internal model governance
- Explainable AI

---

### Bias-Variance Diagnostics

Learning curves objectively determine whether performance improvements require:

- More data
- Better features
- Increased model complexity
- Additional regularization

instead of relying on intuition.

---

# Quick Start

## Requirements

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost shap
```

Run the notebook:

```bash
jupyter notebook Pipeline_v3.ipynb
```

---

# Use Your Own Dataset

Simply modify two variables inside **Phase 2**:

```python
DATA_PATH = r"path/to/your_dataset.csv"
TARGET = "your_target_column"
```

The pipeline automatically detects:

- Numerical features
- Categorical features

and routes each through the appropriate preprocessing branch.

---

# Repository Structure

```text
classification-risk-pipeline/
│
├── Pipeline_v3.ipynb
├── README.md
│
└── figures/
    ├── eda_overview.png
    ├── benchmarking.png
    ├── confusion_mat.png
    ├── roc_curves.png
    ├── shap_summary.png
    ├── shap_waterfall.png
    ├── calibration.png
    └── learning_curve.png
```

---

# Design Principles

- No data leakage
- End-to-end sklearn Pipeline
- Stratified train/test split
- Stratified cross-validation
- Fixed random_state = 42
- Fully reproducible
- Domain agnostic
- Binary and multiclass compatible

---

# References

Lundberg, S.M., & Lee, S.I. (2017). *A Unified Approach to Interpreting Model Predictions.* NeurIPS.

Chen, T., & Guestrin, C. (2016). *XGBoost: A Scalable Tree Boosting System.* KDD.

Wolpert, D.H. (1992). *Stacked Generalization.* Neural Networks.

Niculescu-Mizil, A., & Caruana, R. (2005). *Predicting Good Probabilities with Supervised Learning.* ICML.

---

# License

MIT License.

The implementation is original. Mathematical metrics, algorithms, and evaluation methodologies follow standard machine learning and quantitative risk analysis literature.
