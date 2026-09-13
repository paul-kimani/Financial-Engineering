# 06 — Systematic Trader Reading List

> A complete, tiered book curriculum for the ST path — from mathematical foundations through strategy development, risk management, and market microstructure. Weighted toward practical application: these books are here because they make you a better trader or a more rigorous researcher of your own strategies.

This list is the ST-specific companion to the [[../../Quant Roadmap - Reading List|root reading list]]. The ST path needs the widest practical coverage — less pure mathematical depth than QR, less CS depth than QD, but genuine rigour in the areas that directly affect P&L: strategy methodology, risk, and markets.

**Levels:** [Beginner] · [Core] · [Advanced]

---

## Tier 1 — Mathematical Foundations

The ST needs working quantitative fluency — enough to implement and understand every model in this list, derive the Kelly formula from first principles, and think clearly about probability. Not research-level depth, but not just "I know the formulas" either.

| Book | Level | Why it's here for a ST |
|---|---|---|
| *Mathematics for Machine Learning* — Deisenroth, Faisal, Ong | Core | The best single-volume bridge: linear algebra, calculus, and probability with ML applications throughout. Free PDF. The linear algebra chapter covers covariance matrices and PCA — both directly relevant to multi-strategy portfolio construction. |
| *Introduction to Probability* — Blitzstein & Hwang | Core | Probability is the native language of trading — expected values, conditional probability, distributions, the law of large numbers. This book builds intuition alongside rigour. The gambler's ruin and Markov chain chapters are directly relevant to drawdown analysis and regime modelling. |
| *Statistical Inference* — Casella & Berger | Core | Read at least Ch. 1–6: estimation, sufficiency, hypothesis testing. A trader running their own strategies needs to know whether a result is statistically significant, what the Sharpe ratio's sampling distribution looks like, and how to interpret a p-value correctly. The full book is graduate-level — the first six chapters are sufficient for the ST path. |
| *Convex Optimization* — Boyd & Vandenberghe | Advanced | The LP duality and QP machinery behind mean-variance portfolio optimisation and constrained position sizing. Focus on Ch. 1–5 and Ch. 11 (interior-point methods). Free PDF from authors. Read once you're doing multi-strategy portfolio work. |

---

## Tier 2 — Strategy Development

The core technical curriculum for a systematic trader. Read these in order — Chan I before Chan II, both before de Prado.

| Book | Level | Why it's here for a ST |
|---|---|---|
| *Quantitative Trading: How to Build Your Own Algorithmic Trading Business* — Ernest Chan | Core | **Start here.** The most practical introduction to systematic trading: Sharpe ratio interpretation, in-sample vs. out-of-sample, the dangers of overfitting, position sizing basics, transaction cost modelling, and the full lifecycle from idea to live. Written for someone building strategies, not studying them academically. Directly applicable to your existing cTrader bots. |
| *Algorithmic Trading: Winning Strategies and Their Rationale* — Ernest Chan | Core | Chan's follow-up — more strategy-specific: mean reversion (pairs trading, cointegration), momentum (cross-sectional and time-series), carry, and the statistical rationale behind each. The rationale sections are as important as the implementations — *why* does momentum work (or stop working)? |
| *Advances in Financial Machine Learning* — Marcos López de Prado | Advanced | The book on why naive backtesting lies. The must-read for any systematic trader who is serious about knowing whether their results are real: data leakage, multiple-testing problem, combinatorial purged cross-validation (CPCV), the deflated Sharpe ratio, feature importance. Read this only after you've built and run at least one serious backtest — the problems it describes will immediately resonate. |
| *Evidence-Based Technical Analysis* — David Aronson | Core | A rigorous statistical treatment of technical analysis — testing chart patterns and indicators properly, correcting for data mining bias, the null hypothesis framework for evaluating TA signals. Directly relevant to your indicator-based cTrader strategies: how do you know Gold Pulse's signals are real and not pattern-fitting? This book answers that question with statistics, not hand-waving. |
| *Building Winning Algorithmic Trading Systems* — Kevin Davey | Beginner–Core | Step-by-step process for building, testing, and optimising systematic strategies — including walk-forward optimisation, Monte Carlo robustness testing, and position sizing. More accessible than de Prado but covers the right methodology. Good companion to Chan if you want a practitioner process guide. |

