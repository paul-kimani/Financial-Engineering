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

Substitute the true model's $Y_i$ into the short regression's slope formula, or equivalently note that $Z_i$ is absorbed into the short regression's error $v_i=\beta_3Z_i+u_i$. Regressing $Z$ on $X$ in an auxiliary regression, $Z_i=\delta_0+\delta_1X_i+\text{error}$, with $\delta_1=\operatorname{Cov}(X,Z)/\operatorname{Var}(X)$, gives the probability limit of the short-regression slope:

$$\operatorname*{plim}\hat\beta_{2,\text{short}} = \beta_2 + \beta_3\cdot\frac{\operatorname{Cov}(X,Z)}{\operatorname{Var}(X)}$$

The short-regression coefficient equals the *true* effect $\beta_2$ plus a **bias term**: the omitted variable's own effect $\beta_3$, scaled by how strongly the omitted variable moves with the included regressor.

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
