# Portfolio Risk Analytics Framework

A Python-based quantitative portfolio management and risk analytics framework combining portfolio optimization, factor analysis, risk measurement, scenario testing, rebalancing, and benchmark evaluation.

The project develops and evaluates both an **active U.S. equity portfolio** and a **passive DJIA-tracking portfolio**, with a focus on translating quantitative models into practical portfolio and risk-management decisions.

---

## Project Overview

The framework was developed to answer several practical portfolio management questions:

- How can expected returns be estimated without relying solely on historical averages?
- How can covariance estimation error be reduced when constructing portfolios?
- How should portfolio weights be optimized under realistic investment constraints?
- What factors drive portfolio returns and risk exposures?
- How much downside risk could the portfolio face under adverse market conditions?
- When should a portfolio be rebalanced?
- How does an active strategy perform relative to major benchmarks?
- How accurately can a passive portfolio replicate the DJIA?
- How can index futures be used to manage systematic exposure and cash drag?

The analysis combines **fundamental views, market information, statistical modeling, portfolio optimization, and risk controls** into one end-to-end workflow.

---

## Framework

```text
Market & Fundamental Data
          ↓
Return & Covariance Estimation
          ↓
Covariance Shrinkage
          ↓
Black–Litterman Expected Returns
          ↓
Constrained Portfolio Optimization
          ↓
Portfolio Construction
          ↓
Factor & Risk Diagnostics
          ↓
Monte Carlo Risk Simulation
          ↓
VaR / Downside Risk Analysis
          ↓
Scenario Analysis
          ↓
Rebalancing & Risk Monitoring
          ↓
Performance Evaluation
```

---

## 1. Data & Investment Universe

The active strategy is constructed from large-cap U.S. equities within the **Dow Jones Industrial Average (DJIA)** universe.

Historical market data are collected and processed in Python for:

- Price and return analysis
- Market capitalization
- Covariance estimation
- Portfolio optimization
- Backtesting
- Risk measurement
- Benchmark comparison

The framework also incorporates:

- Risk-free rate data
- Fama–French factor data
- Fundamental company analysis
- Sell-side analyst target prices
- Market-implied expected returns

---

## 2. Covariance & Risk Estimation

A central issue in portfolio optimization is estimation error in the variance-covariance matrix.

Instead of relying only on the sample covariance matrix, the project applies **covariance shrinkage techniques** to improve stability.

Implemented approaches include:

- Sample covariance
- Ledoit–Wolf covariance shrinkage
- Shrinkage coefficient optimization
- Cross-validation
- Global Minimum Variance comparison
- EWMA / RiskMetrics covariance
- Constant-correlation covariance
- Factor-based covariance estimation

The shrinkage framework helps reduce sensitivity to noisy historical estimates and provides a more stable input for portfolio optimization.

---

## 3. Black–Litterman Portfolio Optimization

Expected returns are generated using the **Black–Litterman model**.

The framework starts with market-implied equilibrium returns and incorporates investor views derived from:

- Fundamental analysis
- Market outlook
- Analyst target prices
- Stock-specific expectations

Black–Litterman posterior returns are then used within a constrained optimization framework.

### Portfolio constraints

The optimization incorporates practical investment controls such as:

- Long-only positions
- Full capital allocation
- Sector allocation limits
- Single-stock exposure limits
- Diversification requirements
- Liquidity considerations

The objective is to maximize expected risk-adjusted performance while preventing unrealistic portfolio concentration.

---

## 4. Factor & Risk Attribution

The portfolio is evaluated using the **Fama–French Three-Factor Model**:

- **Mkt–RF:** Market risk
- **SMB:** Size exposure
- **HML:** Value exposure

The analysis indicates that **market risk is the dominant driver of portfolio returns**, while size exposure is structurally limited by the large-cap DJIA universe and value exposure remains moderate.

The factor model is therefore used primarily as a **risk and style diagnostic**, rather than as the main portfolio construction engine.

---

## 5. Monte Carlo Risk Simulation

Forward-looking portfolio risk is assessed using **10,000 Monte Carlo simulations**.

The simulation framework:

1. Estimates return distributions for portfolio assets
2. Preserves cross-asset dependence through the covariance matrix
3. Simulates correlated return scenarios
4. Applies percentile mapping to better reflect empirical return behavior
5. Generates simulated ending portfolio values
6. Evaluates the distribution of potential portfolio outcomes

This provides a distribution-based view of risk rather than relying on a single expected return estimate.

---

## 6. Value-at-Risk & Downside Risk

Portfolio downside exposure is evaluated using:

- **95% Value-at-Risk**
- **99% Value-at-Risk**
- Historical VaR
- Conditional VaR / CVaR
- Maximum Drawdown
- Volatility
- Sharpe Ratio

The simulated portfolio-value distribution allows extreme downside outcomes to be examined directly.

The 95% and 99% VaR thresholds provide estimates of potential losses under adverse but plausible market conditions.

---

## 7. Scenario Analysis

The robustness of the portfolio is also evaluated across different market regimes:

### Bullish Market
Tests portfolio upside participation under favorable market conditions.

### Bearish Market
Evaluates downside sensitivity and capital preservation during stressed markets.

### Stable Market
Measures portfolio behavior under relatively low-volatility conditions.

The same portfolio construction is replayed across different historical environments to identify **regime dependence, downside sensitivity, and changes in risk-return behavior**.

---

## 8. Rebalancing & Risk Monitoring

The active portfolio uses a **drift-based rebalancing framework**.

Target portfolio weights are monitored through time, with a **±5% tolerance band** around the optimized allocation.

