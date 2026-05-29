# Financial Theory and Choice

> Foundations of how individuals (and firms) make consumption and
> investment decisions — first under certainty, then under uncertainty,
> and finally under the lens of market efficiency. The economic
> companion to the SDE machinery in [[../Stochastics/]] and the
> asset-level applications in [[../Derivatives/]] and
> [[../Fixed Income Securities/]].

---

## How to read these notes

The chapters are numbered in the recommended study order. The folder splits into three parts:

**Part I — Choice under certainty** (chapters 00–08) sets up the two-period consumption/savings problem, the axioms of rational choice, and the decoupling of investment and consumption decisions when capital markets exist (Fisher's separation theorem).

**Part II — Choice under uncertainty** (chapters 09–12) extends the framework to risky payoffs via expected utility, the Von Neumann–Morgenstern axioms, and the Pratt–Arrow apparatus for measuring risk aversion.

**Part III — Efficient Market Hypothesis** (chapters 13–15) connects the rational-choice framework to market prices: when do prices fully reflect available information, how do we test for it, and what anomalies break the theory?

| #  | Chapter                                                                                                                              | Status        |
| -: | :----------------------------------------------------------------------------------------------------------------------------------- | :------------ |
| 00 | [[00 - Introduction]] — financial economics, three pillars, research cycle                                                           | Reference     |
| 01 | [[01 - Models in Financial Economics]] — economic vs statistical models, CAPM, Black-Scholes, modeler's manifesto                    | Reference     |
| 02 | [[02 - Consumption and Savings Decision]] — two-period model, $W_0$, $C_0$, $W_0 - C_0$                                              | Core          |
| 03 | [[03 - Axioms of Choice under Certainty]] — completeness, non-satiation, transitivity, convexity                                     | Core          |
| 04 | [[04 - Indifference Curves, MRS and Utility]] — MRS, diminishing marginal utility, convexity                                         | Core          |
| 05 | [[05 - Opportunity Set and MRT]] — budget constraint, MRT, capital-market rate                                                       | Core          |
| 06 | [[06 - Real Investment Decision]] — concave frontier, diminishing returns, $\text{MRS}^* = \text{MRT}^*$                             | Core          |
| 07 | [[07 - Choice with Capital Markets]] — trading line, slope $-(1+r)$, optimal $C^*$                                                   | Core          |
| 08 | [[08 - Fisher's Separation Theorem]] — investment / consumption decisions decouple; transaction-cost breakdown                       | Core          |
| 09 | [[09 - Expected Utility Theory]] — expected utility vs expected value, St. Petersburg paradox                                        | Core          |
| 10 | [[10 - VNM Axioms and Utility Function]] — six axioms, Von Neumann–Morgenstern utility                                               | Core          |
| 11 | [[11 - Risk Aversion - Certainty Equivalent and Risk Premium]] — risk preferences via utility curvature                              | Core          |
| 12 | [[12 - Pratt-Arrow Risk Aversion]] — ARA, RRA, worked $20$k example                                                                  | Application   |
| 13 | [[13 - Efficient Market Hypothesis]] — three forms, implications, math                                                               | Core          |
| 14 | [[14 - Tests of the EMH]] — serial correlation, runs, filter, event studies                                                          | Practice      |
| 15 | [[15 - EMH Limitations and Anomalies]] — overreactions, excess volatility, seasonal patterns, CAPM failures                          | Reference     |

---

## Conventions

- $C_0, C_1$ denote current and future consumption. Some sources write $x_0, x_1$ for the same quantities.
- $W_0$ is initial wealth; $W_0 - C_0$ is the amount saved/invested.
- $r$ is the (constant) capital-market interest rate. When borrowing and lending rates differ we write $r_B$ and $r_L$.
- $i$ is the *real* rate of return on the *marginal* real investment.
- $U(\cdot)$ is the utility function over certain monetary payments; $EU(\cdot)$ is the expected-utility (Von Neumann–Morgenstern) function over uncertain payoffs.
- $\succ, \sim$ denote strict preference and indifference.
- All probability arguments default to the *physical* (real-world) measure unless explicitly marked $\mathbb{Q}$.

---

## Related folders

- [[../Stochastics/]] — SDE / probabilistic machinery. [[../Stochastics/05 - Diffusion Processes Catalogue]] is the GBM that Black-Scholes (chapter [[01 - Models in Financial Economics]]) assumes; [[../Stochastics/09 - Martingales]] is the no-arbitrage backbone of chapter [[13 - Efficient Market Hypothesis]].
- [[../QUANTFRAME/Concepts/BackGround to Kelly Criterion]] — log utility and the optimal-fraction analogue of chapter [[12 - Pratt-Arrow Risk Aversion]].
- [[../Fixed Income Securities/22 April 2026]] — bond PV / discounting is the concrete financial-economic side of chapter [[07 - Choice with Capital Markets]].
- [[../Derivatives/7 May 2026 - Personal Notes 1]] — derivative pricing applies the Black-Scholes statistical model from chapter [[01 - Models in Financial Economics]].
- [[../Time Series/24 April Assignment]] — OLS regression is the statistical-model fit method discussed in chapter [[01 - Models in Financial Economics]].
