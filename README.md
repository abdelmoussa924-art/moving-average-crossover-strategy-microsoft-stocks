# 📈 Moving Average Crossover Strategy — Microsoft Stock

A backtest of a classic MA10/MA50 crossover trading strategy on Microsoft (MSFT) stock, evaluated against a buy-and-hold benchmark using P&L tracking, drawdown analysis, and the Sharpe ratio.

## 🎯 Objective

Implement and backtest a simple **moving average crossover strategy** (golden cross / death cross logic), then evaluate its performance against the market using standard risk-adjusted metrics:

- Cumulative wealth (P&L)
- Maximum drawdown
- Sharpe ratio (risk-adjusted return vs. ECB risk-free rate)
- Strategy vs. benchmark comparison

## 📊 Strategy Logic

The strategy goes **long (1 share)** when the short-term trend is above the long-term trend, and **flat (0 shares)** otherwise:
MA10 = 10-day rolling average of Close price
MA50 = 50-day rolling average of Close price
Position = 1 (long)  if MA10 > MA50
Position = 0 (flat)  if MA10 <= MA50
Daily earn = (Close_t - Close_t-1) × Position
This is a trend-following approach: the idea is to capture sustained uptrends (when the fast MA crosses above the slow MA) and avoid drawdowns during downtrends (by sitting out when MA10 < MA50).

## 📈 Methodology

1. **Period**: 2015-01-01 to 2017-12-31, Microsoft daily closing prices
2. **Signal generation**: Compute MA10 and MA50, derive binary position (1/0)
3. **P&L computation**:
   - Daily earnings = price change × position
   - Split into `profit` (positive earnings) and `losses` (negative earnings)
   - Cumulative wealth = running sum of daily earnings
4. **Drawdown analysis**:
   - Track running peak of cumulative wealth
   - Drawdown = current wealth − running peak
   - Maximum drawdown = worst peak-to-trough decline
5. **Risk-adjusted performance**:
   - Log-returns of the strategy
   - Sharpe ratio = (mean return − risk-free rate) / volatility × √(annualization factor)
   - Risk-free rate benchmarked against ECB rate (3%)
6. **Benchmark comparison**:
   - Buy-and-hold cumulative return (using `Close.diff().cumsum()`) as market benchmark
   - Strategy wealth plotted against benchmark wealth

## 📊 Key Results

| Metric | Value |
|---|---|
| Final wealth (strategy) | **+32.27 €** |
| Maximum drawdown | **-7.69 € (-23.8%)** |
| Sharpe ratio | **1.49** |
| Market (log-return) volatility | **0.01** |

### 🔍 Interpretation

- **Sharpe ratio = 1.49 > 1** → the strategy delivers a solid risk-adjusted return relative to the ECB risk-free rate, generally considered a "good" Sharpe ratio.
- **Strategy vs. Market**: the buy-and-hold benchmark *outperforms* the MA crossover strategy in absolute terms (~39€ vs ~32€ final wealth) over this strongly trending bull period — expected, since trend-following strategies tend to lag a pure uptrend due to entry/exit lag.
- **Drawdown resilience**: despite lower absolute returns, the strategy shows **better downside protection** — the MA crossover exits positions during downturns (e.g., visible flat periods in the wealth curve around mid-2015 and mid-2016), reducing exposure to the worst drawdowns compared to the market.

**Takeaway**: in a strongly trending bull market, a simple MA crossover underperforms buy-and-hold in raw returns, but offers a smoother equity curve and reduced drawdown — a classic trade-off between *return* and *risk control* in trend-following strategies.

## 🛠️ Technologies

- Python 3
- pandas, numpy
- matplotlib (dark theme visualization)

## 📁 Project Structure
├── ma_crossover_strategy.py   # Main strategy & backtest script
├── microsoft.csv               # MSFT historical price data
└── README.md

## 📚 Concepts Covered

- Trend-following / moving average crossover signals
- Vectorized backtesting with pandas
- Cumulative P&L and drawdown analysis
- Sharpe ratio with a risk-free benchmark (ECB rate)
- Strategy vs. benchmark performance comparison
- Log-returns and volatility

## ⚠️ Limitations & Possible Extensions

- **Look-ahead bias check**: ensure no future data leaks into signal generation (current implementation uses past rolling windows, which is correct)
- **Transaction costs**: not modeled — adding realistic fees/slippage would likely reduce strategy returns further
- **Parameter sensitivity**: MA10/MA50 are arbitrary; a parameter sweep (e.g., grid search on MA windows) could optimize the strategy
- **Out-of-sample testing**: test on a different period/asset to check robustness (overfitting risk with only one bull-market period)
- **Position sizing**: currently binary (0/1 share) — could extend to variable position sizing based on signal strength or volatility targeting
- **Annualization factor**: double-check `252*3` in the Sharpe ratio — should typically be `√252` for daily data over 1 year, or `√(252×N years)` if returns span multiple years and you're not annualizing per-year

## 👤 Author

ABDEL MOUSSA, aspiring Quantitative Analyst

---

*This project was developed as part of a personal initiative to explore systematic trading strategies and backtesting methodology.*

Description GitHub (champ "About")
Backtest of a MA10/MA50 crossover strategy on Microsoft stock (2015-2017): P&L, drawdown, and Sharpe ratio analysis vs. buy-and-hold benchmark.

Topics/Tags GitHub
quantitative-finance, trading-strategy, backtesting, moving-average, sharpe-ratio, drawdown, python, pandas, algorithmic-trading, technical-analysis
