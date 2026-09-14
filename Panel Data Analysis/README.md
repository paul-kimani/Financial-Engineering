# Panel Data Analysis (BSE 3211/4211)

> Econometric methods for data that follow the same units repeatedly over
> time — from the estimation vocabulary that makes any regression
> interpretable, through what makes data a panel, sources of variation,
> pooled OLS and its unobserved-effects problem, and cluster-robust
> inference — building toward fixed effects, first differences, and
> random effects in later chapters.

---

## How to read these notes

Chapters are numbered in study order. Each is a self-contained reference
file. [[Working Notes]] is a single running scratch file for active-recall
writing — not split per chapter.

| #  | Chapter | Status |
| -: | :--- | :--- |
| 01 | [[01 Estimation Foundations]] — estimand/parameter/estimator/estimate, sample orthogonality vs. population exogeneity, identification vs. estimation, omitted-variable bias, heteroskedasticity/functional form/influence recap | Core |
| 02 | [[02 What Makes Data a Panel?]] — the same-unit-repeated test, the four data structures compared, panel notation, advantages, balanced/unbalanced panels and attrition bias | Core |
| 03 | [[03 Sources of Variation - Between, Within and Overall]] — the $X_{it}-\bar X=(\bar X_i-\bar X)+(X_{it}-\bar X_i)$ decomposition, worked two-firm example, why between and within relationships can differ in sign | Core |
| 04 | [[04 Pooled OLS and the Unobserved-Effects Model]] — pooled OLS's two failures, $v_{it}=\alpha_i+u_{it}$, $\operatorname{Cov}(X_{it},\alpha_i)\neq0$, exogeneity types, the Chow/F poolability test and what it does *not* establish | Core |
| 05 | [[05 Clustered and Robust Standard Errors]] — one-way/two-way clustering, hierarchy vs. clustering, year effects vs. year clustering, the few-cluster problem | Core |
| 06 | [[06 Running Examples - Grunfeld and World Bank]] — Stata/R code for both running examples, the five-model progression, what the Week 3 labs deliberately don't cover yet | Core |
| — | *Fixed effects, first differences, random effects* | Not yet written — next study session |

---

## Running examples used throughout this unit

- **Grunfeld panel (Stata)** — firm investment, market value and capital stock, 1935–1954. `xtset company year`. See [[06 Running Examples - Grunfeld and World Bank#1. Firm investment: the Grunfeld panel (Stata)|ch.6 §1]].
- **World Bank panel (R, `plm`)** — country-level credit depth, institutions and capital formation (WDI data). `pdata.frame(df, index = c("country","year"))`. See [[06 Running Examples - Grunfeld and World Bank#2. Country investment: the World Bank panel (R)|ch.6 §2]].

Both examples are walked through the same five-model progression: cross-section → time series → pooled OLS → *(later)* fixed effects → *(later)* first differences. See [[06 Running Examples - Grunfeld and World Bank#4. The five-model progression this unit builds toward|ch.6 §4]] for the full roadmap.

---

## Notational conventions

- Unit index $i=1,\dots,N$; time index $t=1,\dots,T$ (or $T_i$, if unbalanced).
- Panel error decomposition: $v_{it}=\alpha_i+u_{it}$ — $\alpha_i$ is the time-invariant unobserved effect, $u_{it}$ the idiosyncratic shock. See [[04 Pooled OLS and the Unobserved-Effects Model#3. The unobserved-effects model|ch.4 §3]].
- Grand mean $\bar X$ vs. unit mean $\bar X_i$ — see [[03 Sources of Variation - Between, Within and Overall#2. The decomposition identity|ch.3 §2]].
- This unit's regression/OLS vocabulary (estimand, estimator, $\hat\beta$, SSR) matches the conventions already established in [[../Applied Analytics For Finance/README|Applied Analytics For Finance]] — the two units share a single underlying OLS toolkit; this unit's chapters cross-link to that vault's ch.4/ch.5/ch.12 rather than re-deriving shared material.

---

## Cross-links to Applied Analytics For Finance

This unit assumes and builds on OLS material developed in the other course's vault, rather than re-deriving it:

- OLS mechanics, Gauss-Markov/BLUE, $\hat\sigma^2$ unbiasedness — [[../Applied Analytics For Finance/04 Supervised Learning Vs unsupervised learning.|ch.4]]
- The SSR/TSS/SSReg decomposition and variable selection — [[../Applied Analytics For Finance/05 Multiple Linear Regression and the F-test|ch.5]]
- Heteroskedasticity, HC standard errors, GLS/WLS/FGLS — [[../Applied Analytics For Finance/12 Regression Diagnostics, Heteroskedasticity and GLS|ch.12]]
