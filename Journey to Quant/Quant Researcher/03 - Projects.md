# 03 — Quant Researcher Projects

> Concrete research projects that develop the habits of mind, statistical rigour, and output format of a real quant researcher. Each one produces something you can discuss in depth in an interview.

**Difficulty:** ★ = beginner · ★★ = intermediate · ★★★ = advanced · ★★★★ = hard

---

## R1 — Factor research notebook ★★

**What it is:** Build, test, and decay-analyse a single equity or futures factor from end to end. Pick one well-known factor (price momentum, earnings momentum, value/book-to-price, quality/ROE) and implement it from raw data through to a clean performance tearsheet.

**The full pipeline:**
1. Download data (Yahoo Finance, WRDS if accessible, or Stooq)
2. Compute the factor signal cleanly (no look-ahead — only data available as of each rebalance date)
3. Rank stocks/instruments into quintiles by signal
4. Compute long-top-quintile / short-bottom-quintile returns at each rebalance
5. Calculate Sharpe, max drawdown, turnover, and IC (information coefficient: correlation of signal with next-period return)
6. Decay analysis: plot IC as a function of forward horizon (1d, 5d, 21d, 63d) to see when the signal stops working
7. Controlled for transaction costs: is the edge real net of realistic execution costs?

**Why it matters:** This is exactly what a junior researcher does on their first rotation. It trains the most important habit: computing the *right* number honestly, not the number that looks good. The IC calculation and decay plot tell you whether you have something real.

**Deliverable:** A Jupyter notebook with all code, clear methodology documentation, and a one-page written summary of findings — including a section on "what could make this result spurious." The written summary is as important as the code.

**Skill targets:** Data pipeline, factor construction, portfolio analysis, writing.

**Reference:** *Active Portfolio Management* (Grinold & Kahn) Ch. 6–7; *Advances in Financial ML* (de Prado) Ch. 5.

---

## R2 — Walk-forward backtesting with proper methodology ★★★

**What it is:** Take one of your existing cTrader bots (or build a new ML signal from scratch) and test it using a statistically correct walk-forward framework — not a simple in-sample / out-of-sample split, but a genuine combinatorial purged cross-validation (CPCV) setup as de Prado describes.

