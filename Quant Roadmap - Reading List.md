# Quant Roadmap — Reading List (End to End)

A complete, staged book curriculum from "beginner" through to a real quant skillset — math, programming, statistics/ML, and finance, in the order that actually builds on itself. Built to double as a mentoring syllabus: every book below is marked with the level it assumes, so you can hand a beginner the right starting point without dumping the whole list on them at once.

This is the companion piece to the career roadmap discussion — read as "what to study," where that conversation was "what to do." Cross-linked throughout to the course chapters already in this vault where the overlap is direct.

---

## How to use this

- **Tiers, not a strict queue.** Within a tier, books can be read in parallel or any order. Move to the next tier once you're comfortable, not once you've "finished" — these overlap on purpose.
- **[Beginner] / [Core] / [Advanced]** tags on every entry — use them to place a mentee correctly instead of guessing.
- **Skip what you've already got.** Your Financial Engineering coursework already covers a fair amount of Tier 2 and Tier 4 — the annotations flag exactly where a book restates vs. extends what's already in this vault.

---

## Tier 1 — Mathematical Foundations

The load-bearing math underneath everything else. A beginner without an engineering/math background should start here in full; you can likely skim-verify most of this and spend real time only on Convex Optimization and Mathematics for Machine Learning, which go past what a standard degree covers.

