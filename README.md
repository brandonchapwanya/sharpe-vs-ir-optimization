# Sharpe Ratio vs Information Ratio Portfolio Optimization

**Course:** FE630 Portfolio Theory and Applications | Stevens Institute of Technology | Spring 2026  
**Author:** Brandon Tanaka Chapwanya (Group Project)

> Academic project completed at Stevens Institute of Technology. All work is original. Do not submit as your own.

---

## Overview

This project compares two portfolio optimization strategies — Maximum Sharpe Ratio (Strategy I) and Maximum Information Ratio (Strategy II) — against a Minimum Variance benchmark across a 13-security universe spanning US equities, international equities, currencies, commodities, and fixed income.

Expected returns and covariance are estimated via the **Fama-French 5-Factor model** rather than raw sample statistics, providing more stable and theoretically grounded inputs — particularly for short rolling windows.

The core finding: **the 180-day lookback window consistently delivers the best risk-adjusted performance** across all strategies, with all three materially outperforming passive SPY indexing.

---

## Universe

| Ticker | Description | Asset Class |
|---|---|---|
| AAPL | Apple Inc. | US Large Cap Tech |
| FXE | Invesco CurrencyShares Euro ETF | Currency |
| EWJ | iShares MSCI Japan ETF | Developed Asia |
| GLD | SPDR Gold Shares | Gold / Safe Haven |
| QQQ | Invesco Nasdaq-100 ETF | US Growth Equities |
| SHV | iShares Short Treasury Bond ETF | Fixed Income |
| DBA | Invesco DB Agriculture Fund | Commodities |
| USO | United States Oil Fund | Commodities |
| XBI | SPDR S&P Biotech ETF | US Biotech |
| ILF | iShares Latin America 40 ETF | EM Latin America |
| EPP | iShares MSCI Pacific ex-Japan ETF | Developed Pacific |
| FEZ | SPDR EURO STOXX 50 ETF | European Equities |
| SPY | SPDR S&P 500 ETF (Benchmark) | US Large Cap |

**Analysis period:** March 2007 – March 2026 (covers GFC, COVID-19, 2022 rate-hike bear market)

---

## Methodology

### Fama-French 5-Factor Model
Each asset's excess return is decomposed into five systematic factors: MKT, SMB, HML, RMW, CMA. Factor-implied expected returns and covariance matrix are re-estimated at each rebalancing date, producing always-positive-semidefinite covariance estimates.

### Optimization Formulations
All three strategies solved as convex programs using **CVXPY with CLARABEL solver**:

- **MinVar:** Minimize portfolio variance subject to 20% annual return target
- **Strategy I (Max Sharpe):** Maximize (μ'ω − r₀) / √(ω'Σω) — convexified via Charnes-Cooper parameterization into SOCP
- **Strategy II (Max IR):** Maximize active return / tracking error volatility — convexified via Charnes-Cooper into SOCP

All strategies: long-only, weights sum to 1, maximum 60% per asset.

### Rolling Backtest
Weekly rebalancing across three lookback windows:
- **Short Term (ST):** 60 trading days
- **Mid Term (MT):** 180 trading days
- **Long Term (LT):** 250 trading days

---

## Key Results

### Performance vs SPY (Mid Term 180d Window)

| Strategy | Ann. Return | Volatility | Sharpe Ratio |
|---|---|---|---|
| MinVar | 9.87% | 8.74% | 0.55 |
| Strategy I (Max Sharpe) | 17.62% | 14.21% | **0.89** |
| Strategy II (Max IR) | 13.41% | 11.38% | 0.74 |
| SPY (Benchmark) | 10.15% | 14.52% | 0.35 |

### Crisis Period Performance (MT Window)

| Crisis | Strategy II Max Drawdown | SPY Max Drawdown |
|---|---|---|
| GFC (2008–2009) | −22.3% | −55.2% |
| COVID-19 (2020) | −11.4% | −33.9% |
| Rate Hike (2022) | −8.7% | −25.4% |

---

## Contents

| File | Description |
|---|---|
| `FE630W_Final_Project_Code.ipynb` | Full Python implementation |
| `FE630W_Final_Project_Report.pdf` | Complete project report |

---

## How to Run

```python
# Install required packages
pip install numpy pandas matplotlib cvxpy yfinance pandas_datareader

# Open and run the notebook
jupyter notebook FE630W_Final_Project_Code.ipynb
```

**Note:** Fama-French factor data is downloaded automatically from Kenneth French's data library via `pandas_datareader`.

---

## Concepts Demonstrated

- Fama-French 5-Factor model estimation and rolling re-estimation
- Charnes-Cooper parameterization for fractional programming (Sharpe, IR objectives)
- Second-Order Cone Programming (SOCP) via CVXPY / CLARABEL
- 19-year rolling backtest with weekly rebalancing
- Crisis period analysis: GFC, COVID-19, 2022 rate-hike bear market
- Rolling risk metrics: Sharpe, Sortino, VaR/CVaR, MDD/Volatility ratio
- Lookback window sensitivity analysis (60d vs 180d vs 250d)

---

## My Contributions

- Fama-French 5-Factor model implementation and rolling re-estimation framework
- Charnes-Cooper convexification of Sharpe and IR objectives into SOCP
- Crisis period analysis across GFC, COVID, and 2022 bear market
- Lookback window sensitivity analysis and optimal window identification
- Full report writing and results interpretation
