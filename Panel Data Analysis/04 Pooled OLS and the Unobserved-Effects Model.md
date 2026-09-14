# Pooled OLS and the Unobserved-Effects Model

## 1. The pooled OLS model

The simplest way to use panel data is to ignore its panel structure almost entirely: stack every unit-period observation together and run one ordinary OLS regression across all of them.

$$Y_{it} = \beta_0 + X_{it}'\beta + v_{it}$$

Using the running example: $\text{Investment}_{it}=\beta_0+\beta_1\,\text{Value}_{it}+\beta_2\,\text{Capital}_{it}+v_{it}$ — the unit is the firm, the time index the year, and $\beta_1,\beta_2$ use **all** firm-year comparisons at once, mixing between- and within-firm variation together (see [[03 Sources of Variation - Between, Within and Overall]]).

Mechanically, pooled OLS is identical to running OLS on a plain cross-section — the estimator does not know or care that some rows share a unit. That is precisely its weakness.

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

> [!example] Classifying regressors in a bank-profitability model
> Bank size and capital ratio are plausibly closer to strictly exogenous or predetermined (slow-moving, not obviously reactive to this period's shock). Contemporaneous loan-loss provisions are a classic *endogenous* regressor — provisioning decisions are made in direct response to the same period's profitability shock. A national tax rate, set at the country level and not by any individual bank, is plausibly strictly exogenous with respect to bank-specific shocks.

## 5. Is pooling adequate? The Chow/F poolability test

Before deciding how to *handle* $\alpha_i$, a simpler, prior question: do the data even support one common intercept, or do unit-specific intercepts matter?

### Setup

Estimate the pooled model **augmented with unit indicators** (dummy variables, one per unit, using a reference-unit parameterization):

$$Y_{it} = \beta_0 + X_{it}'\beta + \delta_2D_{2i}+\delta_3D_{3i}+\dots+\delta_ND_{Ni} + \text{error}$$

$$H_0:\ \delta_2=\delta_3=\dots=\delta_N=0 \qquad\text{(all unit-specific intercept shifts are zero)}$$

Equivalently: $H_0$ says the common-intercept pooled model is adequate. In Stata, this is `reg invest mvalue kstock i.company` followed by `testparm i.company`; in R via **plm**, it's `pFtest()` comparing a within (fixed-effects) model against the pooled model.

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
