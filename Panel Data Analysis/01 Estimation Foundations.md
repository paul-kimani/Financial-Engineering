# Estimation Foundations

## 1. The question underneath every regression

> [!tip] The question in the lime light
> What must be true before we interpret an OLS coefficient as evidence about the population relationship we care about?

Everything in this chapter — and everything panel data adds on top of it — is really just successive, more careful answers to that one question.

### The research target comes before the regression

The dataset is not supposed to dictate the question. The order is: economic question first, then down to the data — not the reverse. Concretely:

- **Estimand** — the *population* quantity you actually want to know (a parameter of the true, unobserved data-generating process). E.g. "the causal effect of a firm's market value on its investment."
- **Parameter** — the symbol standing for the estimand in your model, e.g. $\beta_1$ in $Y_i=\beta_0+\beta_1X_i+\epsilon_i$.
- **Estimator** — the *rule* (a formula/procedure applied to a sample) used to produce a guess at the parameter, e.g. $\hat\beta_1=\dfrac{\sum_i(X_i-\bar X)(Y_i-\bar Y)}{\sum_i(X_i-\bar X)^2}$ (see the full derivation in [[04 Supervised Learning Vs unsupervised learning.|Applied Analytics ch.4]]).
- **Estimate** — the specific *number* you get when you apply the estimator to one particular sample, e.g. $\hat\beta_1=0.42$.
- **Sampling distribution** — the distribution of the estimator's value *across hypothetical repeated samples* from the same population. This is what makes standard errors, confidence intervals and hypothesis tests meaningful — a single estimate is one draw from this distribution.

> [!example] Keeping the chain straight
> Estimand: "true average return to a year of schooling." Parameter: $\beta_1$ in a wage equation. Estimator: the OLS slope formula. Estimate: $0.087$ from your particular dataset. Sampling distribution: what $\hat\beta_1$ would look like if you redrew the sample many times — this is what $\widehat{\operatorname{se}}(\hat\beta_1)$ is trying to summarize.

## 2. The population model and the conditional mean

The **population linear model** posits a true relationship in the population, before any sample is drawn:

$$Y = \beta_0 + \beta_1X_1 + \dots + \beta_pX_p + u$$

Here $u$ is the **error** — everything that determines $Y$ other than the included $X$'s, together with any true nonlinearity or measurement noise. The model is a statement about the **conditional mean**:

$$\mathbb E(Y\mid X_1,\dots,X_p) = \beta_0+\beta_1X_1+\dots+\beta_pX_p$$

i.e. the $\beta$'s describe how the *average* of $Y$ moves as the $X$'s move, holding the rest fixed — not a claim that the model perfectly predicts any single $Y_i$.

### Error vs. residual — a distinction worth never blurring

This distinction runs through the whole unit, and resurfaces the moment you estimate a variance (see the $\hat\sigma^2$ unbiasedness proof in [[04 Supervised Learning Vs unsupervised learning.|Applied Analytics ch.4]]):

| | Error $u_i$ (or $\epsilon_i$) | Residual $\hat u_i$ (or $\hat\epsilon_i$) |
|---|---|---|
| Definition | $Y_i - (\beta_0+\beta_1X_i)$ | $Y_i - (\hat\beta_0+\hat\beta_1X_i)$ |
| Uses | the **true, unknown** parameters | the **estimated** parameters |
| Observable? | Never — it's a population quantity | Yes — computable from any fitted sample |
| Role | what OLS's assumptions are stated about | what you actually get to look at (diagnostics, $\hat\sigma^2$) |

You never see $u_i$. Every diagnostic plot, every $\hat\sigma^2$, every clustered standard error is built from $\hat u_i$ and is only a useful stand-in for $u_i$ to the extent your assumptions hold.

## 3. What OLS actually does vs. what identification requires

This is the single most important distinction in the whole "estimation foundations" toolkit, because it is exactly the distinction panel data methods (fixed effects, first differences) are built to exploit.

### OLS's own guarantee: sample orthogonality

OLS is defined as whatever $\hat\beta_0,\hat\beta_1$ minimizes the sum of squared residuals:

$$\text{SSR}(\beta_0,\beta_1) = \sum_i \left(Y_i - \beta_0 - \beta_1X_i\right)^2$$

**Deriving the normal equations.** Take the partial derivative with respect to $\beta_0$ (chain rule — derivative of the square, times derivative of the inside):

$$\frac{\partial\,\text{SSR}}{\partial \beta_0} = \sum_i 2(Y_i-\beta_0-\beta_1X_i)(-1) = -2\sum_i(Y_i-\beta_0-\beta_1X_i)$$

Setting this to zero and evaluating at the minimizing $\hat\beta_0,\hat\beta_1$ — the term inside the sum is exactly the residual $\hat u_i=Y_i-\hat\beta_0-\hat\beta_1X_i$ — gives the **first normal equation**:

$$\sum_i \hat u_i = 0$$

Now $\beta_1$'s partial derivative (an extra $X_i$ rides along from the chain rule, since the inside term is $-\beta_1X_i$):

$$\frac{\partial\,\text{SSR}}{\partial\beta_1} = \sum_i 2(Y_i-\beta_0-\beta_1X_i)(-X_i) = -2\sum_i X_i(Y_i-\beta_0-\beta_1X_i)$$

Setting to zero gives the **second normal equation**:

$$\sum_i X_i\hat u_i = 0$$

