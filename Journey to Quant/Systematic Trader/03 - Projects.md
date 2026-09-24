# 03 — Systematic Trader Projects

> Concrete builds for developing a systematic trading practice — strategy, infrastructure, and risk management. These extend your existing cTrader work rather than replacing it.

**Difficulty:** ★ = beginner · ★★ = intermediate · ★★★ = advanced · ★★★★ = hard

---

## S1 — Clean market data pipeline ★★

**What it is:** A Python pipeline that downloads, validates, and stores OHLCV data for your trading instruments — XAUUSD, GBPJPY, and at least one equity index (S&P 500, DAX). Store in a local database (TimescaleDB in Docker is the right choice — it's PostgreSQL with time-series optimisations) and expose a clean `get_bars(symbol, start, end, freq)` API.

**Why it matters:** Everything downstream — backtesting, research, live monitoring — depends on clean data. Running your current bots on Yahoo Finance data or cTrader's built-in historical feed is fine for development; building your own data layer is what gives you control, reproducibility, and speed.

**Key quality checks to build in:** no forward gaps, no duplicate rows, adjusted prices where applicable, timezone consistency, flagged missing bars.

**Deliverable:** A Python library + Docker container running the pipeline. Schema documented in README. A validation script that checks data quality and reports anomalies.

**Skill targets:** SQL / TimescaleDB, Docker, data quality discipline.

---

## S2 — Strategy tearsheet generator and live monitor ★★

**What it is:** A Python tool that produces a standardised performance tearsheet for any strategy — daily P&L, cumulative returns chart, rolling Sharpe (63-day window), monthly return heatmap, max drawdown, drawdown duration, and per-trade metrics (win rate, average winner/loser, profit factor). Builds on the data pipeline from S1.

The live component: a simple dashboard (Streamlit works well) that shows each running strategy's current-day P&L, drawdown from peak, and a flag if drawdown breach thresholds are hit.

**Why it matters:** A tearsheet is how a PM evaluates a strategy — you need to be able to produce one and defend every number in it. The live monitor is the most basic risk management tool: you cannot manage risk you're not watching.

**Deliverable:** Python library producing tearsheets (PDF or HTML output), plus a Streamlit dashboard showing live strategy status.

**Skill targets:** Performance attribution, visualisation, risk monitoring.

**Reference:** Quantopian's empyrical library (open source) shows the standard tearsheet format, even though Quantopian no longer operates.

---

## S3 — Kelly criterion position sizing implementation ★★

**What it is:** A proper implementation of Kelly criterion sizing for your existing strategies, starting from the QUANTFRAME background note and building it out to a full implementation: full Kelly, fractional Kelly (half-Kelly recommended), Kelly for multiple simultaneous strategies using the correlated-Kelly formula, and a simulation showing the effect of parameter estimation error on optimal sizing.

**The key insight to implement:** The full Kelly formula for a single strategy is:

$$f^* = \frac{\mu - r_f}{\sigma^2}$$

where $\mu$ is the expected return, $r_f$ the risk-free rate, and $\sigma^2$ the variance of returns. For correlated strategies:

$$\mathbf{f}^* = \Sigma^{-1}(\boldsymbol{\mu} - r_f \mathbf{1})$$

The simulation component: generate 1,000 Monte Carlo paths with the estimated parameters, add ±20% estimation noise, and show what happens to portfolio outcomes at full Kelly vs. half-Kelly vs. quarter-Kelly.

**Why it matters:** This is how you size strategies in a principled way — not "2% risk per trade" from a rule-of-thumb, but a derivation from the growth-maximisation objective with explicit uncertainty accounting.

**Deliverable:** A Python module with the sizing functions, plus a Jupyter notebook with the Monte Carlo simulation and a section explaining why estimation error makes fractional Kelly the practical choice.

**Reference:** [[../../../QUANTFRAME/Concepts/BackGround to Kelly Criterion|Background to Kelly Criterion QUANTFRAME note]] — extend this into the full implementation.

---

## S4 — Retrofit a cTrader bot with a proper walk-forward backtest ★★★

**What it is:** Take your Gold Pulse cBot (or Squeeze Pulse Breakout) and run it through a statistically rigorous walk-forward test in Python — not the in-sample backtest that cTrader produces natively, but a true out-of-sample evaluation: split history into expanding windows, optimise parameters on each training window, evaluate on the next unseen window, repeat.

**The key additions over a naive backtest:**
- Walk-forward parameter selection (no future data in optimisation)
- Transaction costs: realistic spread + slippage + commission model
- Deflated Sharpe Ratio (adjust for the number of parameter combinations tested)
- A comparison: what does the strategy look like in-sample vs. out-of-sample? If the performance gap is large, you have a fitting problem.

**Why it matters:** This is the rigorous version of what you're already doing. Most retail backtests are overfitted in ways the trader doesn't realise. Running this process surfaces the real out-of-sample edge — which might be smaller than the backtest suggested, but is real.

**Deliverable:** A Python backtesting script + a report showing the walk-forward performance vs. in-sample performance, with a section on what the performance gap tells you about parameter stability.

**Skill targets:** Backtesting methodology, statistical rigour applied to your own strategies.

---

## S5 — Multi-strategy portfolio with correlation management ★★★

**What it is:** Run at least three strategies simultaneously in a paper trading account (or in a backtest), managed as a portfolio: individual Kelly sizing for each, then adjusted using the correlated-Kelly formula to account for correlation between strategies. Track portfolio-level Sharpe, drawdown, and how correlated the strategies are in practice.

**The research question:** Do your current three strategies (Gold Pulse, Squeeze Pulse Breakout, Trend Pullback Bot) diversify each other, or do they tend to draw down together (because they're all in some sense trend-following)? The correlation matrix of daily P&L tells you.

**The upgrade:** If the strategies are too correlated, identify what would be uncorrelated — a mean-reversion strategy alongside your trend-following ones, or a different asset class.

**Deliverable:** A Python portfolio simulator running all three strategies, producing: strategy-level and portfolio-level tearsheets, correlation matrix of daily P&L, and a sizing recommendation using correlated Kelly.

**Skill targets:** Portfolio risk management, multi-strategy thinking, correlation awareness.

---

## S6 — Pairs trading / cointegration study ★★★

**What it is:** Implement a simple pairs trading strategy on a cointegrated pair (e.g., Brent crude vs. WTI crude, gold vs. silver, or two correlated equity index ETFs). Steps: test for cointegration (Engle-Granger, Johansen), estimate the hedge ratio, build the spread series, apply a mean-reversion signal (Bollinger Bands or z-score threshold), backtest with walk-forward methodology.

**Why it matters:** Pairs trading is the classical stat-arb strategy — it teaches cointegration, the difference between correlation and cointegration, and what a stationary spread actually means for a trading strategy. It's also a common interview topic for ST roles.

**Deliverable:** A Jupyter notebook with full pipeline, including a section on what happens when the cointegration breaks down (it does — pairs that were cointegrated stop being so). What would you do in production when the spread stops mean-reverting?

**Reference:** *Algorithmic Trading* (Chan) Ch. 3; *Analysis of Financial Time Series* (Tsay) Ch. 8.

---

## S7 — Competition entry: IMC Prosperity ★★★★

**What it is:** IMC Prosperity (runs annually, usually Jan–May) is a trading simulation competition where teams write Python algorithms to trade a fictional market. It tests signal construction, execution, risk management, and fast iteration — the full systematic trading skill stack in a compressed environment.

**Why it matters:** A top finish is a strong signal on a CV for both ST and QD paths. The competition requires fast prototyping, quick statistical thinking, and actually watching your P&L move — which is closer to the real job than any project you can build alone.

**Deliverable:** Competition entry + a debrief document: what strategies you ran, what worked, what didn't, what you'd do differently.

**Timing:** Watch for the IMC Prosperity announcement (typically opens Jan/Feb). Register as a team.

---

## Suggested order

```
Now (Year 3 Sem 2):    S1 (data pipeline) — foundational
                        S2 (tearsheet + live monitor) — immediate utility for your current bots
                        S3 (Kelly sizing) — pairs with QUANTFRAME Kelly note

Next semester:          S4 (walk-forward backtest of Gold Pulse) — rigorous evaluation
                        S5 (multi-strategy portfolio) — portfolio-level thinking

Pre-graduation:         S6 (pairs trading) — broadens beyond trend-following
                        S7 (IMC Prosperity if timing works) — competitive proof point

Always:                 Paper trading live in cTrader with S2 monitoring running
```