---

## Tier 3 — Risk Management and Position Sizing

A strategy without a risk framework is just a bet. This tier is where systematic trading becomes a business rather than a hobby.

| Book | Level | Why it's here for a ST |
|---|---|---|
| *Fortune's Formula* — William Poundstone | Beginner | The story of the Kelly criterion — Claude Shannon, Ed Thorp, and the mathematics of optimal betting. Not a technical text but genuinely important context: the Kelly formula is derived from an information-theoretic argument, and understanding *why* it maximises geometric growth rate makes it a tool rather than a rule of thumb. Read before implementing S3 (Kelly sizing project). |
| *The Kelly Capital Growth Investment Criterion* — MacLean, Thorp, Ziemba (eds.) | Advanced | The technical companion to Fortune's Formula — the original papers on Kelly betting, the fractional Kelly debate, multi-asset Kelly (the correlated-portfolio formula your S3 project implements), and empirical results from Kelly-managed portfolios. A reference, not a cover-to-cover read. Thorp's own papers in here are essential. Cross-links to [[../../../QUANTFRAME/Concepts/BackGround to Kelly Criterion\|Kelly Criterion QUANTFRAME note]]. |
| *Quantitative Risk Management: Concepts, Techniques and Tools* — McNeil, Frey, Embrechts | Advanced | VaR, CVaR, extreme value theory (financial tail events are not Gaussian), copulas for modelling correlation under stress. The rigorous version of the risk management that every serious trader needs to understand even if they don't implement it all themselves. Focus on Ch. 1–5 (basics), Ch. 7 (EVT), and Ch. 9 (copulas). |
| *Dynamic Hedging: Managing Vanilla and Exotic Options* — Nassim Taleb | Advanced | Taleb before Black Swan — a trader's manual for hedging options positions in practice, including gamma scalping, vega hedging, and the distinction between model risk and market risk. More relevant if your strategies include options, but the risk mindset throughout is valuable for any systematic trader. |

---

## Tier 4 — Time Series and Signal Analysis

Understanding the statistical properties of financial time series — the gap between what standard ML assumes and what markets actually produce.

| Book | Level | Why it's here for a ST |
|---|---|---|
| *Analysis of Financial Time Series* — Ruey Tsay | Core | ARMA, GARCH, cointegration, unit roots, seasonality — the toolkit for working with financial data correctly. The GARCH chapter explains why volatility clusters and how to model it, which directly informs position sizing (size down in high-vol regimes). The cointegration chapter is the technical basis for the pairs trading project (S6). |
| *Time Series Analysis* — James Hamilton | Advanced | The graduate-econometrics standard — VAR models, the Kalman filter, spectral analysis, unit root tests. Go here for the theoretical grounding once Tsay is comfortable. Most relevant for regime detection work and for building more sophisticated signal models. |
| *Trading Systems and Methods* — Perry Kaufman | Core | An encyclopedia of technical trading systems — moving averages, oscillators, volatility breakout, pattern recognition, arbitrage — with statistical analysis of each. More comprehensive than any single strategy book; use as a reference when evaluating a new signal idea. Updated regularly (6th edition). |

---

## Tier 5 — Market Microstructure and Execution

Understanding the environment your strategies operate in — how prices are formed, how orders move markets, and how to minimise the gap between backtest and live performance.

