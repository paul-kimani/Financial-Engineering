# 03 — Quant Developer Projects

> Concrete builds that develop and demonstrate QD skills. Each project has a clear deliverable, a skill target, and a difficulty rating. Start with P1, stack upward.

**Difficulty:** ★ = beginner · ★★ = intermediate · ★★★ = advanced · ★★★★ = hard

---

## P1 — Event-driven backtesting engine ★★
**What it is:** A clean, extensible backtesting framework built from scratch — not using Backtrader or Zipline, but your own event loop. Events flow through a queue: `MarketEvent → SignalEvent → OrderEvent → FillEvent`. Strategies subscribe to market events and emit signals; a portfolio module converts signals to orders; a simulated broker fills them.

**Why it matters:** Every quant firm builds their own backtest engine (because off-the-shelf ones all have subtle biases). Building one from scratch forces you to confront look-ahead bias, transaction cost modelling, and event ordering at a fundamental level. It is also the most common "show me a project" ask in QD interviews.

**Deliverable:** Python package on GitHub with: data loader (Yahoo Finance or local CSVs), at least two strategies (a simple MA crossover and one signal from your existing bots), portfolio accounting (cash, positions, P&L), performance metrics (Sharpe, max drawdown, CAGR), and a pytest test suite with ≥80% coverage.

**Skill targets:** Python architecture, event-driven systems, financial data handling, testing.

**Reference:** *Quantitative Trading* (Chan) Ch. 3–5; *Advances in Financial ML* (de Prado) on backtest methodology.

---

## P2 — Market data pipeline ★★
**What it is:** A pipeline that downloads OHLCV data for a universe of instruments, normalises it, stores it in a local database (PostgreSQL + TimescaleDB is the right tool), and exposes a clean Python API (`get_bars(symbol, start, end, freq)`).

**Why it matters:** Bad data breaks everything downstream. Building a clean data layer — handling splits, dividends, timezone alignment, missing bars — is a foundational QD skill. It also gives you the data infrastructure that P1 and all future projects depend on.

**Deliverable:** A Python library + Docker container that runs the pipeline. Schema design documented in a README. A small test suite that validates data quality (no forward gaps, no duplicated rows, correct adjusted prices).

**Skill targets:** SQL / TimescaleDB, Docker, data engineering, data quality thinking.

---

## P3 — Options pricing library ★★★
**What it is:** A Python library implementing Black-Scholes (analytical), a binomial tree (Cox-Ross-Rubinstein), and Monte Carlo pricing for European and American options — including Greeks (delta, gamma, vega, theta, rho) for each.

**Why it matters:** Pricing libraries are what many QDs at banks and hedge funds maintain. It also forces you to confront the gap between the clean academic formula and the messy reality of numerical implementation (handling deep ITM/OTM, convergence in the binomial tree, variance reduction in Monte Carlo).

**Deliverable:** Python library with full documentation, a Jupyter notebook demonstrating pricing + Greeks, and a performance benchmark comparing the three methods' speed and accuracy against known test cases.

**Extensions:** Add a local volatility surface (Dupire's equation), implied vol solver using Newton-Raphson, or a simple SABR model.

**Skill targets:** Numerical methods, financial maths implementation, performance profiling, documentation.

**Reference:** *Options, Futures, and Other Derivatives* (Hull); [[../../../Derivatives/README\|Derivatives vault notes]]; *Numerical Recipes* for numerical methods.

---

## P4 — Order book simulator ★★★
**What it is:** A limit order book (LOB) simulator in Python (then re-implement the hot path in C++ as a stretch). The LOB maintains bid/ask sides as sorted price levels, processes market orders, limit orders, and cancels, and produces a real-time midprice + spread.

**Why it matters:** Order book implementation is a canonical HFT interview problem — understanding the data structure (price-time priority, the right container for each operation), handling edge cases (locked markets, crossed quotes), and thinking about latency at the algorithmic level. The C++ rewrite is the real stretch goal.

**Deliverable:** Python implementation with full test coverage. Benchmark showing throughput (events/second). Optional: C++ rewrite of the matching engine, benchmarked against the Python version.

**Skill targets:** Data structures (heaps, sorted containers), algorithmic complexity, C++ intro, benchmarking.

---

## P5 — Competitive programming track ★★★★
**What it is:** Not one project but a sustained track — 100+ LeetCode problems solved, across arrays, strings, trees, graphs, dynamic programming, and system design. IMC Prosperity or a Jane Street mock competition when available.

**Why it matters:** HFT and top quant fund QD interviews are essentially competitive programming contests. There is no substitute for volume here. 100 Medium/Hard problems is the minimum; 200+ is where you stop worrying about it.

**Deliverable:** LeetCode profile showing consistent progress. Solutions repository on GitHub (private is fine — just track your own progress). Participation in at least one trading competition (IMC Prosperity runs annually and is free).

**Skill targets:** Algorithm fluency, interview readiness.

**Resources:** LeetCode (neetcode.io roadmap is a good ordering); *Cracking the Coding Interview* (McDowell) for interview format; *A Practical Guide to Quantitative Finance Interviews* (Xinfeng Zhou) for the probability/maths puzzles.

---

## P6 — Python backtesting engine → C++ rewrite ★★★★
**What it is:** Take the event-driven engine from P1, profile it to find the bottleneck (almost certainly the matching/fill simulation loop), and rewrite just that component in C++ with a Python wrapper (pybind11 or ctypes).

**Why it matters:** Shows you can work across the Python/C++ boundary — exactly what a quant dev does in production. Also teaches you how to measure before optimising, which is the first lesson of performance engineering.

**Deliverable:** The same engine from P1 but with a C++ hot path, with benchmarks showing the speedup. Write a short README explaining the profiling methodology and why you chose that component.

**Skill targets:** C++ (real code, not exercises), Python/C++ interop, profiling, performance engineering.

---

## P7 — Strategy platform API ★★★
**What it is:** Build a minimal version of the infrastructure a researcher would use to run strategies: a `Strategy` base class, a configuration system (YAML configs for strategy parameters), a results database (SQLite or Postgres), a comparison dashboard (a simple Streamlit or FastAPI app showing multiple strategies' performance side by side).

**Why it matters:** This is the QD contribution to research infrastructure — you're not writing the strategies, you're writing the platform that lets others write and run strategies quickly. Thinking from the "platform team" perspective rather than the "strategy" perspective is a QD mindset shift.

**Deliverable:** A Python framework with a documented API, at least three example strategies plugged in, and the dashboard.

**Skill targets:** Software architecture, API design, web basics (FastAPI / Streamlit), documentation.

---

## Suggested order

```
Start now (Year 3, Sem 2):    P1 (backtesting engine) + P2 (data pipeline) in parallel
                               P5 (LeetCode) as a continuous background track — 3 problems/week minimum

Next semester:                P3 (options library) — pairs with Derivatives and Financial Calculus coursework
                               P4 (order book) — pairs with market microstructure reading

Final year / pre-graduation:  P6 (C++ rewrite of P1) — when C++ is solid enough
                               P7 (strategy platform) — when P1 + P2 are mature

Always running:               P5 (competitive programming)
```
