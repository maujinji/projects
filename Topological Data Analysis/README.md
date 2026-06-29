# Topological Data Analysis for Systemic Risk Detection
### Real Market Data Results — Four Original Modules

**Author:** Mauricio Silva López  
**Data:** Yahoo Finance — ^GSPC · ^DJI · ^IXIC · ^RUT · ^VIX  
**Period:** 1995-01-04 → 2011-12-30 (4,281 trading days)

---

## Research question

> *Is the topological L1-norm a superior early warning signal compared to
> classical risk indicators, and how does that superiority depend on the
> observation horizon?*

---

## Key results (real data)

| Finding | Value |
|---------|-------|
| L1-norm lead time before dot-com crash | **680 days** (≈ 27 months) |
| L1-norm lead time before Lehman crash | **224 days** (≈ 9 months) |
| Mann-Kendall τ pre dot-com | +0.1756  (p = 3.56 × 10⁻⁵) ✓ |
| Mann-Kendall τ pre Lehman | +0.2950  (p = 3.70 × 10⁻¹²) ✓ |
| TRI logistic regression CV AUC | 0.6725 ± 0.0765 |
| PCA PC1 explained variance | 52.2% |

---

## Four original modules

| Module | Original question | Key result |
|--------|-----------------|-----------|
| **1 — Benchmark** | Does L1 fire *earlier* than VIX / realized vol / rolling corr? | L1 leads by 680d before dot-com — more than any classical indicator |
| **2 — Mann-Kendall** | Is the pre-crash L1 rise statistically significant? | Both crashes p < 10⁻⁴ |
| **3 — TRI** | Can 4 TDA features be fused into one predictive index? | AUC = 0.67, PC1 explains 52% |
| **4 — Sensitivity** | SNR–lead-time trade-off across window sizes? | No universal w*; small w → earlier, large w → stronger τ |

---

## Reference papers

| Paper | Role |
|-------|------|
| Gidea & Katz (2017), *Physica A* | Baseline: L1-norm of persistence landscapes as EWS |
| De Jesus et al. (2025), *Neural Computing & Applications* | Features: entropy, amplitude, N(D) |

---

## Repository structure

```
tda-systemic-risk/
├── tda_real_data.ipynb          # Main notebook — all modules, real data, figures embedded
├── tda_step1_save_csvs.py       # Data collection script (run locally to regenerate CSVs)
├── README.md                    # This file
└── data/
    ├── out_us_returns.csv
    ├── out_vix.csv
    ├── out_us_tda_w25.csv
    ├── out_us_tda_w50.csv
    ├── out_us_tda_w75.csv
    └── out_us_tda_w100.csv
```

---

## Quickstart

```bash
pip install yfinance ripser pymannkendall scikit-learn matplotlib pandas numpy scipy jupyter
python tda_step1_save_csvs.py   # ~30 min — generates all CSVs
jupyter notebook tda_real_data.ipynb
```

---

## What makes this original (vs. both reference papers)

| Contribution | Gidea & Katz 2017 | De Jesus 2025 |
|-------------|:-----------------:|:-------------:|
| Lead-time benchmark vs VIX + classical indicators | ✗ | ✗ |
| Systematic window sensitivity (SNR / lead / τ) | ✗ | ✗ |
| TRI via PCA + logistic regression with CV AUC | ✗ | ✗ |
| Real yfinance data with ^VIX comparison | ✗ | ✗ |

---

## References

1. Gidea & Katz (2017). *Topological Data Analysis of Financial Time Series: Landscapes of Crashes*. Physica A. arXiv:1703.04385.
2. De Jesus Jr. et al. (2025). *Enhancing financial time series forecasting through topological data analysis*. Neural Computing & Applications, 37, 6527–6545.
3. Bubenik (2015). *Statistical topological data analysis using persistence landscapes*. JMLR 16:77–102.

---
MIT License. Code is original; mathematical results follow cited references.