| Book | Level | Why it's here for a ST |
|---|---|---|
| *Trading and Exchanges: Market Microstructure for Practitioners* — Larry Harris | Core | The definitive guide to how markets actually work: order books, market makers, adverse selection, spread decomposition, informed vs. uninformed order flow, fragmentation. A systematic trader who doesn't understand why their limit orders get picked off in volatile markets, or why their fills are worse in thin sessions, is ignoring this. Required reading for anyone trading live. |
| *Algorithmic and High-Frequency Trading* — Cartea, Jaimungal, Peñalva | Advanced | The academic treatment of optimal execution and HFT — Almgren-Chriss model, optimal liquidation, market making strategies, adverse selection in limit order books. More relevant if you're moving toward high-frequency or if execution cost is eating a meaningful fraction of your edge. |
| *Market Microstructure Theory* — Maureen O'Hara | Advanced | The theoretical foundations — why spreads exist (the adverse selection / inventory cost decomposition), what price discovery means, how information asymmetry shapes market structure. More academic than Harris; useful for understanding the "why" behind the "what." |

---

## Tier 6 — Core Financial Theory

The financial knowledge layer. A systematic trader needs this for vocabulary, for instrument mechanics, and for the moments when macro context drives your strategy's P&L in ways you don't expect.

| Book | Level | Why it's here for a ST |
|---|---|---|
| *Investments* — Bodie, Kane, Marcus | Beginner–Core | The standard investments textbook — portfolio theory, CAPM, market efficiency, equity, fixed income, derivatives, and asset allocation. Good single-volume survey for building or filling gaps in the financial theory background. Most relevant chapters: Part IV (fixed income), Part V (derivatives), Part VII (active portfolio management). |
| *Options, Futures, and Other Derivatives* — John Hull | Core | Even if you don't trade derivatives directly, understanding how they're priced gives you intuition for what drives implied volatility, what carry means in FX (interest rate differentials), and how futures pricing relates to spot. For a gold trader specifically: Hull's commodity futures chapters explain the convenience yield and the gold lease rate, which drive XAUUSD's term structure. |
| *Active Portfolio Management* — Grinold & Kahn | Advanced | The portfolio construction layer above individual strategy development — information ratio, transfer coefficient, alpha decay, risk budgeting. The framework for managing multiple strategies as a portfolio rather than as independent bets. Read once you're managing more than two simultaneous strategies. |

---

## Tier 7 — Mental Game and Market Intuition

The psychological and intuitive layer of trading. Not soft skills — these books contain real insights about how markets work and how traders fail even with good systems.

| Book | Level | Why it's here for a ST |
|---|---|---|
| *Market Wizards* — Jack Schwager | Beginner | Interviews with the best traders of their generation — Seykota, Marcus, Jones, Kovner. Every interview has a lesson about the mental side of running a strategy through drawdowns, about when to increase size vs. when to stop, and about the difference between knowing a strategy is good and *acting as if* it's good. Read this before running live capital. |
| *The New Market Wizards* — Jack Schwager | Beginner | The follow-up volume — more interviews, more mental game, more regime awareness. Feeder into the first volume. |
| *Reminiscences of a Stock Operator* — Edwin Lefevre | Any level | Fictional memoir of Jesse Livermore — the classic traders' text on patience, position sizing, the psychology of being right but early, and the distinction between a market opinion and a trading position. Written 1923, still read on every trading desk. |
| *The Alchemy of Finance* — George Soros | Advanced | Soros's "reflexivity" theory — markets are not simply discounting machines, they actively shape the fundamentals they're supposed to be reflecting. Not a trading manual but a thinking framework for macro regimes. Challenging and occasionally obscure, but the core idea is genuinely important for trend traders. |
| *Trading in the Zone* — Mark Douglas | Beginner | The mental discipline text — probabilistic thinking about individual trades, the distinction between "this trade will work" (wrong frame) and "this edge plays out over many trades" (right frame). Short and direct. Relevant for the discipline of staying in a strategy through drawdowns. |

---

## Tier 8 — Programming for Trading