In matrix form (bundling both equations — one row of $X'$ is a row of 1's giving the first equation, the other row is the $X_i$'s giving the second), this is written:

$$X'\hat u = 0$$

> [!tip] These are corollaries, not assumptions
> $\hat\beta_0=\bar Y-\hat\beta_1\bar X$ — the familiar closed-form intercept formula — is just algebra applied to the first normal equation ($\bar Y-\beta_0-\beta_1\bar X=0$, divide the first normal equation through by $n$). The normal equations themselves are the important result: they are **mathematical facts, true of every OLS fit that has ever existed**, because they fall straight out of "set the derivative of a minimization to zero." It doesn't matter whether the true relationship is real, causal, linear, or complete nonsense — regress ice-cream sales on shark attacks and you'll still get $\sum_i\hat u_i=0$ exactly — these equations always hold, mechanically, by construction.

This says the residuals are *exactly, mechanically* uncorrelated with every regressor **in the sample you have** — always, by construction of the minimization, whether or not your model is any good. It is a fact about arithmetic, not about the world.

### OLS as projection — the geometric picture

There's a geometric way to see exactly why $X'\hat u=0$ has to hold, and it's worth carrying around because it makes the "always true, regardless of the model" point almost visual.

Picture $Y$ (the vector of your actual data — one coordinate per observation, so $n$ firms means $Y$ lives in $n$-dimensional space) as a fixed point floating in that space, *not* built from $X$ at all. Now picture every possible fitted-value vector you could construct from some combination of $\beta_0\cdot\mathbf 1+\beta_1\cdot X$ as a flat plane — a "tabletop" — sitting inside that same space. Every point on the tabletop is some possible fitted line's predictions; $Y$ itself generally isn't one of them, because real data doesn't fall exactly on any straight line.

![[OLS as Projection.svg]]

OLS's job is to find the point on the tabletop **closest** to $Y$ — that closest point is $\hat Y$, the fitted values. The leftover gap between $Y$ and that closest point is $\hat u$, the residual. The key geometric fact: **the closest point on a flat surface to something floating above it is always found by dropping straight down, perpendicular to the surface** — not at an angle. So the residual, being exactly that perpendicular drop, is automatically orthogonal to the tabletop (i.e. to every direction spanned by $X$) — which is precisely what $X'\hat u=0$ says in algebra. OLS isn't *trying* to make the residual perpendicular to $X$; it falls out for free, because "closest point on a flat surface" and "perpendicular drop" are the same thing.

> [!warning] Keep this picture sample-only
> Everything in this diagram — $Y$, the tabletop, $\hat Y$, $\hat u$ — is built purely from your observed sample. The true, unobservable population error $u_i$ has no place in this picture at all: it would require knowing the *true* $\beta_0,\beta_1$, which you never see. Don't conflate the residual $\hat u$ (drawable, sample-only) with the theoretical $u$ (never drawable, population-only) — see the error-vs-residual table in §2.

### Identification's requirement: population exogeneity

What actually lets you interpret $\hat\beta_1$ as an estimate of the *causal* or *structural* estimand is a completely different, substantive condition on the **population**:

$$\mathbb E(X'u) = 0 \qquad \text{(population exogeneity)}$$

This says the *true* error term is, on average, unrelated to the regressors *in the population*. Unlike $X'\hat u=0$, this is never guaranteed by algebra — it's an assumption about the world, and it can be false.

> [!warning] The trap
> $X'\hat u=0$ (sample orthogonality) holds **automatically** for every OLS regression you will ever run, no matter how badly misspecified. $\mathbb E(Xu)=0$ (population exogeneity) is the assumption doing all the actual identification work, and it is exactly what can fail — most importantly for this unit, when a persistent unit-specific factor is both omitted and correlated with $X$. Never mistake the guaranteed sample fact for the assumed population fact.

### Identification vs. estimation

- **Identification** asks: *if you had infinite data*, could you in principle recover the estimand? This is a question about the model and the assumptions (does $\mathbb E(Xu)=0$ hold?), not about sample size.
- **Estimation** asks: given the (finite, noisy) data you actually have, what's your best guess, and how uncertain is it?

A parameter can be well-estimated (small standard error, tight confidence interval) while being completely unidentified (the "precise" estimate is consistently estimating the *wrong* thing, because $\mathbb E(Xu)\neq0$). Precision is not evidence of identification — this is the conceptual seed of everything panel data does: repeated observations of the same unit are a tool for *improving identification*, not merely for adding sample size (see [[02 What Makes Data a Panel?]]).

## 4. Omitted-variable bias

Suppose the *true* model includes two regressors:

$$Y_i = \beta_1 + \beta_2X_i + \beta_3Z_i + u_i \qquad (\mathbb E(u_i\mid X_i,Z_i)=0)$$

but you estimate the **short regression**, omitting $Z$:

$$Y_i = \beta_1 + \beta_2X_i + v_i$$

### The derivation

Both equations describe the same $Y_i$, so comparing them directly shows the short regression's "error" is secretly absorbing the entire omitted variable:

$$v_i = \beta_3Z_i + u_i$$

> [!tip] What "short" means
> The subscript in $\hat\beta_{2,\text{short}}$ is just a label distinguishing *which regression* produced this coefficient — the short (truncated) regression that omits $Z$, as opposed to the full/true model's $\beta_2$. Both get called "the coefficient on $X$," but they are not the same number — the whole point of this derivation is to work out exactly how they differ.

As sample size grows, the OLS slope estimator converges to the population version of the familiar ratio (same shape as $\hat\beta_1=\sum_i(X_i-\bar X)(Y_i-\bar Y)/\sum_i(X_i-\bar X)^2$, just written with population covariance/variance):

$$\operatorname*{plim}\hat\beta_{2,\text{short}} = \frac{\operatorname{Cov}(X,Y)}{\operatorname{Var}(X)}$$

To see what this is *really* estimating, substitute the **true** model for $Y$ (since that's what actually generates the data) and expand using linearity of covariance, $\operatorname{Cov}(aW+bV,X)=a\operatorname{Cov}(W,X)+b\operatorname{Cov}(V,X)$:

$$\operatorname{Cov}(X,Y) = \operatorname{Cov}\big(X,\ \beta_1+\beta_2X+\beta_3Z+u\big) = \underbrace{\operatorname{Cov}(X,\beta_1)}_{=\,0} + \beta_2\operatorname{Cov}(X,X) + \beta_3\operatorname{Cov}(X,Z) + \operatorname{Cov}(X,u)$$

Two terms vanish here, but for entirely different reasons — worth never conflating:

- $\operatorname{Cov}(X,\beta_1)=0$ is a **pure algebraic identity**: a constant has zero covariance with anything, no assumptions needed.
- $\operatorname{Cov}(X,u)=0$ is **not free** — it holds only because the true model assumed $\mathbb E(u_i\mid X_i,Z_i)=0$ (population exogeneity, §3). If that assumption failed, this term wouldn't vanish either.

With $\operatorname{Cov}(X,X)=\operatorname{Var}(X)$ by definition, this leaves:

$$\operatorname{Cov}(X,Y) = \beta_2\operatorname{Var}(X)+\beta_3\operatorname{Cov}(X,Z)$$

Substituting back and dividing through by $\operatorname{Var}(X)$:

$$\operatorname*{plim}\hat\beta_{2,\text{short}} = \frac{\beta_2\operatorname{Var}(X)+\beta_3\operatorname{Cov}(X,Z)}{\operatorname{Var}(X)} = \beta_2 + \beta_3\cdot\frac{\operatorname{Cov}(X,Z)}{\operatorname{Var}(X)}$$

The short-regression coefficient equals the *true* effect $\beta_2$ plus a **bias term**: the omitted variable's own effect $\beta_3$, scaled by how strongly the omitted variable moves with the included regressor. The bias vanishes only if $\beta_3=0$ (the omitted variable doesn't actually matter for $Y$) or $\operatorname{Cov}(X,Z)=0$ (it doesn't move with $X$) — if both are nonzero, the short regression is contaminated no matter how large the sample gets.

### Direction of bias

| $\operatorname{Cov}(X,Z)$ | $\beta_3$ | Sign of bias | Net effect on $\hat\beta_{2,\text{short}}$ |
|---|---|---|---|
| $+$ | $+$ | $+$ | overstates $\beta_2$ |
| $+$ | $-$ | $-$ | understates $\beta_2$ |
| $-$ | $+$ | $-$ | understates $\beta_2$ |
| $-$ | $-$ | $+$ | overstates $\beta_2$ |
| $0$ (either) | any | $0$ | no bias — OVB vanishes exactly when $\operatorname{Cov}(X,Z)=0$ |

> [!tip] Why this table matters for panel data
> Replace $Z$ with $\alpha_i$ — an unobserved, **time-invariant** unit-specific trait (managerial quality, institutional quality, a household's unmeasured ability) — and this is *exactly* the pooled-OLS omitted-variable problem in [[04 Pooled OLS and the Unobserved-Effects Model]]. The entire motivation for fixed effects and first differences is: panel data lets you eliminate $\alpha_i$ algebraically (by differencing across time for the same unit) rather than having to measure it, argue $\operatorname{Cov}(X,\alpha_i)=0$, or find an instrument for it.

## 5. Heteroskedasticity, functional form, and influence — a working recap

These three diagnostic topics are prerequisite material from cross-sectional OLS. They matter for panel work too (pooled-OLS residuals can be heteroskedastic; a poorly specified functional form or a single influential observation can distort a panel result exactly as it would a cross-section) — but the *deep* toolkit for all three already lives in [[12 Regression Diagnostics, Heteroskedasticity and GLS|Applied Analytics ch.12]], so this section is a compressed pointer rather than a rebuild.

- **Heteroskedasticity**: $\operatorname{Var}(u_i\mid X_i)$ is not constant across $i$. OLS stays unbiased but its usual standard errors are wrong; use **heteroskedasticity-consistent (HC/robust) standard errors**, and test formally with **Breusch–Pagan** (regress squared residuals on the $X$'s and test joint significance). See ch.12 for the full derivation and the White/GLS/WLS/FGLS alternatives.
- **Functional form**: if the true conditional mean is nonlinear (e.g. includes $X^2$ or an $X_1\times X_2$ interaction) and you fit a purely linear model, you can badly misrepresent the relationship — recall the $Y=X^2$, $\operatorname{Cov}(X,Y)=0$ toy example in [[05 Multiple Linear Regression and the F-test#Toy example: why the model-fitting step matters as much as selection|Applied Analytics ch.5]]. **RESET** (Ramsey's Regression Equation Specification Error Test) checks this formally: add powers of the fitted values ($\hat Y^2,\hat Y^3,\dots$) as extra regressors and test their joint significance — significant coefficients suggest the original functional form is missing something systematic.
- **Outliers, leverage and influence**: a point can be unusual in its $Y$ (an **outlier**, large residual), unusual in its $X$ (**high leverage**, e.g. via the hat matrix diagonal $h_{ii}$), or both — and only the combination determines whether it actually *changes* your estimated coefficients (**influence**). **Cook's distance** summarizes overall influence on the whole fit; **DFBETA** measures influence on one coefficient specifically (how much $\hat\beta_j$ would change if that one observation were dropped).

> [!warning] Panel-specific twist to watch for later
> A firm or country that is an "influential outlier" in a *pooled* panel regression may simply be a unit with a persistently different level ($\alpha_i$ far from the rest) — not a data error at all. Diagnosing this correctly is part of why the poolability test in [[04 Pooled OLS and the Unobserved-Effects Model]] matters before trusting a pooled result.

## 6. Cheat sheet

| Concept | One-line definition |
|---|---|
| Estimand | the population quantity you want |
| Parameter | the symbol for it in your model |
| Estimator | the rule/formula producing a guess |
| Estimate | the number from one particular sample |
| Sampling distribution | how the estimator varies across hypothetical resamples |
| $X'\hat u=0$ | sample orthogonality — guaranteed by OLS's own minimization, always |
| $\mathbb E(Xu)=0$ | population exogeneity — the substantive identifying assumption, can fail |
| Identification | can the estimand be recovered even with infinite data? |
| Estimation | what's the best guess from the data you actually have? |
| OVB formula | $\operatorname{plim}\hat\beta_{2,\text{short}}=\beta_2+\beta_3\cdot\operatorname{Cov}(X,Z)/\operatorname{Var}(X)$ |

## 7. Open questions for revision

- [ ] Re-derive both normal equations from scratch: take $\partial\,\text{SSR}/\partial\beta_0$ and $\partial\,\text{SSR}/\partial\beta_1$, set each to zero, and simplify only as far as $\sum_i\hat u_i=0$ and $\sum_iX_i\hat u_i=0$ — don't jump straight to the closed-form $\hat\beta_0,\hat\beta_1$.
- [ ] Explain the OLS-as-projection picture in your own words: what is $Y$, what is the "tabletop," what is $\hat Y$, and why is $\hat u$ perpendicular to it? Why does the true error $u_i$ never appear in that picture?
- [ ] Re-derive the OVB formula from scratch, starting from the auxiliary regression $Z_i=\delta_0+\delta_1X_i+\text{error}$.
- [ ] Explain in your own words why $X'\hat u=0$ can never, by itself, be evidence that your model is correctly specified.
- [ ] Given a bank-profitability model (bank size, lagged profitability, capital ratio, loan-loss provisions, national tax rate), classify each regressor as plausibly strictly exogenous, predetermined, or endogenous (this question resurfaces formally with the exogeneity table in [[04 Pooled OLS and the Unobserved-Effects Model]]).
- [ ] Connect the OVB table above to $\operatorname{Cov}(X_{it},\alpha_i)\neq0$ — write out, in words, what "omitted variable" and "persistent unobserved heterogeneity" have in common.

---
*Source: BSE 3211/4211 Week 1–2 student notes and labs ("Estimation Foundations" chapter).*

---

## 8. Standard errors: what they measure, and what they cannot

> *Added 2026-10-25 from the Week 1 lecture paragraph: "The estimator is random before the sample is observed because a different sample would generally produce a different value. The estimate is fixed after the sample has been observed. Standard errors describe the sampling variation of the estimator; they do not describe variation in the outcome itself and they do not measure whether the identifying assumptions are credible."*

The paragraph makes three separate claims. Each one is unpacked below.

### 8.1 Estimator random, estimate fixed

Before sampling, the $Y_i$ are random variables, and any function of random variables is random, so $\hat\beta$ (the rule) is random. After sampling, the $Y_i$ are fixed numbers, so the estimate (e.g. $0.08$) is just a number with **no variance**. Variance belongs to the estimator, never to the estimate.

**Analogy.** The estimator is the die before it is rolled; the estimate is the face that came up. The face "4" has no variance. The die does.

### 8.2 How a formula replaces "repeated samples"

We never actually draw a second sample. The repeated-sampling story is a thought experiment. Algebra under the model's assumptions tells us what the spread across repeated samples *would be*, and the single sample we have fills in the unknowns.

Simplest case: $Y_1,\dots,Y_n$ i.i.d. with variance $\sigma^2$.

$$\operatorname{Var}(\bar Y)=\operatorname{Var}\Big(\tfrac1n\sum_i Y_i\Big)=\tfrac{1}{n^2}\sum_i\operatorname{Var}(Y_i)=\tfrac{1}{n^2}\,n\sigma^2=\frac{\sigma^2}{n}$$

- Pulling out $\tfrac1n$ gives $\tfrac1{n^2}$ because $\operatorname{Var}(aW)=a^2\operatorname{Var}(W)$.
- Writing the variance of the sum as the sum of the variances uses **independence**: all the covariance terms are zero.
- Making every term $\sigma^2$ uses **identical distribution**.

Two steps produce the SE:
1. **Theory** gives the shape of the sampling variance, $\sigma^2/n$. This is a statement about all possible samples.
2. **The one sample** supplies $s^2=\tfrac{1}{n-1}\sum_i(Y_i-\bar Y)^2$ as a plug-in estimate for $\sigma^2$, giving $\widehat{\operatorname{se}}(\bar Y)=s/\sqrt n$.

Regression works the same way. Under homoskedastic, uncorrelated errors, $\operatorname{Var}(\hat\beta\mid X)=\sigma^2(X'X)^{-1}$, with $\hat\sigma^2$ from the residuals plugged in.

**The bootstrap** is the version that actually does the thought experiment. It resamples the data with replacement many times, recomputes $\hat\beta$ each time, and takes the SD of those values. It is useful when no clean formula exists (see the wild-cluster bootstrap in [[05 Clustered and Robust Standard Errors]]).

**The catch.** The formula is only as good as the assumptions used to derive it. That leads to 8.3.

### 8.3 Dropping independence: the covariance term and the inflation factor

Without independence, the two-variable case is:

$$\operatorname{Var}(Y_1+Y_2)=\mathbb E\big[(Y_1-\mu_1)+(Y_2-\mu_2)\big]^2=\operatorname{Var}(Y_1)+\operatorname{Var}(Y_2)+\mathbf{2}\operatorname{Cov}(Y_1,Y_2)$$

The **2** is the cross term from $(a+b)^2=a^2+b^2+2ab$.

In general there are $n$ variance terms and $n(n-1)$ covariance terms:

$$\operatorname{Var}(\bar Y)=\frac{1}{n^2}\Big[\sum_i\operatorname{Var}(Y_i)+\sum_i\sum_{j\neq i}\operatorname{Cov}(Y_i,Y_j)\Big]$$

With a common correlation $\rho$, so that $\operatorname{Cov}=\rho\sigma^2$:

$$\operatorname{Var}(\bar Y)=\frac{1}{n^2}\big[n\sigma^2+n(n-1)\rho\sigma^2\big]=\frac{\sigma^2}{n}\big[1+(n-1)\rho\big]$$

**Which SE "inflates".** The **true** sampling SD grows by $\sqrt{1+(n-1)\rho}$. The **default formula** $s/\sqrt n$ assumes $\rho=0$, so it stays put and is now too small. As a result, t-stats come out too big and confidence intervals too narrow.

**Panel version.** Correlation lives *within* a unit (a firm's $T$ observations), while different units are roughly independent. The inflation factor (the **design effect**) is about $1+(T-1)\rho$. With $\rho=0.3$ and $T=10$: $1+9(0.3)=3.7$, so the variance is 3.7× what the naive formula reports and the SE is about 1.9× larger. Clustering by firm keeps these within-firm covariance terms ([[05 Clustered and Robust Standard Errors]]).

### 8.4 Bias vs. noise: why a small SE is not identification

$$\hat\beta-\beta=\underbrace{(\mathbb E[\hat\beta]-\beta)}_{\text{bias}}+\underbrace{(\hat\beta-\mathbb E[\hat\beta])}_{\text{sampling noise}}$$

- The SE measures **only the noise**: $\operatorname{se}(\hat\beta)=\sqrt{\mathbb E[(\hat\beta-\mathbb E[\hat\beta])^2]}$. It is the spread of $\hat\beta$ **around its own expectation**, not around the true $\beta$.
- The **bias** is zero only if the identifying assumptions hold ($\mathbb E(Xu)=0$, strict exogeneity in FE, no omitted confounders, no reverse causality). The SE formula *takes these as given*, so it cannot test them.

The same point in mean-squared-error form:

$$\text{MSE}=\mathbb E[(\hat\beta-\beta)^2]=\operatorname{Var}(\hat\beta)+\text{Bias}^2$$

A small SE shrinks only the first term. With a large $N\times T$ panel, a biased estimator is **precisely centred on the wrong value**.

**SE vs. outcome variation.** $\operatorname{Var}(Y_i)=\sigma^2$ is how noisy individual outcomes are. $\sigma^2/n$ is how noisy the *average* is. As $n\to\infty$ the SE goes to 0, but $\sigma$ does not change. A small SE means the parameter is precisely estimated, not that the outcome is predictable.

**Trading link.** A 20-year tick-data backtest gives a Sharpe ratio with a tiny SE. That precision says nothing about look-ahead or survivorship bias in the data. Precision is not validity.

### 8.5 Worked exercise: board independence and bank liquidity creation

**Q1. Write one plausible estimand in words.**

*First attempt:* "the expected causal change in bank liquidity creation if bank independence is raised by one unit."

**What was right.** It is causal (not an association), it is an expectation (an average over units), and it is phrased as an intervention without naming an estimator.

**What to tighten.**
- The treatment is *board* independence, not *bank* independence.
- Define the unit: "one unit" of a 0–1 share means going from 0% to 100%, which isn't realistic, so use +10 percentage points.
- Name the population: which banks, and which years.
- Name the outcome measure and the timing (e.g. Berger–Bouwman liquidity creation over total assets, next year).

**Model answer.** *The average causal effect, across Kenyan commercial banks over 2010–2024, of increasing the share of independent directors on the board by 10 percentage points on the bank's liquidity creation (scaled by total assets) in the following year.*

**Confounder.** Managerial quality $q$:
- better managers may welcome or attract independent directors ($q\to x$);
- better managers can safely create more liquidity ($q\to y$).

Leaving it out contaminates the **bias** term, not the noise term.

**Direction of bias.** Using the §4 formula, $\operatorname{plim}\hat\beta=\beta+\gamma\delta$ with $\gamma>0$ (effect of $q$ on $y$ holding $x$ fixed) and $\delta=\operatorname{Cov}(x,q)/\operatorname{Var}(x)>0$. So the bias is **upward**, and $\hat\beta$ overstates the effect.

- $\gamma$ is a *partial* effect, not a raw correlation. If $\gamma=0$, there is no bias however correlated $q$ and $x$ are.
- An upward bias makes a positive finding *less* convincing: the true $\beta$ is at most about $\hat\beta$, and could be zero.

**Panel angle.** If $q$ is time-invariant ($\alpha_i$), bank fixed effects remove it. A time-varying change such as a new CEO is not removed and breaks strict exogeneity.

**Q2. The estimated coefficient is 0.08. Identify parameter, estimator, estimate.**
- **Parameter:** $\beta$, the true, fixed, unknown effect of board independence on liquidity creation in $y_{it}=\beta x_{it}+\dots$. It equals the Q1 estimand only if the model is correctly specified (the identification question).
- **Estimator:** name which one. Pooled OLS is $(X'X)^{-1}X'y$. Given the $\alpha_i$ concern, the natural choice is the **within/FE estimator** $(\tilde X'\tilde X)^{-1}\tilde X'\tilde y$ with $\tilde x_{it}=x_{it}-\bar x_i$. It is random because it is a rule applied to a random sample.
- **Estimate:** $0.08$, fixed, with no variance. Interpret it in units: if $x$ is a 0–1 share and $y$ is over total assets, then 0%→100% independence changes liquidity creation by 8% of assets, and +10 pp changes it by $0.1\times0.08=0.008$ (0.8% of assets).

**Q3. Why doesn't a small SE prove the coefficient estimates the intended causal effect?**

*First attempt:* "the SE measures the deviations in the estimates from itself, not from the true beta". This is correct. More precisely, it is the spread around $\mathbb E[\hat\beta]$.

**Model answer.** *The standard error measures the sampling variability of the estimator around its own expectation $\mathbb E[\hat\beta]$ (how much $\hat\beta$ would vary across repeated samples). It says nothing about whether $\mathbb E[\hat\beta]$ equals the causal $\beta$. That depends on the identifying assumptions (no omitted confounders such as managerial quality, no reverse causality), which the SE formula takes as given. A biased estimator can have a tiny SE: in a large sample it is precisely centred on the wrong value. Precision is not validity.*

### 8.6 Open questions for revision

- [ ] Derive $\operatorname{Var}(\bar Y)=\sigma^2/n$ cold, naming which assumption does each step.
- [ ] Re-derive $\operatorname{Var}(\bar Y)=\frac{\sigma^2}{n}[1+(n-1)\rho]$ from scratch. Count the $n(n-1)$ covariance terms and don't drop the factor of 2.
- [ ] Explain in one sentence which SE "inflates" under positive within-firm correlation: the true one or the reported one.
- [ ] Write the bias/noise decomposition and $\text{MSE}=\operatorname{Var}+\text{Bias}^2$, and use them to answer "why isn't a small SE identification?"
- [ ] Write an estimand for a new question (e.g. "does CEO duality reduce bank risk-taking?") with treatment unit, population, outcome measure and timing.

---

## 9. Week 1 lab: residuals, errors and the OLS first-order conditions

> *Added 2026-11-02. Covers the Week 1 lab on Wooldridge's `ceosal1` (209 CEOs): `regress lsalary roe lsales`, with and without `vce(robust)`, plus the residual checks.*

### 9.1 The baseline regression

$$\log(\text{salary}) = \beta_0 + \beta_1\,\text{roe} + \beta_2 \log(\text{sales}) + u$$

| | Coef | Default SE | Robust (HC1) SE |
|---|---|---|---|
| roe | 0.0179 | 0.0040 | 0.0034 |
| lsales | 0.2751 | 0.0333 | 0.0301 |
| _cons | 4.3622 | 0.2939 | 0.2596 |

$n = 209$, $R^2 = 0.282$.

**Reading the coefficients.**
- **lsales (log on log) is an elasticity.** A 1% increase in sales goes with about 0.28% higher salary.
- **roe (log on level) is a semi-elasticity.** One extra point of ROE goes with about $100 \times 0.0179 \approx 1.8\%$ higher salary.

These are associations. Omitted CEO or firm quality could drive both ROE and pay (§4).

**Stata note.** Results are only saved if a log is open: `log using "file.log", replace text` at the top and `log close` at the end. `bcuse` loads data into memory only; nothing is written to disk without `save`.

### 9.2 Why the residuals sum to zero (exactly, not approximately)

Nobody imposes $\sum_i \hat u_i = 0$. It **falls out of** minimising SSR when the model has an intercept. The first-order condition for $\beta_0$ is

$$\frac{\partial\,\text{SSR}}{\partial\beta_0} = -2\sum_i(y_i - \hat\beta_0 - \hat\beta_1 x_i) = 0 \;\Longrightarrow\; \sum_i \hat u_i = 0$$

(This is the first normal equation from §3.) Software prints something like $10^{-10}$ only because of floating-point rounding.

**Intuition.** If the residuals summed to a positive number, the line would sit too low on average. Raising the intercept would lower SSR, so the fit couldn't have been at the minimum.

**Three consequences.**
- **No intercept, no guarantee.** With `noconstant` there is no $\beta_0$ condition, so $\sum\hat u_i \neq 0$ in general.
- **The line passes through the means.** Dividing the condition by $n$ gives $\bar y = \hat\beta_0 + \hat\beta_1 \bar x$.
- **It's a sample fact, not evidence.** It holds for every OLS fit with an intercept, however bad the model.

### 9.3 Do the population errors sum to zero in a sample? No.

| | Residual $\hat u_i$ | Error $u_i$ |
|---|---|---|
| Defined with | estimated $\hat\beta$ | true $\beta$ |
| Lives in | the sample | the population |
| Orthogonal to $x$? | $\sum x_i\hat u_i = 0$: **guaranteed** by the first-order conditions | $E(xu) = 0$: an **assumption** that can fail |
| Observable? | yes | never |

The true β's are not chosen to fit your sample, so nothing forces the $u_i$ to cancel. $E(u) = 0$ is a statement about the population. In a sample, $\sum_i u_i$ is random with

$$E\Big(\sum_i u_i\Big) = 0, \qquad \operatorname{Var}\Big(\sum_i u_i\Big) = n\sigma^2 \quad \text{(i.i.d.)}$$

The *average* $\bar u \to 0$ by the law of large numbers. The *sum* does not: its SD $\sigma\sqrt n$ grows.

**The link between the two.** Substitute the true model into the residual:

$$\hat u_i = u_i - (\hat\beta_0 - \beta_0) - (\hat\beta_1 - \beta_1)x_i$$

The residual is the error minus OLS's estimation mistakes. When $\operatorname{Cov}(x,u) \neq 0$, $\hat\beta$ shifts away from $\beta$ just enough to make $\hat u$ orthogonal to $x$. The estimation mistake *is* the bias.

**One line to remember:** $\hat u$ is always clean because OLS makes it clean; $u$ is clean only if the world is.

### 9.4 A large residual does not automatically mean a data error

A residual measures distance from **the model's prediction**, not from the truth. Large residuals can come from:
1. **Genuine noise:** high or unequal error variance (heteroskedasticity).
2. **An omitted variable,** e.g. a founder-CEO or a high-paying industry. The residual is information.
3. **Wrong functional form:** a straight line through a curve misses most at the ends.
4. **Real extreme cases in skewed data:** the largest salary in ceosal1 is about \$14.8m against a mean of about \$1.3m.

A data error is a fifth possibility, confirmed against the source, never assumed from the residual's size.

**Large residual ≠ influential.** Influence also needs high leverage (an unusual $x$). A high-leverage point can pull the line towards itself and end up with a *small* residual. Dropping large-residual points because they "look wrong" biases results towards your model and understates $\sigma^2$.

### 9.5 Checking the first-order conditions in R and Stata

```r
crossprod(model.matrix(m0), resid(m0))   # X'u_hat: 3x1 vector, all ~0
cor(ceosal1$roe, resid(m0))              # ~0
cor(log(ceosal1$sales), resid(m0))       # ~0
```
```stata
predict ehat, residuals
correlate roe ehat
correlate lsales ehat
quietly summarize ehat
display r(mean)          // ~0; must come straight after summarize
```

- `model.matrix(m0)` is $X$ (column of 1s, roe, log sales).
- `crossprod(A, B)` computes $A'B$, so the first line gives $X'\hat u = [\sum\hat u_i,\ \sum \text{roe}_i\hat u_i,\ \sum \text{lsales}_i\hat u_i]'$.
- `quietly` suppresses output but stores results in `r()`.

**Purpose.** (1) Confirm the software solves the minimisation you derived. (2) The main lesson: **residual–regressor correlations are zero by construction, so they cannot test exogeneity.**

**Why the correlation, not just the sum, is zero (derived live).** Start from

$$\operatorname{Cov}(x,\hat u) = \tfrac{1}{n-1}\sum_i (x_i - \bar x)(\hat u_i - \bar{\hat u})$$

- $\bar{\hat u} = 0$ by the **first** normal equation, so $(\hat u_i - \bar{\hat u}) = \hat u_i$.
- Distribute: $\sum_i (x_i - \bar x)\hat u_i = \sum_i x_i\hat u_i - \bar x\sum_i \hat u_i$.
- $\bar x\sum_i\hat u_i = 0$ by the **first** normal equation ($\beta_0$ condition).
- $\sum_i x_i\hat u_i = 0$ by the **second** normal equation ($\beta_1$ condition).

So $\operatorname{Cov}(x,\hat u) = 0$ and $\operatorname{Corr}(x,\hat u) = 0$. The zero *correlation* needs **both** conditions. With `noconstant`, $\sum x_i\hat u_i = 0$ still holds but $\bar x\sum\hat u_i$ survives.

**Vocabulary.** Both are **first-order** conditions. A **second-order** condition is different: it checks that the stationary point is a minimum (the Hessian $2X'X$ is positive definite).

### 9.6 Lab questions

**Q. Why are these correlations approximately zero even if the model omits an important variable?**
*Model answer.* Minimising SSR gives two first-order conditions, $\sum\hat u_i = 0$ and $\sum x_i\hat u_i = 0$, so $\sum(x_i - \bar x)\hat u_i = \sum x_i\hat u_i - \bar x\sum\hat u_i = 0$ and $\operatorname{Corr}(x,\hat u) = 0$ apart from rounding. These conditions use only the sample, not the true data-generating process, so they hold even when an important variable is omitted. Any correlation between $x$ and the omitted variable is absorbed into $\hat\beta$ as omitted-variable bias rather than showing up in the residuals. Residual correlations therefore cannot test $E(xu) = 0$.

**Q. What population condition is violated if managerial quality affects salary and is correlated with sales?**
*Model answer.* Exogeneity, $E(u \mid \text{roe}, \text{lsales}) = 0$, or at least $\operatorname{Cov}(\text{lsales}, u) = 0$. Omitted quality $q$ enters the error, $u = \gamma q + e$. Because $\gamma \neq 0$ and $\operatorname{Cov}(\text{lsales}, q) \neq 0$, we get $\operatorname{Cov}(\text{lsales}, u) = \gamma\operatorname{Cov}(\text{lsales}, q) \neq 0$. OLS is biased and inconsistent for the sales elasticity (likely upward), and since roe and lsales are correlated the bias can spill into roe. Residual diagnostics can't reveal it, because $\operatorname{Cov}(\text{lsales}, \hat u) = 0$ mechanically.

(First attempt said "it affects $E[\hat\beta] = \beta$". That is the **consequence**; the question asks for the **condition**.)

**Q. Why can't good in-sample fit establish exogeneity?**
*Model answer.* Fit ($R^2 = 1 - \text{SSR}/\text{SST}$) is about the **size** of the residuals; exogeneity ($E(u \mid x) = 0$) is about the **correlation** between $x$ and the true error. Neither implies the other:
- **High fit, not exogenous:** a confounded $x$ can fit well precisely because it proxies for the omitted variable, so confounding can *raise* $R^2$.
- **Low fit, exogenous:** a randomised $x$ is exogenous by design but can give a tiny $R^2$.

And OLS forces $\operatorname{Cov}(x,\hat u) = 0$ however well it fits. Exogeneity must be justified by economic reasoning and research design.

(Wording fix: OLS minimises **SSR**; the first-order conditions are *set to zero* at the minimum, not "minimised".)

---

## 10. Identification and conditional exogeneity

### 10.1 Identification vs. estimation

- **Identification:** with infinite data, would the observed variation and your assumptions pin down **one** value of the parameter? This is about logic and assumptions, not sample size.
- **Estimation:** given identification, how do you compute a value from a finite sample?

Identification fails when **two different stories about the world produce the same data**. No amount of data separates them, and software never warns you.

**Leverage and ROA.** A negative slope is predicted by two stories:

| Story | Mechanism | Debt causal? |
|---|---|---|
| A: debt hurts | interest burden and distress costs reduce ROA | yes, $\beta < 0$ |
| B: managerial quality | good managers choose low debt **and** produce high ROA | no; $\beta$ could be 0 |

Under B, the OVB term is $\gamma\delta$ with $\gamma > 0$ and $\delta < 0$, so a negative $\hat\beta$ appears even if $\beta = 0$.

**What identification requires:** a **source of variation** plus a **reason to believe it's clean**. Ask why X differs across units. In a controlled regression, the reason is the assumption of conditional exogeneity. Later designs (fixed effects, instruments, natural experiments, timing restrictions) replace or modify that assumption.

### 10.2 Conditional exogeneity: $E(u_i \mid X_i) = 0$

**In words:** among observations with the same included X values, the omitted stuff averages zero. Knowing X tells you nothing about u.

$E(u \mid X) = 0 \Rightarrow E(Xu) = 0 \Rightarrow \operatorname{Cov}(X,u) = 0$. It is **stronger** than zero correlation, because it also rules out non-linear dependence.

**Lecturer wording.** "Residual orthogonality is imposed by OLS": read *imposed* as "forced by the first-order conditions", not "chosen by us".

### 10.3 The four threats: one idea, four routes

Common idea: something in $u$ becomes linked to $X$.

| Threat | Route to $\operatorname{Cov}(X,u) \neq 0$ | Typical bias | Finance example |
|---|---|---|---|
| Omitted variable | $q$ is in $u$ and moves with $X$: $\gamma\operatorname{Cov}(X,q)$ | sign of $\gamma\delta$ | quality → leverage and ROA |
| Simultaneity | $Y$ also causes $X$, so shocks to $u$ feed into $X$ | depends on the feedback | risk ↔ interest rate ↔ default |
| Measurement error | observed $X^* = X + e$; $e$ sits in $X^*$ and (with a minus sign) in the composite error | classical case: attenuation towards 0 | misclassified leverage |
| Sample selection | being observed depends on $u$ | depends on the selection rule | only survivors report full histories |

---

## 11. Omitted-variable bias: lecture framing and simulation

### 11.1 What the lecture adds to §4

The lecture writes the true model as $Y = \beta_0 + \beta_1X + \beta_2Z + u$ with $E(u \mid X,Z) = 0$, so

$$v = \beta_2 Z + u, \qquad \operatorname{plim}\hat\beta_{1,\text{short}} = \beta_1 + \beta_2\frac{\operatorname{Cov}(X,Z)}{\operatorname{Var}(X)}$$

(Lecturer notation "plim(β̂₁,short)": the "short" is a subscript label, not a second argument.)

**Each term has one job.**
- $\beta_1\operatorname{Var}(X)$: the genuine effect.
- $\beta_2\operatorname{Cov}(X,Z)$: the bias channel, $X \leftrightarrow Z \to Y$.
- $\operatorname{Cov}(X,u) = 0$: from $E(u \mid X,Z) = 0$, an assumption about the **long** model's error. The short error $v$ is *not* exogenous: $\operatorname{Cov}(X,v) = \beta_2\operatorname{Cov}(X,Z)$.

**$\operatorname{Cov}(X,Z)/\operatorname{Var}(X) = \delta$**, the slope from regressing $Z$ on $X$. The bias reads as a chain: X up 1 → Z up $\delta$ → Y up $\beta_2\delta$, all credited to X.

**"Downward" means the signed value moves down, not that the effect looks smaller.**
- If $\beta_1 < 0$, a downward bias makes the effect **more negative** (debt looks more harmful).
- If $\beta_1 > 0$, a downward bias shrinks it towards 0 or **flips its sign**.

**Sign rule:** sign(bias) = sign($\beta_2$) × sign($\operatorname{Cov}(X,Z)$). Always justify both signs.

**Multiple regressors.** With several included X's, $\delta$ is Z's coefficient on X in a regression of Z on **all** the included regressors: how Z moves with X after accounting for the other controls.

### 11.2 The simulation

```r
set.seed(4211)
n <- 1000
quality  <- rnorm(n)
leverage <- -0.7 * quality + rnorm(n)
roa      <- 0.05 - 0.03 * leverage + 0.08 * quality + rnorm(n, sd = 0.05)
short <- lm(roa ~ leverage)
long  <- lm(roa ~ leverage + quality)
```

**Predict before running.**
- $\operatorname{Var}(\text{quality}) = 1$; $\operatorname{Var}(\text{leverage}) = 0.49 + 1 = 1.49$; $\operatorname{Cov} = -0.7$.
- $\delta = -0.7/1.49 = -0.470$.
- Bias $= 0.08 \times (-0.470) = -0.0376$.
- Short regression → $-0.03 - 0.0376 \approx -0.068$.
- $\operatorname{Corr}(\text{leverage}, \text{quality}) = -0.7/\sqrt{1.49} \approx -0.57$.

**Stata results (seed 2009):** short **−0.0683** (SE 0.0022); long **−0.0271** (SE 0.0017); quality 0.0831. The prediction matched almost exactly.

**Exact in-sample identity:** $\hat\beta_{\text{short}} = \hat\beta_{1,\text{long}} + \hat\beta_{2,\text{long}}\cdot\hat\delta$, to every decimal place.
```stata
quietly reg quality leverage
scalar d = _b[leverage]
quietly reg roa leverage quality
display _b[leverage] + _b[quality]*d
```

**Why including quality moves the coefficient up towards −0.03.** Quality raises ROA **directly** (+0.08) and **separately** lowers leverage. In the short regression, high-leverage firms are disproportionately low-quality, and low quality directly lowers their ROA; that shortfall is credited to debt. Controlling for quality means comparing firms with the same quality, so only the part of leverage unrelated to quality (the `rnorm(n)` piece) is used. That is the Frisch–Waugh idea.

(First-attempt slips: "quality increases ROA *by reducing leverage*" is not the bias channel, since the path quality → less debt → higher ROA is part of leverage's genuine effect. And "reducing the coefficient" should be "it rises from −0.068 towards −0.03": less negative, smaller in magnitude.)

### 11.3 Experiments: the two necessary links

| Case | Change | Bias? | More precise model |
|---|---|---|---|
| Both links | original | yes, $\beta_2\delta < 0$ | (short is wrong) |
| $\delta = 0$ | `leverage <- rnorm(n)` | no | **long**: quality removed from the error. $\operatorname{Var}(v) = 0.0089$ vs $0.0025$, so SE ≈ 0.0030 vs 0.0016 |
| $\beta_2 = 0$ | `+ 0*quality` | no; both centre on −0.03 | **short**: no collinearity. Long SE inflated by $1/\sqrt{1-\rho^2}$, ≈ 0.0016 vs 0.0013 |

Omitting a relevant but uncorrelated Z costs **precision**, not validity. Including an irrelevant but correlated Z also costs precision.

---

## 12. Heteroskedasticity and robust standard errors

### 12.1 The picture

Plot firm size against earnings. Small firms hug the line; big firms scatter widely, so the cloud is a **funnel**. That is heteroskedasticity: $\operatorname{Var}(u_i \mid X_i) = \sigma_i^2$, a scatter that differs across observations (Greek *hetero* = different, *skedasis* = scatter).

**Two separate assumptions, two separate jobs.**

| Assumption | Controls |
|---|---|
| $E(u_i \mid X_i) = 0$ (mean) | whether $\hat\beta$ is right: unbiased, consistent |
| $\operatorname{Var}(u_i \mid X_i) = \sigma^2$ (spread) | whether the default SE is right, and whether OLS is efficient |

A funnel can still be centred on the line, so $\hat\beta$ is fine. Analogy: a scale that reads 2 kg high or low at random is noisy but correct on average. **Noise isn't bias.**

### 12.2 The seesaw: why the default SE goes wrong

The SE answers "how much would the slope wobble in a new sample?" The slope is a seesaw: points near the middle of the x-range barely tilt it, and points at the ends tilt it a lot.
- The **default** SE assumes every point is equally noisy.
- If the noisy points sit at the **ends**, the slope wobbles more than the default thinks, so the default SE is **too small**.
- If they sit in the **middle**, the default SE is **too big**. This was ceosal1's case.

**Bias vs wobble.** Noise doesn't push the line in a direction; it makes the tilt jump both ways from sample to sample. A consistent push in one direction would be bias (exogeneity failing).

**Worked check.** Repayment regressed on credit score, with weak-score borrowers (low end of x) much more variable → default SE **too small**.

### 12.3 The formula (one regressor)

From $\hat\beta_1 = \beta_1 + \sum_i (x_i - \bar x)u_i / SST_x$, where $SST_x = \sum_i (x_i - \bar x)^2$, with independent errors:

$$\operatorname{Var}(\hat\beta_1 \mid X) = \frac{\sum_i \overbrace{(x_i - \bar x)^2}^{\text{distance from middle}}\;\overbrace{\sigma_i^2}^{\text{noise of point } i}}{SST_x^{\,2}}$$

**Default, derived live.** Set $\sigma_i^2 = \sigma^2$.
- $\sigma^2$ is a constant, so it comes out of the sum: $\dfrac{\sigma^2\sum_i(x_i-\bar x)^2}{SST_x^2}$.
- One copy of the sum cancels: $\dfrac{\sigma^2}{SST_x}$.

Since $SST_x \approx n\operatorname{Var}(x)$:

$$SE(\hat\beta_1) \approx \frac{\sigma}{\sqrt n\,\operatorname{sd}(x)}$$

A slope is precise when $\sigma$ is small (points hug the line), $n$ is large, and $\operatorname{sd}(x)$ is large (a long seesaw). If every firm has about 40% debt, the leverage slope is imprecise however many firms you have.

Under a funnel you can't pull $\sigma_i^2$ out of the sum, so nothing cancels.

### 12.4 Where robust (HC) SEs come in

Robust SEs are **not a different estimator**. Same OLS, same $\hat\beta$, same $R^2$; only the variance formula differs. The two versions differ in what they put in for the unknown $\sigma_i^2$:

| | In place of $\sigma_i^2$ | Formula |
|---|---|---|
| Default | one common $\hat\sigma^2 = \sum\hat u_i^2/(n-2)$ | $\hat\sigma^2/SST_x$ |
| Robust (White/HC0) | each point's own $\hat u_i^2$ | $\sum_i (x_i - \bar x)^2\hat u_i^2 / SST_x^2$ |

Stata's `vce(robust)` and R's `vcovHC(type = "HC1")` multiply by $n/(n-k)$ (HC1). HC3 inflates high-leverage points.

**Why one noisy $\hat u_i^2$ is good enough.** You never need each $\sigma_i^2$, only the weighted sum. $\frac1n\sum(x_i-\bar x)^2\hat u_i^2 \to \frac1n\sum(x_i-\bar x)^2\sigma_i^2$ by the law of large numbers. This is why HC SEs can be unreliable in small samples.

**Matrix version (the sandwich).**

$$\operatorname{Var}(\hat\beta \mid X) = (X'X)^{-1}\,(X'\Omega X)\,(X'X)^{-1}, \quad \Omega = \operatorname{diag}(\sigma_1^2,\dots,\sigma_n^2)$$

Robust estimates the middle ("meat") with $\sum_i \hat u_i^2 x_ix_i'$. With $\Omega = \sigma^2 I$, it collapses to $\sigma^2(X'X)^{-1}$.

**Worked example.** $x = 1,2,3,4$ ($\bar x = 2.5$, $SST_x = 5$), residuals $0,\ 0.2,\ -0.4,\ 0.2$.

| $x_i$ | $(x_i-\bar x)^2$ | $\hat u_i^2$ | product |
|---|---|---|---|
| 1 | 2.25 | 0 | 0 |
| 2 | 0.25 | 0.04 | 0.01 |
| 3 | 0.25 | 0.16 | 0.04 |
| 4 | 2.25 | 0.04 | 0.09 |
| Sum | 5 | 0.24 | 0.14 |

- Default: $\hat\sigma^2 = 0.24/2 = 0.12$; Var $= 0.12/5 = 0.024$; **SE ≈ 0.155**.
- Robust: Var $= 0.14/25 = 0.0056$; **SE ≈ 0.075**. Smaller, because the biggest residual sits mid-seesaw.
- Swap to residuals $0.2,\ 0,\ -0.4,\ 0.2$: robust sum $= 0.09 + 0 + 0.04 + 0.09 = 0.22$, Var $= 0.0088$, **SE ≈ 0.094**. Up, because a 0.2 residual moved from weight 0.25 to weight 2.25. **The default SE doesn't change** ($\sum\hat u_i^2$ is still 0.24): it only sees total noise, never where it sits.

**"Robust regression" is misleading language.** In statistics that phrase means outlier-resistant estimators (Huber M-estimation, LAD, Stata's `rreg`) that change the line. `vce(robust)` keeps $\hat\beta$ and replaces only the covariance matrix. Say "OLS with heteroskedasticity-robust standard errors".

### 12.5 Efficiency: why OLS is no longer "best"

Under a funnel, noisy points carry less information about where the line is, yet OLS weights all points equally. **Weighted least squares** minimises $\sum_i \hat u_i^2/\sigma_i^2$ (inverse-variance weighting) and wobbles less. **Gauss–Markov** (OLS is BLUE) needs homoskedasticity: without it OLS is still unbiased and consistent, but not *best*.

**Trade-off.** WLS needs the $\sigma_i^2$ pattern known; guessing it wrong can do harm. Robust SEs need no such knowledge. Hence the applied default of OLS + robust SEs.

**Same idea elsewhere:** the minimum-variance portfolio overweights low-variance assets; meta-analysis weights studies by $1/SE^2$; volatility clustering in returns is heteroskedasticity over time (GARCH models $\sigma_t^2$).

### 12.6 Breusch–Pagan: what it does and doesn't do

**Mechanics.** Save $\hat u_i$, square it, regress $\hat u_i^2$ on the regressors, and test whether the slopes are jointly zero. $H_0$: noise size is unrelated to x. Statistic: $LM = n\cdot R^2 \sim \chi^2_k$.

**What it doesn't do.**
1. **Failing to reject ≠ homoskedasticity.** The test has low power in small samples, and it only detects variance that changes linearly with the specified regressors. U-shapes or funnels driven by other variables slip through; the White test adds squares and interactions.
2. **It says nothing about endogeneity.** BP concerns the **spread** of u; causality needs its **mean**, $E(u \mid x) = 0$.
3. **It doesn't validate the conditional mean, and a wrong shape can fake heteroskedasticity.** A line through a curve gives residuals large at the ends.

**Koenker version.** Original BP **assumes normal errors**, using $\operatorname{Var}(u^2) = 2\sigma^4$ as the benchmark for how much $\hat u_i^2$ naturally varies. With **fat tails** (excess kurtosis), $\hat u_i^2$ varies much more, so the original test **over-rejects**. Koenker's $n\cdot R^2$ measures that variability from the data, so it needs no normality. Prefer it for finance data (returns, executive pay, P&L).

| | Original BP | Koenker |
|---|---|---|
| Benchmark for noise in $\hat u_i^2$ | assumed normal: $2\sigma^4$ | measured from the data |
| With fat tails | rejects too often | valid |
| Default in | Stata `estat hettest` (fitted values only) | R `bptest()` (all regressors) |

To match R in Stata: `estat hettest, rhs iid`.

**Exam check.** "BP p = 0.40, so no heteroskedasticity, so my coefficients are causal and default SEs are fine" has three mistakes:
- failing to reject isn't proof of equal scatter;
- causality needs the mean assumption, which BP doesn't test;
- BP ignores correlation across observations (e.g. common daily market shocks), which also invalidates default SEs.

### 12.7 Lab code and ceosal1 results

```r
library(lmtest); library(sandwich)
summary(m0)$coef
coeftest(m0, vcov. = vcovHC(m0, type = "HC1"))
bptest(m0)
plot(fitted(m0), resid(m0), xlab = "Fitted log salary", ylab = "Residual")
abline(h = 0, lty = 2)
```
```stata
bcuse ceosal1, clear
regress lsalary roe lsales
estimates store conventional
estat hettest
estat hettest, rhs iid
regress lsalary roe lsales, vce(robust)
estimates store robust
estimates table conventional robust, b(%9.4f) se(%9.4f)
rvfplot, yline(0)
```

**Debugging notes from the run.**
- `r(111) estimation result robust not found`: `estimates store robust` was missing.
- `sales` was typed instead of `lsales`, which estimates a different model ($R^2 = 0.13$).
- `set obs 1000` on top of loaded data adds empty rows; `clear` before simulating and reload before the next exercise.

**Results.** Stata default BP: $\chi^2(1) = 0.27$, **p = 0.60**. No evidence of heteroskedasticity against fitted values.

**Reading the residual-vs-fitted plot.**
- Centred on 0 (mechanical).
- Roughly constant band, about −0.8 to +0.6, so no funnel.
- No arch or U, so no sign of a wrong functional form.
- **Fat right tail:** residuals stop near −1 but reach +2.8. A residual of 2.8 in logs means salary is about $e^{2.8} \approx 16\times$ the prediction. Real outliers, likely omitted factors.
- The big residuals sit mid-range, where seesaw weights are small, which explains why the robust SEs are *smaller* than the default.
- Few points at the ends, so the spread there can't really be judged.

**Lab questions.**
- **Coefficients identical, SEs differ?** Yes: $\hat\beta = (X'X)^{-1}X'y$ involves no variance assumption. The default uses one common $\hat\sigma^2$; the HC sandwich uses each $\hat u_i^2$ weighted by $(x_i - \bar x)^2$. (Not "the sigma of all the error terms": robust SEs never see the true errors.)
- **Which assumption changes?** Homoskedasticity is dropped. Still required: exogeneity, **independence across observations** (hence clustering in panels), and a reasonably large sample.
- **Do robust SEs fix omitted managerial quality?** No. Omission violates $E(u \mid X) = 0$ and biases $\hat\beta$ itself. Robust SEs leave $\hat\beta$ unchanged and only fix the variance estimate, while assuming exogeneity. Bias concerns the **mean** of u; robust SEs concern its **variance**. The short regression's −0.068 would stay −0.068 with `vce(robust)`: a precise, wrong number.

---

## 13. Functional form

### 13.1 Why a wrong shape violates $E(u \mid X) = 0$

If the truth is $Y = \beta_0 + \beta_1X + \beta_2X^2 + u$ but you fit a line, the short error is

$$v = \beta_2X^2 + u$$

This is **OVB where the omitted variable is $X^2$**. Both links hold: $X^2$ affects Y, and $X^2$ is correlated with X. So $E(v \mid X) \neq 0$ even when X is exogenous.

**What the linear coefficient averages:** a weighted average of the changing slopes, weighted towards where the data sits. A low-debt sample makes debt look mildly harmful; a high-debt sample makes it look very harmful.

**Apparent heteroskedasticity.** A line through a curve misses most at the ends, giving an arch in the residual plot that BP can mistake for changing variance.

**Problems listed in the notes:** omitted nonlinearities, missing interactions, inappropriate scale.

### 13.2 Squares: no single coefficient is "the effect"

$$\frac{dY}{dX} = \beta_1 + 2\beta_2X$$

**Example.** $\beta_1 = -0.01$, $\beta_2 = -0.05$:
- at $X = 0.2$: $-0.01 + 2(-0.05)(0.2) = -0.03$;
- at $X = 0.8$: $-0.09$, so debt is three times as harmful.

$\beta_1$ alone is the slope **at X = 0**. Report effects at meaningful values (mean, quartiles) or the **average marginal effect**: compute $\beta_1 + 2\beta_2X_i$ for each unit, then average.

**First-principles derivation (done live).** For a 1-unit step:

$$\Delta Y = [\beta_0 + \beta_1(X+1) + \beta_2(X+1)^2] - [\beta_0 + \beta_1X + \beta_2X^2]$$

Expand $(X+1)^2 = X^2 + 2X + 1$ and cancel $\beta_0$, $\beta_1X$ and $\beta_2X^2$:

$$\Delta Y = (\beta_1 + \beta_2) + 2\beta_2X$$

That is one extra $\beta_2$ compared with the derivative. For a step of size $h$:

$$\frac{\Delta Y}{h} = \beta_1 + 2\beta_2X + \beta_2h \;\xrightarrow{\;h\to0\;}\; \beta_1 + 2\beta_2X$$

The derivative is the tangent slope at a point. Over a finite step the curve keeps bending, and $\beta_2h$ is that extra bend. For leverage as a share, "+1 unit" is 0% → 100%, so the gap matters. For +10 pp at $X = 0.2$: tangent $-0.030$, exact $-0.035$ per unit.

### 13.3 Interactions

$$Y = \beta_0 + \beta_1X + \beta_2D + \beta_3(X\times D) + u$$

- $D = 0$: slope $\beta_1$.
- $D = 1$: $Y = (\beta_0 + \beta_2) + (\beta_1 + \beta_3)X$, so the slope is $\beta_1 + \beta_3$.

$\beta_3$ is the **difference** in slopes, not group 1's slope. With $\beta_1 = -0.02$ and $\beta_3 = -0.03$, the D = 1 slope is −0.05. Likewise, $\beta_2$ is D's effect **only at X = 0**.

**Final-year project link:** signal × high-volatility dummy. $\beta_1$ is the calm-regime slope and $\beta_1 + \beta_3$ the volatile-regime slope. A significant $\beta_3$ means the signal behaves *differently* across regimes.

### 13.4 Scale

- Level–level: +1 unit of X adds a fixed amount.
- Log–log: +1% of X gives β%, a constant elasticity.

A \$1m sales increase can't plausibly have the same dollar effect on pay at a \$50m firm and a \$50bn firm, hence `lsales`. Using `sales` gave $R^2 = 0.13$ vs $0.28$.

### 13.5 RESET

Fit the model, add $\hat y^2$ and $\hat y^3$, and F-test them jointly. $\hat y^2$ contains every $X_j^2$ and $X_jX_k$, so one test checks many shapes at once.
- **Reject:** the conditional mean is incomplete.
- **But it doesn't say what's missing.** Theory, residual plots and targeted transformations decide.

```stata
regress lsalary roe lsales
estat ovtest
```
```r
resettest(m0)   # lmtest
```

**Naming trap.** Stata's `ovtest` ("omitted variables test") **cannot** detect omitted variables like managerial quality. It only checks functions of the included X's.

### 13.6 Open questions for revision

- [ ] Explain why $\sum\hat u_i = 0$ exactly, and why $\sum u_i \neq 0$ in a sample. Write $\hat u_i$ in terms of $u_i$ and the estimation errors.
- [ ] Derive $\operatorname{Corr}(x,\hat u) = 0$, naming which normal equation kills which term. What changes with `noconstant`?
- [ ] Give four reasons for a large residual other than a data error.
- [ ] Explain identification failure using the leverage/ROA two-story example.
- [ ] For each of the four threats to exogeneity, give the route by which X becomes correlated with u.
- [ ] Predict the short-regression coefficient in the simulation from the DGP parameters alone.
- [ ] Derive $\operatorname{Var}(\hat\beta_1) = \sigma^2/SST_x$ from the general formula, then write the robust version.
- [ ] Explain with the seesaw when robust SEs are larger or smaller than default SEs.
- [ ] State three things BP does not tell you, and why the Koenker version is preferred for finance data.
- [ ] Derive $\Delta Y$ for a step $h$ in the quadratic model and take $h\to0$.
- [ ] In an interaction model, derive the effect of switching D from 0 to 1 at a given X.
