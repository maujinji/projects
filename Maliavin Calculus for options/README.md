# Malliavin Calculus for Option Greeks — Monte Carlo Estimation

**Author:** Mauricio Silva López  


---

## Overview

This project implements the Malliavin Calculus framework for computing option
sensitivities (**Greeks**) via Monte Carlo simulation under the Black-Scholes model.
It is inspired by and extends Montero & Kohatsu-Higa (2003), *"Malliavin Calculus
applied to Finance"*, Physica A.

The central idea: when a payoff function Φ is non-differentiable (digital options,
barrier knock-outs, Asian averages), standard finite differences become unreliable.
Malliavin's stochastic **integration by parts** transfers the derivative of Φ onto
a probabilistic *weight* H, giving an unbiased Monte Carlo estimator that works
regardless of payoff smoothness:

```
E[Φ'(X) · Y] = E[Φ(X) · H]
```

---

## Repository Structure

```
malliavin-greeks/
├── malliavin_finanzas.ipynb     # Main Jupyter notebook (fully executed)
├── malliavin_report.pdf         # Academic report (LaTeX, 8 pages)
├── malliavin_report.tex         # LaTeX source
├── README.md                    # This file
└── figures/
    ├── fig1_europeas.png        # Delta & Vega convergence (European)
    ├── fig3_asiatica.png        # Asian Delta — 3 estimators
    ├── fig4_moneyness.png       # Delta vs. moneyness (Vanilla / Asian / Exotic)
    ├── figA_gamma.png           # Gamma convergence [original]
    ├── figB_epsilon.png         # FD epsilon sensitivity [original]
    └── figC_barrier.png         # Down-and-Out Barrier Call [original]
```

---

## What's Covered

### Reproducing the Article (Montero & Kohatsu-Higa 2003)

| Section | Content | Figure |
|---------|---------|--------|
| §4.1 | Malliavin weights for Δ, V, Γ (European call) | Fig. 1 & 2 |
| §5   | Asian call — 3 Malliavin estimators of Δ | Fig. 3 |
| §6   | Exotic Best-of (European ∨ Asian) — Δ vs. moneyness | Fig. 4 |

### Original Contributions (not in the article)

**1. Gamma Convergence (`figA_gamma.png`)**  
The article omits the Gamma convergence figure, noting it follows from the
identity Γ = V/(S₀²σT). We plot it explicitly and verify numerical consistency
independently of this identity. The Malliavin estimator converges to
Γ_BS = 0.016661 with variance ≈ 0.0228, orders of magnitude smaller than
the Vega variance (≈ 91,314).

**2. Finite-Difference ε Sensitivity (`figB_epsilon.png`)**  
We sweep ε ∈ [0.01, 10] for the central-difference Delta estimator and
decompose the MSE into bias² + Var/N. Three regimes emerge:
- Small ε (< 0.1): variance explodes, MSE >> Malliavin
- Moderate ε (≈ 0.5–2): MSE is minimized but still above Malliavin
- Large ε (> 2): quadratic bias dominates

The Malliavin estimator achieves lower MSE than the optimal FD choice
at N = 10,000, with no free parameter to tune.

**3. Down-and-Out Barrier Call (`figC_barrier.png`)**  
We extend the Malliavin IBP formula to a path-dependent payoff:

```
Φ_B(S_T) = (S_T − K)⁺ · 1{min_{t∈[0,T]} S_t > B}
```

The knock-out indicator is discontinuous in the Brownian path, making the
direct method invalid. Using the universal weight from eq. (23) of the article:

```
Δ_B = E[Φ_B · H_exotic] / S₀
     where H = 6∫S²dt/(∫Sdt)² − (4S₀ + 2S_T)/∫Sdt − r
```

Results for B = 85, K = 100, S₀ = 100: Barrier price ≈ 12.60,
Barrier Delta ≈ 0.030 (vs. vanilla Δ ≈ 0.726).

---

## Quickstart

### Requirements

```bash
pip install numpy scipy matplotlib nbformat jupyter
```

### Run the notebook

```bash
jupyter notebook malliavin_finanzas.ipynb
```

### Compile the report

```bash
pdflatex malliavin_report.tex
pdflatex malliavin_report.tex   # twice for TOC/refs
```

---

## Parameters

All experiments use the following base configuration (matching the article):

| Parameter | Value | Description |
|-----------|-------|-------------|
| S₀        | 100   | Initial asset price |
| K         | 100   | Strike price (ATM) |
| r         | 0.10  | Risk-free rate (10%) |
| σ         | 0.20  | Volatility (20%) |
| T         | 1.0   | Time to maturity (years) |
| N         | 10,000 | Monte Carlo paths |
| N_sub     | 252   | Discrete time steps (trading days) |
| B         | 85    | Barrier level (barrier section only) |

---

## Key Results Summary

| Greek | Option | Method | MC Estimate | Reference |
|-------|--------|--------|-------------|-----------|
| Δ | Vanilla Call | Malliavin | 0.7287 | 0.7257 (B-S) |
| V | Vanilla Call | Malliavin | 34.57  | 33.32 (B-S) |
| Γ | Vanilla Call | Malliavin | 0.01729 | 0.01666 (B-S) |
| Δ | Asian Call | Malliavin (est. 3) | 0.580 | ≈ 0.65 |
| Δ | Barrier Call (B=85) | Malliavin | 0.0297 | — (no closed form) |

---

## References

1. M. Montero & A. Kohatsu-Higa, *Malliavin Calculus applied to Finance*,
   Physica A 320 (2003) 548–570.
2. E. Fournié, J.-M. Lasry, J. Lebuchoux, P.-L. Lions, N. Touzi,
   *Applications of Malliavin calculus to Monte Carlo methods in finance*,
   Finance & Stochastics 3 (1999) 391–412.
3. E. Fournié et al., *Applications of Malliavin calculus... II*,
   Finance & Stochastics 5 (2001) 201–236.
4. D. Nualart, *The Malliavin Calculus and Related Topics*, Springer, 1995.

---

## License

MIT. Code is original; mathematical results follow the cited references.
