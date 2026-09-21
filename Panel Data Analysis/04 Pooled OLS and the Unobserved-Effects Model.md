# Pooled OLS and the Unobserved-Effects Model

## 1. The pooled OLS model

The simplest way to use panel data is to ignore its panel structure almost entirely: stack every unit-period observation together and run one ordinary OLS regression across all of them.

$$Y_{it} = \beta_0 + X_{it}'\beta + v_{it}$$

Using the running example: $\text{Investment}_{it}=\beta_0+\beta_1\,\text{Value}_{it}+\beta_2\,\text{Capital}_{it}+v_{it}$ — the unit is the firm, the time index the year, and $\beta_1,\beta_2$ use **all** firm-year comparisons at once, mixing between- and within-firm variation together (see [[03 Sources of Variation - Between, Within and Overall]]).

Mechanically, pooled OLS is identical to running OLS on a plain cross-section — the estimator does not know or care that some rows share a unit. That is precisely its weakness.

**Concretely: what "stacking" means.** There's no special panel machinery here. If you have 3 firms observed over 2 years, pooled OLS turns this into one flat table of $3\times2=6$ rows and runs completely ordinary OLS on it — the same normal equations from [[01 Estimation Foundations#Deriving the normal equations|ch.1]], just summed over all $NT$ rows instead of $N$:

| Row | Firm ($i$) | Year ($t$) | $Y_{it}$ | $X_{it}$ |
|---|---|---|---|---|
| 1 | 1 | 2020 | ... | ... |
| 2 | 1 | 2021 | ... | ... |
| 3 | 2 | 2020 | ... | ... |
| 4 | 2 | 2021 | ... | ... |
| 5 | 3 | 2020 | ... | ... |
| 6 | 3 | 2021 | ... | ... |