A systematic trader needs Python at a professional level and enough platform-specific knowledge (your cTrader/C#) to implement cleanly.

| Book | Level | Why it's here for a ST |
|---|---|---|
| *Python for Data Analysis* — Wes McKinney | Beginner–Core | Pandas, time series operations, resampling, rolling windows — the toolkit for signal research and backtest analysis. Sections to focus on: time series (Ch. 11), groupby (Ch. 10), and HDF5/I/O for data storage. |
| *Fluent Python* — Luciano Ramalho | Core | Production-quality Python — important when your research scripts become the foundation of live trading infrastructure. Generators, async basics, and the data model chapters are the most relevant for trading systems. |
| *Python for Finance* — Yves Hilpisch | Beginner–Core | Finance-specific Python — data sourcing, financial time series, derivatives valuation, real-world trading system components. More practically focused than the research-stack books. Good companion to McKinney for finance-specific patterns. |

---

## Tier 9 — Interview Preparation

| Book | Level | Why it's here for a ST |
|---|---|---|
| *A Practical Guide to Quantitative Finance Interviews* — Xinfeng Zhou | Core | The Green Book. Probability puzzles, brain teasers, and the mental maths problems that prop trading firm interviews run. Work through systematically — the probability and expected-value sections are the most heavily tested in ST interviews. |
| *Heard on the Street* — Timothy Crack | Core | Companion to the Green Book — statistics, distributions, hypothesis testing, and derivative basics. More emphasis on markets questions alongside the maths. |
| *How to Make Friends and Influence People* — Dale Carnegie | Any level | Not a joke entry. SIG, Optiver, and DRW explicitly test communication and persuasion skills in their later-stage interviews — the cultural fit round rewards people who can explain their thinking to a non-technical audience. The most practical book on that skill. |

---

## Tier 10 — Context and Career

| Book | Level | Why it's here for a ST |
|---|---|---|
| *My Life as a Quant* — Emanuel Derman | Any level | The quant's side of finance — a model-builder's memoir that gives context for where systematic trading fits in the broader financial ecosystem. |
| *The Quants* — Scott Patterson | Any level | The rise and near-fall of quantitative trading — the crisis of 2007–08 and why correlated strategies that work independently can catastrophically fail together. A caution about what happens when multiple systematic traders are on the same side of the same trade. Directly relevant to the correlation management project (S5). |
| *More Money Than God* — Sebastian Mallaby | Any level | The history of hedge funds — from Alfred Jones through Simons. Shows how systematic trading evolved from discretionary, and what the great funds were doing differently. |

---

## Suggested sequencing

```
Now (Year 3, Sem 2):
  Quantitative Trading (Chan I)         (Tier 2 — start here, read this month)
  Introduction to Probability           (Tier 1 — concurrent)
  Fortune's Formula (Poundstone)        (Tier 3 — short, motivating, pairs with S3 project)
  Market Wizards (Schwager)             (Tier 7 — one interview per week, alongside technical reading)
  Green Book (Zhou) — 3 problems/week   (Tier 9 — interview prep starts now)

Next semester:
  Algorithmic Trading (Chan II)         (Tier 2 — strategy types and rationale)
  Analysis of Financial Time Series (Tsay) Ch. 1–5   (Tier 4 — ARMA, GARCH)
  Advances in Financial ML (de Prado)   (Tier 2 — methodology, after first real backtest)
  Trading and Exchanges (Harris)        (Tier 5 — microstructure, pairs with S4)

Pre-graduation:
  Evidence-Based Technical Analysis (Aronson)  (Tier 2 — statistical validity of your signals)
  Kelly Capital Growth (MacLean et al., selected papers)  (Tier 3 — advanced sizing)
  Active Portfolio Management (Grinold & Kahn) Ch. 1–8  (Tier 6 — portfolio-level thinking)
  Heard on the Street                   (Tier 9 — interview prep deepening)

Post-graduation / first role:
  Quantitative Risk Management (McNeil et al.) (Tier 3 — tail risk, the serious version)
  Hamilton (Time Series Analysis)       (Tier 4 — deep time series theory)
  Cartea et al. (execution/HFT)         (Tier 5 — if moving toward execution-focused work)
  Grinold & Kahn cover-to-cover         (Tier 6 — portfolio management at scale)
```
