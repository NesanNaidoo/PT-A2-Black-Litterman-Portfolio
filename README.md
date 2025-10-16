# Black-Litterman Portfolio Optimization: Rolling Window Analysis

A quantitative finance project implementing the Black-Litterman model with rolling window backtesting on JSE (Johannesburg Stock Exchange) market data from 1994-2017.

## Project Overview

This analysis applies the Black-Litterman framework to construct optimal portfolios using South African market indices. The implementation uses a 60-month training window and 12-month testing period to evaluate portfolio performance both in-sample and out-of-sample.

## Data

**Source**: JSE Daily data (1994-2017)  
**File**: `PT-TAA-JSE-Daily-1994-2017.xlsx`

**Assets included**:
- ALBI (All Bond Index)
- STEFI (Short-Term Fixed Interest)
- ALSI (All Share Index)
- J203, J500 (JSE indices)
- J510-J590 (JSE sector indices)

## Methodology

### Black-Litterman Model

The implementation combines market equilibrium returns with investor views to generate posterior expected returns and optimal portfolio weights.

**Key parameters**:
- τ (tau): 0.05 - controls prior confidence
- γ (gamma): 3.0 - risk aversion parameter
- Training window: 60 months (5 years)
- Testing window: 12 months (1 year)
- Rolling step: 1 month

### Portfolio Constraints

- Long-only positions (no short selling)
- Fully invested (weights sum to 1)
- Cash asset (STEFI) excluded from optimization

### Performance Metrics

**In-Sample (IS) and Out-of-Sample (OOS)**:
- Mean return
- Variance
- Sharpe ratio
- Jensen's alpha (relative to ALSI)
- Portfolio beta (relative to ALSI)

**Additional metrics**:
- Portfolio turnover
- Tracking error vs ALSI benchmark
- Equity curve evolution

## Requirements

### R Environment
- R version: Compatible with RStudio 2024.09.0+375
- R version: 4.x or higher recommended

### Required Libraries

```r
openxlsx          # Excel file handling
timeSeries        # Time series operations
xts               # Extended time series
zoo               # Time series infrastructure
matrixStats       # Matrix computations
quadprog          # Quadratic programming
knitr             # Report generation
dplyr             # Data manipulation
ggplot2           # Visualization
tidyr             # Data tidying
PerformanceAnalytics  # Performance metrics
```

Install all dependencies:

```r
install.packages(c("openxlsx", "timeSeries", "xts", "zoo", 
                   "matrixStats", "quadprog", "knitr", "dplyr", 
                   "ggplot2", "tidyr", "PerformanceAnalytics"))
```

## Project Structure

```
Analytics_A2/
├── NDXNES005_A2_RMD.Rmd          # Main analysis script
├── _raw_data/
│   └── PT-TAA-JSE-Daily-1994-2017.xlsx
├── _citation_style/
│   └── apa.csl                    # Citation formatting
├── Sources.bib                    # Bibliography
└── README.md
```

## Usage

### Running the Analysis

1. **Open RStudio** and set working directory to project folder

2. **Ensure data file is in correct location**:
   ```r
   # Data should be at: _raw_data/PT-TAA-JSE-Daily-1994-2017.xlsx
   ```

3. **Run the R Markdown file**:
   ```r
   rmarkdown::render("NDXNES005_A2_RMD.Rmd")
   ```

4. **Output**: PDF document with complete analysis and visualizations

### Key Functions

**blacklitterman(Pi, Sigma, P, Q, Omega, tau, gamma, constrain)**

Computes Black-Litterman posterior estimates and optimal weights.

- `Pi`: Prior equilibrium returns (n×1)
- `Sigma`: Covariance matrix (n×n)
- `P`: View matrix (k×n)
- `Q`: View returns (k×1)
- `Omega`: View uncertainty (k×k)
- `tau`: Prior confidence scalar
- `gamma`: Risk aversion parameter
- `constrain`: Apply non-negativity constraint

Returns:
- `weights`: Optimal portfolio weights
- `mu_post`: Posterior mean returns
- `Sigma_post`: Posterior covariance matrix

## Key Results

The analysis produces:

1. **Performance comparison tables**: IS vs OOS statistics across rolling windows
2. **Turnover analysis**: Portfolio rebalancing costs over time
3. **Risk-return visualization**: Alpha and beta evolution
4. **Weight heatmaps**: Asset allocation changes across windows
5. **Tracking error plots**: Portfolio deviation from ALSI benchmark

## Technical Notes

### Data Processing

- Daily prices converted to monthly frequency
- Geometric returns calculated using log differences
- Missing values handled with LOCF (Last Observation Carried Forward)
- All-NA columns removed before optimization

### Model Specifications

**Example view construction**:
```r
P <- matrix(0, nrow=1, ncol=n_assets)
P[1, 1:3] <- 1/3          # Equal-weighted view on first 3 assets
Q <- matrix(0.01, nrow=1)  # Expected 1% return
Omega <- matrix(0.0001, nrow=1)  # Low view uncertainty
```

### CAPM Analysis

Jensen's alpha and beta calculated using:
- Excess returns: Portfolio return - Risk-free rate
- Benchmark: ALSI index
- Linear regression: Excess portfolio returns ~ Excess benchmark returns

## References

Code based on materials provided by Professor Tim Gebbie (STA4028Z), University of Cape Town.

## Author

Student ID: Nesan Naidoo
Course: STA4028Z - Portfolio Theory
Institution: University of Cape Town

## License

Academic project - all rights reserved.

## Acknowledgments

- Professor Tim Gebbie for foundational R and MATLAB code
- JSE for market data provision
- Black-Litterman model: Black, F., & Litterman, R. (1992)