```text
Target Weight
     ↓
Monitor Actual Weight
     ↓
Within ±5%? ── Yes → Maintain Position
     │
     No
     ↓
Rebalance Portfolio
     ↓
Restore Target Allocation
```

A rebalance may also be considered when:

- The fundamental investment thesis changes
- Market conditions materially change
- Sector exposures move outside intended limits

During the analyzed backtest, portfolio weights remained inside the drift threshold, avoiding unnecessary turnover while preserving the intended portfolio structure.

---

## 9. Active Portfolio Performance

### Backtest Period
**1 October 2024 – 20 November 2025**

### Initial Capital
**USD 50 million**

| Metric | Active Portfolio |
|---|---:|
| Total Return | **42.12%** |
| Annualized Return | **36.45%** |
| Annualized Volatility | **20.86%** |
| Sharpe Ratio | **1.60** |
| Maximum Drawdown | **-20.59%** |
| Final Portfolio Value | **USD 68.70 million** |

The portfolio materially outperformed the DJIA over the analyzed period while producing a higher risk-adjusted return.

Performance was supported by selective exposure to high-quality cyclicals, financials, technology, and defensive holdings.

---

## 10. Passive DJIA Portfolio

The project also constructs a **passive price-weighted DJIA portfolio**.

The strategy is designed to replicate the benchmark while minimizing tracking error.

Key features include:

- DJIA price-weight replication
- Event-driven rebalancing
- Corporate-action adjustments
- Tracking-error measurement
- Rolling beta analysis
- Performance attribution
- Index futures overlays
- Cash equitization

### Passive Portfolio Results

| Metric | Result |
|---|---:|
| Ending Portfolio Value | **USD 55.80 million** |
| Total Return | **11.59%** |
| Annualized Volatility | **16.45%** |
| Sharpe Ratio | **0.43** |
| Tracking Error | **0.22%** |

The low tracking error demonstrates close replication of the DJIA benchmark.

---

## 11. Futures Hedging & Cash Equitization

DJIA index futures are incorporated to examine two different applications.

### Cash Equitization

Index futures can convert temporarily idle cash into synthetic equity exposure, helping reduce unintended benchmark underexposure and tracking error.

### Systematic Risk Hedging

A short DJIA futures overlay is also evaluated as a method for reducing market beta during stressed periods.

The analysis demonstrates the trade-off between:

- Benchmark participation
- Portfolio volatility
- Market beta
- Drawdown protection
- Tracking error

This illustrates how derivatives can be used not only for return generation, but also for **exposure management and risk control**.

---

## 12. Performance & Risk Metrics

The framework calculates and evaluates:

- Total Return
- Annualized Return
- Annualized Volatility
- Sharpe Ratio
- Beta
- Tracking Error
- Maximum Drawdown
- Value-at-Risk
- Conditional Value-at-Risk
- Factor Exposure
- Portfolio Attribution
- Portfolio Value
- Benchmark Relative Performance

---

## Technology Stack

### Programming
- Python
- Jupyter Notebook

### Data Analysis
- Pandas
- NumPy
- SciPy

### Machine Learning & Statistics
- Scikit-learn
- Statsmodels

### Portfolio Analytics
- PyPortfolioOpt
- Black–Litterman Model
- Mean–Variance Optimization
- Covariance Shrinkage

### Data Sources & Processing
- yfinance
- Fama–French factor data
- Analyst target-price inputs

### Visualization
- Matplotlib
- Seaborn
- Plotly

---

## Repository Structure

```text
Portfolio-Risk-Analytics-Framework/
│
├── Code.ipynb
│   └── Main portfolio construction, optimization,
│       risk analysis and backtesting notebook
│
├── utils.py
│   └── Reusable functions and classes for data processing,
│       portfolio analytics, risk measurement and visualization
│
├── ETF Portfolio Performance_Report_Quant Approach.pdf
│   └── Full portfolio management and quantitative analysis report
│
└── README.md
    └── Project documentation
```

Additional input files used by the notebook include:

```text
target_prices_djia.xlsx
F-F_Research_Data_Factors_daily.xlsx
```

---

## Running the Project

Clone the repository:

```bash
git clone <repository-url>
cd Portfolio-Risk-Analytics-Framework
```

Install the main dependencies:

```bash
pip install numpy pandas scipy matplotlib seaborn plotly \
scikit-learn statsmodels yfinance PyPortfolioOpt openpyxl
```

Place the required input files in the project directory:

```text
target_prices_djia.xlsx
F-F_Research_Data_Factors_daily.xlsx
```

Then open:

```text
Code.ipynb
```

and run the notebook sequentially.

---

## Key Takeaways

This project demonstrates the integration of **portfolio construction and risk management** rather than treating them as separate tasks.

The main analytical insights are:

- Covariance shrinkage can improve the stability of portfolio risk estimates.
- Black–Litterman provides a structured way to combine market equilibrium information with fundamental investment views.
- Portfolio constraints are necessary to translate mathematical optimization into implementable allocations.
- Market exposure remained the dominant source of systematic risk in the active portfolio.
- Monte Carlo simulation and VaR provide a forward-looking view of downside exposure.
- Scenario analysis highlights how portfolio risk can change substantially across market regimes.
- Rule-based rebalancing can preserve target risk exposures while limiting unnecessary turnover.
- Tracking error is critical when evaluating passive strategies.
- Index futures can be used either to maintain benchmark exposure or reduce systematic market risk.

---

## Disclaimer

This repository was developed for **academic and educational purposes**. The analysis, models, portfolio allocations, and results presented here should not be interpreted as investment advice or as recommendations to buy or sell any financial instrument.

Historical and simulated performance does not guarantee future results.