OLS minimizes $\sum(\text{row's }Y-\beta_0-\beta_1X)^2$ over all 6 rows, blind to the fact that rows 1–2 share a firm.

## 2. Two failures of naive pooling


### Failure 1: a single common intercept

Pooled OLS imposes **one** intercept $\beta_0$ for every unit. If units genuinely differ in their baseline level of $Y$ — a persistently more profitable firm, a structurally wealthier country — forcing them to share an intercept can distort the *slope* estimates too, not merely mislabel the intercept.

### Failure 2: persistent omitted traits

More seriously, pooled OLS's error term $v_{it}$ is silently absorbing every persistent, unit-specific trait that was left out of $X_{it}$ — three examples worth naming explicitly: managerial quality (firms), institutional quality (countries), and innate ability (households). If any such trait is correlated with $X_{it}$, this is exactly the omitted-variable-bias problem from [[01 Estimation Foundations#4. Omitted-variable bias|ch.1 §4]], now dressed in panel notation.

## 3. The unobserved-effects model

Panel data lets us be explicit about this second failure by **decomposing** the pooled error into two pieces:

$$v_{it} = \alpha_i + u_{it}$$

| Symbol | Meaning |
|---|---|
| $\alpha_i$ | the **unobserved effect** — a persistent, **time-invariant** trait of unit $i$ (does not carry a $t$ subscript) |
| $u_{it}$ | the **idiosyncratic error** — a transitory, unit-*and*-time-specific shock |

The full model is then:

$$Y_{it} = \beta_0 + X_{it}'\beta + \alpha_i + u_{it}$$

This single decomposition is the pivot the entire unit turns on: because $\alpha_i$ does not vary with $t$, repeated observations of the same unit create the *possibility* of eliminating it algebraically (by comparing a unit with itself, as fixed effects and first differences will do later) — something no single cross-section could ever offer.

### The central pooling problem, stated precisely

**What $\alpha_i$ is, precisely.** Unlike $X_{it}$, which competes for its own estimated coefficient $\beta$, $\alpha_i$ enters the *true* model with an implicit coefficient of exactly 1 — it's each unit's own permanent adjustment to the intercept, never observed and never included as a regressor in the pooled equation. Because pooled OLS doesn't include it, it doesn't disappear; it falls straight into the error term, which is exactly why $v_{it}=\alpha_i+u_{it}$.

**The derivation.** Take the true model with a single regressor for simplicity, $Y_{it}=\beta_0+\beta_1X_{it}+\alpha_i+u_{it}$ — structurally identical to ch.1's $Y_i=\beta_1+\beta_2X_i+\beta_3Z_i+u_i$ with $\beta_3\to1$ and $Z_i\to\alpha_i$. The pooled ("short") regression omits $\alpha_i$ entirely: $Y_{it}=\beta_0+\beta_1X_{it}+v_{it}$.

Pooled OLS's plim is a ratio, $\operatorname*{plim}\hat\beta_{1,\text{pooled}}=\operatorname{Cov}(X_{it},Y_{it})/\operatorname{Var}(X_{it})$. Substitute the true model into the numerator and expand by linearity of covariance:

$$\operatorname{Cov}(X_{it},Y_{it}) = \operatorname{Cov}(X_{it},\beta_0) + \beta_1\operatorname{Cov}(X_{it},X_{it}) + \operatorname{Cov}(X_{it},\alpha_i) + \operatorname{Cov}(X_{it},u_{it})$$

Four terms, exactly parallel to ch.1: $\operatorname{Cov}(X_{it},\beta_0)=0$ and $\operatorname{Cov}(X_{it},X_{it})=\operatorname{Var}(X_{it})$ are pure algebraic identities; $\operatorname{Cov}(X_{it},u_{it})=0$ is an assumption (contemporaneous exogeneity of the idiosyncratic shock); $\operatorname{Cov}(X_{it},\alpha_i)$ is left standing — it cannot be assumed away. So:

$$\operatorname{Cov}(X_{it},Y_{it}) = \beta_1\operatorname{Var}(X_{it}) + \operatorname{Cov}(X_{it},\alpha_i)$$

Dividing through by $\operatorname{Var}(X_{it})$ gives the full result:

$$\operatorname*{plim}\hat\beta_{1,\text{pooled}} = \beta_1 + \frac{\operatorname{Cov}(X_{it},\alpha_i)}{\operatorname{Var}(X_{it})}$$

— identical in shape to ch.1's $\beta_2+\beta_3\cdot\operatorname{Cov}(X,Z)/\operatorname{Var}(X)$, with $\alpha_i$ standing in for $Z_i$ and its implicit coefficient of 1 absorbed directly into the fraction.

Pooled OLS is safe **only if** $\operatorname{Cov}(X_{it},\alpha_i)=0$. If instead


$$\operatorname{Cov}(X_{it},\alpha_i)\neq0$$

then $\alpha_i$ is an omitted variable correlated with an included regressor, and by the OVB formula in [[01 Estimation Foundations#4. Omitted-variable bias|ch.1 §4]], pooled OLS's $\hat\beta$ is biased and inconsistent for the true structural parameter — no matter how large $N$ or $T$ get. More data does not fix this; only a different identification strategy (or a credible argument that $\operatorname{Cov}(X,\alpha)=0$) does.

> [!warning] Two classmates' mistakes, corrected
> "Because we now have panel data, omitted firm characteristics are no longer a problem" — **false**. Simply stacking repeated observations does not, by itself, remove $\alpha_i$; pooled OLS still puts $\alpha_i$ straight into the error term. Panel data only *creates the opportunity* to exploit repetition — an opportunity pooled OLS specifically declines to use.

## 4. Exogeneity concepts

Whether $X_{it}$ is safely uncorrelated with the error depends on *which* error — contemporaneous or across all periods — and this distinction matters even more once dynamic and predetermined regressors enter later estimators.

| Type | Condition | Reading |
|---|---|---|
| **Contemporaneously exogenous** | $\mathbb E(u_{it}\mid X_{it})=0$ | $X$ is unrelated to *this period's* shock only — says nothing about other periods |
| **Strictly exogenous** | $\mathbb E(u_{it}\mid X_{i1},\dots,X_{iT})=0$ | $X$ in *any* period is unrelated to the shock in *this* period — the strongest, full-history condition |
| **Predetermined** | $\mathbb E(u_{it}\mid X_{i1},\dots,X_{it})=0$ but not necessarily for future $X$ | past and current $X$ are unrelated to today's shock, but *future* $X$ may respond to today's shock (e.g. a lagged dependent variable) |
| **Endogenous** | $\mathbb E(u_{it}\mid X_{it})\neq0$ | correlated with the contemporaneous shock — the condition fails outright |

> [!example] Classifying regressors in a bank-profitability model, resolved
> The discriminating question for strict-vs-predetermined isn't "is $X$ related to *past* shocks" — it's "does *future* $X$ respond to *today's* shock?" And the general pattern-match for endogeneity: if the regressor is a decision made *in reaction to* the very shock in question, that's endogeneity, not exogeneity.
>
> - **Bank size — strictly exogenous.** Slow-moving and structural; no period's profitability shock (past, present, or future) has a real channel to move it.
> - **Lagged profitability — predetermined.** Today's shock $u_{it}$ affects today's profitability $Y_{it}$, which *becomes* tomorrow's regressor $X_{i,t+1}$ — future $X$ responding to today's shock, which is exactly what breaks strict exogeneity. But today's regressor (last period's profitability, already realized before today's shock occurred) can't be related to today's shock, so predetermined holds.
> - **Capital ratio — strictly exogenous.** Regulatory-driven and adjusted slowly; not set in reaction to a single period's earnings shock.
> - **Contemporaneous loan-loss provisions — endogenous.** Provisioning is a decision banks make *in direct response to* the same period's profitability shock — the shock determines the regressor, not the other way around. (Note: "strictly" only ever modifies *exogenous* — once contemporaneous exogeneity fails, it's just endogenous, with no further grading.)
> - **National tax rate — strictly exogenous**, and cleanly so: set at the country level, above any individual bank, so no bank-specific shock has any feedback channel into it in either direction.

## 5. Is pooling adequate? The Chow/F poolability test

Before deciding how to *handle* $\alpha_i$, a simpler, prior question: do the data even support one common intercept, or do unit-specific intercepts matter?

### Setup

Estimate the pooled model **augmented with unit indicators** (dummy variables, one per unit, using a reference-unit parameterization):

$$Y_{it} = \beta_0 + X_{it}'\beta + \delta_2D_{2i}+\delta_3D_{3i}+\dots+\delta_ND_{Ni} + \text{error}$$

$$H_0:\ \delta_2=\delta_3=\dots=\delta_N=0 \qquad\text{(all unit-specific intercept shifts are zero)}$$

Equivalently: $H_0$ says the common-intercept pooled model is adequate. Note this is a **joint $F$-test** on the $\delta_i$ coefficients — the same logic as testing joint significance of any group of regression coefficients — not a test involving $\operatorname{Cov}(X,\alpha)$ in any direct sense; covariance never enters the test statistic. In Stata, this is `reg invest mvalue kstock i.company` followed by `testparm i.company`; in R via **plm**, it's `pFtest()` comparing a within (fixed-effects) model against the pooled model.


### Interpreting the result

- **Reject $H_0$**: unit-specific intercepts matter; simple pooling is too restrictive.
- **Fail to reject**: no evidence against a single common intercept.

> [!warning] What the poolability test does NOT establish
> Rejecting $H_0$ tells you unit heterogeneity exists and matters — nothing more. It does **not**:
> - prove or establish $\operatorname{Cov}(X_{it},\alpha_i)\neq0$,
> - choose between treating $\alpha_i$ as **fixed** vs. **random** effects (that choice turns on exactly this correlation assumption, covered in a later chapter),
> - by itself fix anything — it is purely a diagnostic joint-significance test on intercept dummies.
>
> "The poolability test rejects, so we've proved $X$ is correlated with $\alpha_i$" is a common but incorrect inference — the test establishes that firms have persistent level differences *not captured by the included regressors alone*; it cannot distinguish a story where those differences are correlated with $X$ from one where they merely shift the intercept independently of $X$.

## 6. Cheat sheet

| Concept | Key fact |
|---|---|
| Pooled OLS model | $Y_{it}=\beta_0+X_{it}'\beta+v_{it}$, one common intercept |
| Unobserved-effects decomposition | $v_{it}=\alpha_i+u_{it}$ |
| Central risk | $\operatorname{Cov}(X_{it},\alpha_i)\neq0\Rightarrow$ biased, inconsistent pooled $\hat\beta$ |
| Contemporaneous exogeneity | $\mathbb E(u_{it}\mid X_{it})=0$ |
| Strict exogeneity | $\mathbb E(u_{it}\mid X_{i1},\dots,X_{iT})=0$ |
| Predetermined | past/current $X$ unrelated to $u_{it}$; future $X$ may react to it |
| Chow/F poolability test | tests whether unit-specific intercepts are jointly zero — a specification test, not an FE-vs-RE choice, not a test of $\operatorname{Cov}(X,\alpha)$ |

## 7. Open questions for revision

- [ ] State in one sentence why "we used panel data, therefore endogeneity was controlled" is wrong — what does merely stacking repeated observations fail to do?
- [ ] Give a coherent empirical story where the pooled coefficient on a regressor is larger than the true within-firm relationship, because of persistent cross-firm heterogeneity correlated with that regressor.
- [ ] Classify each variable in a bank-profitability model (bank size, lagged profitability, capital ratio, contemporaneous loan-loss provisions, national tax rate) as strictly exogenous, predetermined, or endogenous, and justify each classification.
- [ ] Explain precisely what rejecting the Chow/F poolability test does and does not establish — write both sides explicitly.
- [ ] What evidence, short of estimating a new panel estimator, should make you uncomfortable treating pooled OLS as your final model?

---
*Source: BSE 3211/4211 Week 2 "Panel Data Foundations — Pooled Models" lecture (PDF, pp.21–70) and "Panel data FENG 3y" (pptx).*
