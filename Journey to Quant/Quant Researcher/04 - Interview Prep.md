# 04 — Quant Researcher Interview Prep

> The format, what each round tests, and how to prepare for the most technically rigorous interview process in finance.

---

## 1. Typical interview structure

QR interviews at top firms are 4–6 rounds and are the most academically rigorous interviews in finance. The structure varies but typically includes:

**Phone screen:** A 30–45 minute conversation with a researcher. Expect 2–3 probability puzzles and one question about a paper you've read or research you've done. This is the filter round — roughly 80% of candidates don't pass it.

**Technical loop (3–4 rounds):**
1. **Probability and statistics** — derivations, puzzles, expected value arguments, and occasionally measure theory at top firms
2. **Machine learning** — conceptual depth, assumptions, what breaks when they fail
3. **Finance** — derivatives pricing, factor models, what drives P&L attribution
4. **Research presentation** — walk through a paper you've read or your own original work; defend it under questioning

**Behavioural + cultural fit:** Usually shorter and last. How do you work, how do you handle being wrong, what research are you excited about.

---

## 2. Probability and statistics round

This is the hardest round at most QR employers. What it tests:

**Probability puzzles (the Green Book style):**
- Expected number of steps to absorb in a random walk
- The probability that a fair random walk ever returns to 0 (it does, with probability 1 — the recurrence theorem)
- Expected value calculations with conditioning: P(A|B), iterated expectation, tower property
- Combinatorial problems: how many ways to arrange X under constraint Y

**Distributional knowledge:**
- Named distributions and their properties (Gaussian, Poisson, Exponential, Binomial, Gamma, Beta)
- Moment generating functions and how to use them
- The central limit theorem — precise statement, not just "averages are normal"
- Law of large numbers (weak vs. strong)
- Convergence concepts: in probability, almost surely, in distribution

**Statistics:**
- MLE: set up the likelihood, differentiate, solve for the estimator — for Gaussian, Bernoulli, Poisson
- Method of moments
- Sufficient statistics and what sufficiency means
- Hypothesis testing: p-value (precise definition), Type I / Type II errors, power
- Confidence intervals — precise interpretation, not the common misstatement
- The F-test: what it tests, what its assumptions are, when it breaks

*Resources:* Green Book + *Heard on the Street* for puzzles; Casella & Berger for the rigorous statistics; Blitzstein & Hwang Ch. 1–10 for probability intuition.

---

## 3. Machine learning round

Less competitive-programming, more conceptual depth. What it tests:

**Model assumptions and failure modes:**
- Linear regression: the five Gauss-Markov assumptions, what happens when each fails, how you diagnose violations
- Regularisation: what Ridge and Lasso are actually doing geometrically and probabilistically; the Bayesian interpretation
- Trees and ensembles: why bagging reduces variance, why boosting reduces bias, what makes a tree overfit
- SVM: the kernel trick, what the dual problem means, why the support vectors are the support vectors

**Model selection:**
- Bias-variance decomposition — not just the terms but the tradeoff at each model complexity
- Cross-validation: why it works, when it doesn't (time series — your ch.7 notes cover this), what look-ahead bias is
- Information criteria (AIC, BIC): what they're trading off, when to use which

**The ML-for-finance overlay:**
- What makes financial data different from iid data (autocorrelation, non-stationarity, regime changes)
- Why standard k-fold CV is wrong for time series and what to do instead (purging, embargo)
- What an information coefficient is and why it's the right evaluation metric for an alpha signal

*Resources:* ISLR for the conceptual baseline; *Advances in Financial ML* Ch. 1–6 for the finance-specific overlay; your Applied Analytics vault notes throughout.

---

## 4. Finance round

Derivatives and factor models dominate. What it tests:

**Derivatives pricing:**
- Black-Scholes: the assumptions, the PDE derivation via Itô's lemma, what each assumption breaking means for the model (jumps → need for stochastic vol, discrete hedging → hedging error)
- Greeks: what delta, gamma, theta, vega mean and how they change with spot / time / vol
- Put-call parity: derive it from a no-arbitrage argument, not just state it

**Factor models:**
- What the CAPM says and what it assumes; why those assumptions don't hold
- Multi-factor models (Fama-French): what each factor captures, why they persist (risk or mispricing?)
- Risk decomposition: idiosyncratic vs. systematic risk, why a diversified portfolio eliminates the former

**Time series:**
- Stationarity: what it is, the ADF test, why non-stationary series break OLS
- ARMA: identification from ACF/PACF, parameter estimation
- GARCH: why conditional heteroskedasticity exists in financial returns, what GARCH models

*Resources:* Hull for derivatives; your Financial Theory and Derivatives vault notes; *Analysis of Financial Time Series* (Tsay) for time series.

---

## 5. Research presentation round

The most open-ended round and often the one candidates underprepare. You will be asked to:
- Walk through a paper you've read — explain the research question, the methodology, the key results, and your critique
- OR present your own research (a project from ch.03)

**What the interviewers look for:**
- Do you understand *why* the methodology was chosen, not just what it was?
- Can you identify the assumptions and when they'd break?
- Do you have a genuine opinion about the result — is it convincing? What would make it more convincing?
- Can you defend it when pushed?

**Preparation:** Have one paper and one project you can present at depth. Practice out loud, including fielding "why did they do X instead of Y?" questions. The paper should be something you find genuinely interesting — that comes through.

---

## 6. Preparation timeline

```
Now → 3 months:   Blitzstein & Hwang (finish it)
                   Casella & Berger (Ch. 1–5: estimation, sufficiency, hypothesis testing)
                   Green Book puzzles: 5/week
                   R1 (factor research) as the "project to present"

3–6 months:        ISLR cover-to-cover (cross-links to vault already done)
                   *Advances in Financial ML* (de Prado) — methodology chapters
                   R2 (walk-forward backtest) as second project
                   Start a reading log: one paper per month, written summary

6 months → grad:   Shreve I (first half) — stochastic calculus for QR interviews
                   R3 (paper replication) — shows independent research capability
                   Mock interviews with a peer who can ask "why" relentlessly

Applications:      Target a mix of bank QR internships (more accessible) and
                   hedge fund QR programmes. Apply to MSc programmes simultaneously
                   as the graduate route into top hedge fund QR is via master's/PhD.
```