**The key constraints:**
- No look-ahead in feature construction
- Purging embargo between train and test folds (no adjacent data leaking)
- Walk-forward parameter selection (parameters chosen on training data only, never on test)
- Benchmark: compare your strategy to a simple trend-following or buy-and-hold baseline
- Deflated Sharpe Ratio: adjust the reported Sharpe for the number of strategy variants tested (de Prado's DSR formula)

**Why it matters:** Most retail backtests are wrong. This project forces you to confront exactly how wrong by building the right methodology and watching the performance shrink from what a naive backtest showed. The discipline of implementing this correctly *is the job* of a quant researcher.

**Deliverable:** A Python framework implementing CPCV for your strategy, a performance report showing the difference between naive and methodologically correct results, and a written explanation of each correction and why it matters.

**Reference:** *Advances in Financial ML* (de Prado) Ch. 7–12; [[../../../Applied Analytics For Finance/07 Model Assessment, Bias-Variance and Resampling\|ch.7 vault notes]] for the cross-validation background.

---

## R3 — Replicate a published paper ★★★

**What it is:** Pick one empirical paper in quantitative finance and replicate its core result from scratch using publicly available data. Good starting choices:

- *"Momentum"* (Jegadeesh & Titman, 1993) — the seminal momentum paper, data available from CRSP / Stooq
- *"Value and Momentum Everywhere"* (Asness, Moskowitz & Pedersen, 2013) — AQR's multi-asset factor paper
- *"Carry"* (Koijen et al., 2018) — carry factor across asset classes
- *"The Cross-Section of Expected Stock Returns"* (Fama & French, 1992) — the 3-factor model

**The task:** Read the paper carefully, identify the exact methodology, implement it from raw data, and check whether your numbers are close to the paper's reported numbers. Write a short report: what matches, what doesn't, and why (data differences, methodology interpretation, time period).

**Why it matters:** Replication is how researchers verify they can read a paper critically, implement it correctly, and identify where results are fragile. It's also a common interview discussion — "walk me through a paper you've replicated."

**Deliverable:** Code + a short research note (2–3 pages) documenting what you did, what matched, what didn't, and what you'd do differently.

---

## R4 — Regime detection study ★★★

**What it is:** Build and compare multiple approaches to detecting market regimes in XAUUSD (or another instrument you know well) — hidden Markov models, Gaussian mixture models, change-point detection, and a clustering approach (k-means on feature vectors of rolling statistics).

This directly extends the note you already have: [[../../../QUANTFRAME/Project Notes/Discovering Market Regimes Through Structure|Discovering Market Regimes Through Structure]].

**The research questions:**
1. Can you reliably identify regimes in-sample? (The easy question.)
2. Can you do it out-of-sample without future information? (The real question.)
3. If you condition a simple trend-following strategy on detected regime, does performance improve net of the regime-detection cost?

**Why it matters:** Regime detection is a live research topic. Most methods that look good in-sample degrade quickly out-of-sample — learning to document this rigorously rather than selectively reporting the good version is the core discipline.

**Deliverable:** A research report (not just a notebook — write it up with sections: methodology, data, results, limitations, conclusions) plus the code. The limitations section should be longer than the results section.

**Reference:** *Analysis of Financial Time Series* (Tsay) Ch. 3–5; the existing QUANTFRAME project note.

---

## R5 — Volatility forecasting comparison ★★★

**What it is:** A systematic comparison of volatility forecasting models on a single instrument: GARCH(1,1), EGARCH, realised vol (using intraday data), a HAR-RV model, and an ML model (gradient boosted trees or a simple LSTM). Evaluate all on the same test set using a proper scoring rule (QLIKE loss or MSE of log-vol).

**Why it matters:** Volatility forecasting is a core quant skill — it underlies options pricing, risk management, and position sizing. Building a rigorous comparative study, with a proper loss function and out-of-sample evaluation, demonstrates exactly the statistical discipline that researchers are hired for.

**Deliverable:** A research report comparing all models, with honest evaluation — the model that wins on in-sample should not automatically win out-of-sample. Include a section on why each model fails where it does.

**Reference:** *Analysis of Financial Time Series* (Tsay) Ch. 3–4; [[../../../Time Series/README\|Time Series vault notes]].

---

## R6 — Write a research note on a topic from your coursework ★★

**What it is:** Pick one result from your Financial Engineering coursework — the Gauss-Markov theorem, the Black-Scholes derivation, the Kelly criterion, or the relationship between SVM duality and constrained portfolio optimisation — and write it up as a self-contained research note, as if writing for a junior colleague who knows basic probability but not this specific result.

**The format:** Introduction (why this result matters), setup (definitions and assumptions), derivation (full, with every step), discussion (what the assumptions do, where the result breaks, extensions), references.

**Why it matters:** The ability to write clearly is the most underrated skill in research. Writing a derivation forces you to find every gap in your own understanding. A portfolio of well-written notes is also a concrete thing to show in interviews and MSc applications.

**Deliverable:** A clean LaTeX-compiled PDF (or a polished Markdown document in the vault), following the same conventions as the Stochastics chapter notes.

**Suggested starting point:** The Kelly Criterion background note in QUANTFRAME: [[../../../QUANTFRAME/Concepts/BackGround to Kelly Criterion|Background to Kelly Criterion]] — extend it from background into a full research-note treatment.

---

## Suggested order

```
Now (Year 3 Sem 2):      R1 (factor research) — foundational, pairs with coursework
                          R6 (write-up a result) — runs in parallel, one per month

Next semester:            R2 (walk-forward methodology) — builds on your existing bots
                          R4 (regime detection) — extends existing QUANTFRAME work

Pre-graduation:           R3 (replicate a paper) — shows independent research capability
                          R5 (volatility forecasting) — advanced, great MSc application material
```