| Book | Level | Why it's here |
|---|---|---|
| *Linear Algebra Done Right* — Sheldon Axler | Core | Proof-based, conceptual linear algebra. Matrix transposes, inner products, and eigenstructure show up constantly — [[06 CAPM and Multifactor Models]]'s covariance decomposition ($\Sigma=\sigma_m^2\beta\beta^\top+\Psi$) *is* linear algebra wearing a finance costume. |
| *Introduction to Linear Algebra* — Gilbert Strang | Beginner alt. | More computational, less proof-heavy than Axler — better first pass if starting from near-zero. Strang's MIT OCW lectures pair with it and are excellent. |
| *Understanding Analysis* — Stephen Abbott | Advanced | Real analysis — limits, continuity, convergence, done rigorously. Not strictly required for quant dev, but the prerequisite mindset for stochastic calculus (Tier 5) and for reading academic quant papers without hand-waving. |
| *Introduction to Probability* — Blitzstein & Hwang | Core | The best modern probability text for this purpose — builds intuition (Bayes' theorem, distributions, expectation) alongside rigor. Directly underpins [[09 Logistic Regression and Classification]]'s Naive Bayes section and MLE more broadly. |
| *Statistical Inference* — Casella & Berger | Advanced | The standard graduate mathematical-statistics text — estimation theory, hypothesis testing, sufficiency, done properly. Go here once Blitzstein & Hwang feels easy and you want the "why" behind the F-test in [[05 Multiple Linear Regression and the F-test]] proven from first principles rather than asserted. |
| *Convex Optimization* — Boyd & Vandenberghe | Advanced | **The** optimization reference — LP duality (your [[03 - Duality of Linear Programming]] chapter is literally a special case of what this book covers), quadratic programs (portfolio optimization), and the Lagrangian machinery behind SVM's dual ([[10 Support Vector Machines]]). Free PDF from the authors — no excuse not to have it. |
| *Mathematics for Machine Learning* — Deisenroth, Faisal, Ong | Core | The one you asked about by name. Purpose-built bridge text: linear algebra, calculus, and probability, each explicitly angled toward *why ML needs them* — PCA, gradient descent, etc. Read this if you want the Tier 1 topics re-taught with the ML payoff visible the whole way, rather than as three separate abstract subjects. Free PDF available from the authors. Excellent for a mentee — it's the single most efficient book on this list per hour spent. |

---

## Tier 2 — Programming & Computational Foundations

Skills, not just theory — this is what turns Tier 1/3 knowledge into working code.

| Book | Level | Why it's here |
|---|---|---|
| *Python for Data Analysis* — Wes McKinney | Beginner | Written by pandas' creator. If a mentee knows zero Python, this is the on-ramp — gets them to a working data-analysis toolkit fast. |
| *Fluent Python* — Luciano Ramalho | Core | Once basic Python works, this teaches *idiomatic*, production-quality Python — the gap between "my script runs" and "my code would survive a code review." Matters most for the quant dev track. |
| *Introduction to Algorithms* (CLRS) — Cormen, Leiserson, Rivest, Stein | Advanced | The canonical algorithms/data-structures text. Not finance-specific, but a real requirement if aiming at quant dev roles that also run technical interviews (many do) — complexity analysis, sorting, graphs, dynamic programming. |

---

## Tier 3 — Statistical Learning & Machine Learning

This is where your current coursework already lives — these books deepen [[Applied Analytics For Finance/README|the Applied Analytics unit]] rather than replace it.

| Book | Level | Why it's here |
|---|---|---|
| *An Introduction to Statistical Learning* (ISLR) — James, Witten, Hastie, Tibshirani | Core | Read this **first** in this tier — it maps almost one-to-one onto [[07 Model Assessment, Bias-Variance and Resampling]], [[08 Regularization - Ridge and Lasso]], and [[09 Logistic Regression and Classification]], uses R like your course does, and will make the existing chapters click harder instead of feeling repeated. Free PDF from the authors. |
| *The Elements of Statistical Learning* (ESL) — Hastie, Tibshirani, Friedman | Advanced | ISLR's material at full mathematical depth — same authors, same structure, no hand-holding. Go here once ISLR feels easy, not before. Also free from the authors. |
| *Pattern Recognition and Machine Learning* — Christopher Bishop | Advanced | The Bayesian-flavored classic — probabilistic graphical models, more rigorous treatment of classification/clustering than ISLR/ESL. Common on quant *researcher* reading lists specifically (more than quant dev ones). |
| *Deep Learning* — Goodfellow, Bengio, Courville | Advanced | The standard deep-learning reference — feeds directly into [[Applied Analytics For Finance/11 Neural Networks (stub)|ch.11's Neural Networks stub]] once that lecture lands, and into your stated interest in interpretability/scaling. Free HTML version online. Don't start here — Tier 1's Mathematics for Machine Learning is the correct on-ramp first. |

---

## Tier 4 — Core Financial Theory

The finance-specific knowledge layer — much of this already exists elsewhere in your vault ([[Financial Theory]], [[Fixed Income Securities]], [[Derivatives]]); these are the canonical textbook versions to check your course notes against.

| Book | Level | Why it's here |
|---|---|---|
| *Investments* — Bodie, Kane, Marcus | Beginner–Core | The standard broad investments textbook — portfolio theory, CAPM, market efficiency, asset classes. Good single-book survey for a mentee who needs the full landscape before specializing. |
| *Options, Futures, and Other Derivatives* — John Hull | Core | The industry-standard derivatives reference. Even outside derivatives-specific roles, this is common vocabulary across the whole field — worth having read once cover to cover. |
| *Fixed Income Securities* — Bruce Tuckman & Angel Serrat | Advanced | The standard rates/fixed-income text if that specialization pulls you — yield curves, duration, term structure models, beyond what an intro course covers. |

---

## Tier 5 — Stochastic Calculus & Quantitative Pricing

The mathematical core of "quant finance" in the narrow, classic sense — required for derivatives-pricing-heavy roles, optional (but still valuable context) for quant dev/systematic trading.

| Book | Level | Why it's here |
|---|---|---|
| *Paul Wilmott Introduces Quantitative Finance* | Core | The gentlest on-ramp to stochastic calculus and option pricing — intuition-first, before the measure-theoretic version. Good starting point for a mentee who isn't ready for Shreve yet. |
| *Stochastic Calculus for Finance I: The Binomial Asset Pricing Model* — Steven Shreve | Core | Builds option pricing from discrete-time binomial trees first — genuinely approachable, and the standard first real stochastic-calculus text in quant finance master's programs. |
| *Stochastic Calculus for Finance II: Continuous-Time Models* — Steven Shreve | Advanced | The continuous-time follow-up — Brownian motion, Itô calculus, the Black-Scholes-Merton derivation done properly. This is the book most "quant" job postings implicitly assume you've worked through. |

---

## Tier 6 — Time Series & Financial Econometrics

The gap flagged earlier in the career roadmap — this is what your coursework's CV-on-financial-data warning ([[07 Model Assessment, Bias-Variance and Resampling#4.4 Why plain CV can fail on financial data]]) gestures at without building out.

| Book | Level | Why it's here |
|---|---|---|
| *Analysis of Financial Time Series* — Ruey Tsay | Core | Purpose-built for finance: autocorrelation, ARCH/GARCH volatility modeling, the actual toolkit for the "financial data violates SL1" problem your notes already flag. |
| *Time Series Analysis* — James Hamilton | Advanced | The graduate-econometrics-standard reference — more mathematically complete than Tsay, less finance-flavored. Go here if the theory itself, not just the application, is what you want. |

---

## Tier 7 — Systematic & Algorithmic Trading

The most directly applicable tier to your existing bots (Gold Pulse, Squeeze Pulse Breakout, Trend Pullback).

| Book | Level | Why it's here |
|---|---|---|
| *Quantitative Trading* — Ernest Chan | Beginner–Core | The practical "how do I actually build and evaluate a strategy" book — closest in spirit to what you're already doing. Good first real-world trading book for a mentee once ISLR/Tsay basics are in place. |
| *Algorithmic Trading: Winning Strategies and Their Rationale* — Ernest Chan | Core | Chan's follow-up — more strategy-specific (mean reversion, momentum, pairs trading), with the statistical rationale behind each. |
| *Advances in Financial Machine Learning* — Marcos López de Prado | Advanced | The book on why naive backtesting lies to you — overfitting, data leakage, the purging problem your [[07 Model Assessment, Bias-Variance and Resampling|ch.7]] notes already touch on, taken to full rigor. Read once you're seriously walk-forward-testing your own strategies, not before — it'll make more sense with real backtests already under your belt. |
| *Trading and Exchanges: Market Microstructure for Practitioners* — Larry Harris | Advanced | How order books, market makers, and exchange mechanics actually work — most relevant if the quant *trader* path (vs. dev/researcher) pulls harder, since it explains the environment HFT/market-making strategies operate in. |

---

## Tier 8 — Portfolio Construction & Risk Management

| Book | Level | Why it's here |
|---|---|---|
| *Active Portfolio Management* — Grinold & Kahn | Advanced | Practitioner-level portfolio construction — takes the weight-constraint idea in [[06 CAPM and Multifactor Models#3. Portfolio weights (brief note)|ch.6 §3]] much further: information ratios, alpha models, risk budgeting. |
| *Quantitative Risk Management* — McNeil, Frey, Embrechts | Advanced | The standard risk-management reference — tail risk, extreme value theory, copulas. Most relevant for bank/insurer risk roles, but genuinely useful context for anyone running real capital. |

---

## Tier 9 — Context & Career (short, non-technical)

Read once, not for technique — for a realistic feel of the field and its history.

| Book | Level | Why it's here |
|---|---|---|
| *My Life as a Quant* — Emanuel Derman | Any level | Short memoir from a physicist-turned-Goldman-quant — honest about what the work and culture actually feel like. Good first recommendation for a total beginner deciding if this world even appeals to them. |
| *The Quants* — Scott Patterson | Any level | Narrative nonfiction on the rise (and 2007-08 near-fall) of quantitative trading — gives a mentee the industry's history and cautionary tales in a genuinely readable format. |

---

## Suggested sequencing (tying it back to the career roadmap)

```
Right now (rest of Year 3):
  Mathematics for Machine Learning  →  ISLR  →  Tsay  →  Chan (Quantitative Trading)
  (alongside: Convex Optimization, read in parallel with the LP Duality chapter)

Final stretch / just after graduating:
  Fluent Python + CLRS (if quant dev)   |   Casella & Berger + Shreve I (if research/trading)
  Advances in Financial Machine Learning (once you're walk-forward-testing real strategies)

1-3 years in:
  ESL, Bishop, Shreve II, Hamilton — whichever tier matches the specialization that's pulling you
  Grinold & Kahn / McNeil et al. if portfolio/risk work is in the mix

Whenever, for context:
  Derman and Patterson — no prerequisite, read whenever curiosity strikes
```

For a mentee starting from zero: **Mathematics for Machine Learning → Python for Data Analysis → ISLR → Derman's memoir (for motivation) → Chan's Quantitative Trading**, then branch based on what actually excites them.

---
*Companion to the quant career-path discussion — see chat history for the roadmap this reading list supports. Not tied to a specific course; cross-links point to the relevant existing vault chapters where the overlap is direct.*
