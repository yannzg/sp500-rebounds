# S&P 500 Post-Crash Rebound Analysis

A quantitative study examining whether large single-stock crashes in the S&P 500 are followed by statistically significant positive returns (2015–2025).

**TLDR:** No. Crashed stocks continue declining on average, underperforming baseline expectations by **28% over the following year**.


## Motivation

One of the most popular investing wisdom advises you to "buy the dip", meaning that stocks bounce back after sharp crashes and subsequently reward contrarian investors. High-profile recoveries like the 34% COVID crash of March 2020 (which recovered within weeks) reinforce this narrative. This project tests that claim rigorously across a sample of 50 stocks and a decade of data.


## Key Finding

> **Crashed stocks exhibit momentum, not reversal.** Post-crash abnormal returns are significantly *negative* at every horizon beyond 1 day, and the underperformance compounds over time.

| Horizon | Mean Abnormal Return | 95% Bootstrap CI | p-value |
|--------:|---------------------:|:----------------:|:-------:|
| 1 day   | −0.116%              | [−0.295, +0.064] | 0.208   |
| 5 days  | −0.854%              | [−1.165, −0.544] | <0.001  |
| 20 days | −1.599%              | [−2.131, −1.068] | <0.001  |
| 60 days | −3.111%              | [−3.810, −2.412] | <0.001  |
| 90 days | −4.185%              | [−4.981, −3.388] | <0.001  |
| 180 days| −13.510%             | [−14.492, −12.527]| <0.001 |
| 365 days| **−28.413%**         | [−29.917, −26.909]| <0.001 |

*n = 3,034 crash events across 50 stocks. Abnormal return = actual log return − expected return (stock's historical mean × n days).*

By one year post-crash, only **18.8%** of crash events show positive abnormal returns — worse than a coin flip.


## Methodology

50 S&P 500 stocks sampled via stratified random sampling (by GICS sector), covering January 2015 through December 2025. Crashes are defined per-stock using a μ − 2σ threshold on daily log returns. Abnormal return is the actual cumulative return minus the stock's own historical baseline. Significance tested with one-sample t-tests and bootstrap CIs (B = 10,000). Recovery trajectories grouped into four archetypes via K-means clustering.

Full walkthrough with rationale, formulas, and EDA in `analysis.ipynb`.


## Visualizations

The notebook produces a full suite of charts:

- S&P 500 price history with major event markers (COVID-19, trade wars, Brexit, etc.)
- Rolling volatility regimes (30-day and 90-day, color-coded by volatility level)
- Crash threshold distribution across the 50 sampled companies
- Rebound vs. underperformance rates at each horizon
- Bootstrap sampling distribution overlays (normal CI vs. percentile CI)
- K-means elbow plot and recovery archetype overlays
- Sector-level heatmap of post-crash returns


## Setup

```bash
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook analysis.ipynb
```

Run cells in order. Data is fetched live via `yfinance` on first run.

**Dependencies:** `pandas`, `numpy`, `yfinance`, `scikit-learn`, `matplotlib`, `seaborn`, `scipy`, `jupyter`

## Limitations & Caveats

A few limitations worth taking into account: 

- **Survivorship bias:** the analysis uses active S&P 500 constituents. But stocks that were delisted or went bankrupt after crashing are excluded, which likely makes results *more optimistic* than reality.
- **Normality assumption:** return distributions empircally show fat tails and not normal distribution.
- **Sample size:** 50 stocks only represents 10% of the index with stratified sampling. 
- **Single market:** findings only apply to large-cap U.S. equities.

