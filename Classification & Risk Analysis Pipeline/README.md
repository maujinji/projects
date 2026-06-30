# Classification & Risk Analysis Pipeline
### A Multi-Model Showcase with Interpretability and Calibration

**Author:** Mauricio Silva López  
**Affiliation:** Actuarial Science & Mathematics, UNAM (FES Acatlán / Facultad de Ciencias)  
**Domain:** HR Analytics — AI-Driven Layoff Risk Prediction

---

## Overview

A standardized, reusable, production-grade classification pipeline for tabular
datasets with binary or multiclass targets. Built around five core principles:
reusability, traceability, leak-proof preprocessing, model interpretability,
and probability calibration — not just accuracy.

The showcase dataset predicts **AI-driven layoff risk** (Low / Medium / High)
from employee and role features, but the pipeline is domain-agnostic: swap
the CSV path and target column to apply it to fraud detection, churn, credit
default, or any other tabular classification problem.

---

## What's inside

| Phase | Content |
|-------|---------|
| 1 — Setup | Environment configuration |
| 2 — EDA | Data health diagnostics + visual exploration (missingness, class balance, correlations) |
| 3 — Preprocessing | Leak-proof `ColumnTransformer` pipeline (median imputation + RobustScaler + OneHotEncoder) |
| 4 — Theory | Mathematical foundations: Logistic Regression, Random Forest, XGBoost, Stacking |
| 5 — Benchmarking | 5-fold stratified CV across 5 algorithms |
| 6 — Evaluation | Confusion matrices, ROC-AUC (OvR), F1, full classification report on held-out test set |
| 7 — Stacking Ensemble | Meta-learning architecture combining the 3 strongest base learners |
| 8 — Interpretability | SHAP — beeswarm, bar importance, and individual waterfall explanations |
| 9 — Calibration | Reliability diagrams + Brier score — does P(risk) actually mean risk? |
| 10 — Learning Curves | Bias/variance diagnosis across training set size |

---

## Why this is more than a template

Most classification pipelines stop at cross-validated accuracy. This one adds
three things that matter specifically for **risk analysis**:

1. **Calibration (Phase 9).** A model that says "80% High Risk" should be
   right 80% of the time. Tree-based models are often poorly calibrated
   out of the box — this is checked explicitly via reliability diagrams
   and the Brier score, not assumed.

2. **Individual-level interpretability (Phase 8).** Beyond global feature
   importance, the SHAP waterfall plot explains *any single prediction*,
   which satisfies the "right to explanation" required by many regulatory
   and compliance frameworks (e.g., GDPR Article 22, internal model risk
   governance).

3. **Bias/variance diagnosis (Phase 10).** Learning curves distinguish
   underfitting from overfitting before deciding whether to add complexity,
   more data, or regularization.

---

## Quickstart

### Requirements

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost shap
```

### Run

```bash
jupyter notebook Pipeline_v3.ipynb
```

Then **Run All**. The notebook ships with a calibrated synthetic dataset
generator, so it runs end-to-end with zero configuration.

### Use your own data

Change two lines in the Phase 2 cell:

```python
DATA_PATH = r"path/to/your_dataset.csv"   # was: None
TARGET    = "your_target_column"
```

The pipeline auto-detects numeric vs. categorical columns and routes them
through the appropriate preprocessing branch.

---

## Models compared

| Model | Family | Key property |
|-------|--------|--------------|
| Logistic Regression | Linear | Fast, interpretable, well-calibrated baseline |
| Random Forest | Bagging | Robust to outliers, built-in feature importance |
| XGBoost | Gradient Boosting | State-of-the-art on tabular data |
| Bagging (deep trees) | Bagging | Variance reduction |
| Gradient Boosting | Sequential Boosting | Strong predictor, no missing-value handling |
| **Stacking Ensemble** | Meta-learning | Combines RF + XGBoost + Bagging via logistic meta-learner |

---

## Repository structure

```
classification-risk-pipeline/
├── Pipeline_v3.ipynb      # Main notebook — all 10 phases, fully executable
├── README.md              # This file
└── eda_overview.png       # Saved EDA figure (generated on first run)
```

---

## Design principles

- **No data leakage** — all transformations (imputation, scaling, encoding)
  are fitted exclusively on training data inside a `sklearn.Pipeline`.
- **Stratified splits** — class balance is preserved across train/test and
  every CV fold via `stratify=y` and `StratifiedKFold`.
- **Reproducibility** — `random_state=42` fixed throughout.
- **Reusability** — domain-agnostic; works on any binary or multiclass
  tabular classification problem.

---

## References

1. Lundberg, S.M. & Lee, S.I. (2017). *A Unified Approach to Interpreting
   Model Predictions*. NeurIPS.
2. Chen, T. & Guestrin, C. (2016). *XGBoost: A Scalable Tree Boosting System*.
   KDD.
3. Wolpert, D.H. (1992). *Stacked generalization*. Neural Networks.
4. Niculescu-Mizil, A. & Caruana, R. (2005). *Predicting Good Probabilities
   with Supervised Learning*. ICML.

---

## License

MIT.
