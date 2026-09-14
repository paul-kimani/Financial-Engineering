# Clustered and Robust Standard Errors

## 1. Why panel residuals are rarely independent

Ordinary OLS standard errors assume the errors are independent across observations. In a panel, this is often implausible in two distinct ways:

$$\operatorname{Cov}(v_{it},v_{is}) \neq 0, \quad t\neq s \qquad\text{(within-unit, across time)}$$
$$\operatorname{Cov}(v_{it},v_{jt}) \neq 0, \quad i\neq j \qquad\text{(across units, within a period)}$$

The first says an unexplained shock hitting a firm can persist across the years it's observed — its residual in year $t$ is related to its own residual in year $s$. The second says a shock in a given calendar year (a financial crisis, a monetary policy shift, a common regulatory change) can hit many units at once, making residuals for *different* units in the *same* year move together.

> [!tip] Clustering changes inference, not the model
> Clustered/robust standard errors are entirely a fix to the **estimated covariance matrix** — they change how uncertain you say your coefficients are. They do **not** change the coefficients themselves, and critically, they do **not** remove omitted-variable bias, reverse causality, or correlation between a regressor and $\alpha_i$ ([[04 Pooled OLS and the Unobserved-Effects Model]]). A clustered standard error on a biased coefficient is a *more honest* standard error attached to a still-biased number.

## 2. One-way clustering by firm

Staying with the Grunfeld equation $\text{Investment}_{it}=\beta_0+\beta_1\text{Value}_{it}+\beta_2\text{Capital}_{it}+v_{it}$: firm $i$ is observed repeatedly, and if an unexplained shock affecting that firm persists across years, residuals in different years for the *same* firm may be related — $\operatorname{Cov}(v_{it},v_{is})\neq0$ for $t\neq s$.

**Clustering by firm** allows this within-firm correlation, together with heteroskedasticity, without specifying its exact functional form. The practical question to ask is: *which observations are likely to share unexplained firm-specific shocks?* If the answer is "observations belonging to the same firm," firm clustering is the natural starting point.

## 3. One-way clustering by year

Dependence can also run the other way — across firms, within a period. During a financial crisis, monetary tightening, or a regulatory change, different firms observed in the *same* calendar year may share unexplained shocks: $\operatorname{Cov}(v_{it},v_{jt})\neq0$ for $i\neq j$. **Clustering by year** allows residuals for different firms in the same year to be correlated, shifting the independence assumption across years rather than across firms — useful when common calendar shocks dominate whatever residual dependence remains.

## 4. One-way vs. two-way clustering

One-way clustering allows unrestricted dependence within *one* dimension. **Two-way clustering** allows two distinct dependence dimensions simultaneously — e.g. both within-firm serial dependence *and* within-year cross-sectional dependence — appropriate when both remain plausible after specifying the regression model.

| Clustering choice | Residual dependence allowed | Typical panel concern |
|---|---|---|
| Firm | within the same firm, across time | persistent firm-specific shocks |
| Year | across different firms, in the same year | common market or macro shocks |
| Firm and year (two-way) | both of the above | persistent firm shocks *plus* common calendar shocks |

> [!warning] Two-way clustering is not automatically superior
> The clustering choice must match the actual economic dependence structure, and there must be enough independent clusters in *each* dimension for the standard errors to behave well (see the few-cluster problem, §7). Defaulting to two-way clustering "to be safe" is not costless.

## 5. Clustering vs. hierarchy — a distinction worth never blurring

Many datasets have more than one natural level — years nested within firms, employees nested within branches, firms nested within countries. It is tempting to treat "hierarchical structure" and "clustering" as the same idea. They are not:

> [!warning] HIERARCHY ≠ CLUSTERING
> A **hierarchical or mixed-effects model** specifies how outcomes and random effects *vary across levels* — it is a statement about the conditional-mean model. **Cluster-robust standard errors** specify how *sampling uncertainty is calculated* — a statement about the covariance model only. The correct clustering level follows the source of dependence or the level at which a treatment is assigned — not simply the lowest-level observation unit.

> [!example] Choosing a clustering level
> Suppose a banking regulation changes at the country level and affects every bank in that country. Even with 2,000 bank-year observations, the independent policy variation may come from only 12 countries. Clustering by bank would allow serial correlation *within* banks but would fail to allow common regulation shocks *among different banks in the same country* — the correct level here follows the level at which the policy varies (country), not the observation unit (bank).

This generalizes to a rule: **level of observation ≠ level of treatment ≠ necessarily the appropriate level of clustering.** The clustering level should follow the source of residual dependence or the level at which the identifying treatment or policy varies.

## 6. Year effects and year clustering solve different problems

This distinction is fundamental, and easy to conflate because both involve "the year." Suppose 2008 caused investment to fall for nearly every firm — two genuinely different statistical problems can arise from this one fact:

1. **2008 may have shifted the average level of investment for all firms.** A **year indicator** (year effect) addresses this by adding a separate intercept for each year: $\text{Investment}_{it}=\beta_0+\beta_1\text{Value}_{it}+\beta_2\text{Capital}_{it}+\lambda_t+v_{it}$. The term $\lambda_t$ changes the **conditional-mean specification** — it absorbs shocks that affect all firms additively in a given year, such as a recession or a broad shift in financing conditions.
2. **Even after removing that common average shift, firms in 2008 may still have residuals that co-move.** **Year clustering** addresses this *remaining* covariance in the errors — it changes the **estimated covariance matrix**, not the fitted conditional mean.

| Device | What changes? | Problem addressed |
|---|---|---|
| Year indicators (year effects) | The regression specification, and potentially the coefficients | Common average level shifts across calendar years |
| Cluster by year | The estimated standard errors, not the coefficients | Residual correlation among units observed in the same year |
| Firm indicators / unit intercepts | The regression specification, and potentially the coefficients | Persistent level differences across firms |
| Cluster by firm | The estimated standard errors, not the coefficients | Residual correlation within a firm over time |

> [!tip] Key distinction
> Effects belong to the conditional-mean model; clustering belongs to the covariance model. Adding year indicators and clustering by year are therefore **not substitutes** — a specification may reasonably require both. At this stage, "firm effects" and "year effects" simply mean firm-specific and year-specific intercepts; the formal fixed-effects *estimator* (as opposed to this dummy-variable device) is developed in a later chapter.

## 7. The few-cluster problem

Cluster-robust inference relies on having a reasonable number of *independent* clusters — and the appropriate cluster need not be the same as the observation unit.

> [!example] Bank-year data, country-level regulation
> Consider bank-year data in which a regulation is implemented at the country level. Banks are the observation unit, but all banks in the same country may share the regulatory shock. Clustering only by bank would allow serial correlation within banks while still (incorrectly) treating different banks in the same country as independent. If a policy varies across only 12 countries, thousands of bank-year observations do **not** create thousands of independent policy clusters.

With few clusters, conventional cluster-robust standard errors can be **downward biased** — understating true uncertainty. Researchers should report the number of clusters and, when it is small, consider small-sample corrections or wild-cluster bootstrap methods.

## 8. Choosing a covariance estimator — the questions to ask

Before choosing a covariance estimator, ask:

1. Can errors persist within units over time?
2. Can units share shocks within a period?
3. Is treatment assigned at a higher level than the observation unit (e.g. country, industry)?

The answers determine whether firm, year, two-way, or higher-level clustering is defensible.

## 9. Reporting a pooled panel result

- Use pooled OLS as a clearly labelled **benchmark**.
- State that it blends cross-unit and within-unit variation, identify plausible persistent omitted variables, and report standard errors robust to the relevant dependence.
- Avoid writing "we used panel data; therefore endogeneity was controlled." Merely stacking repeated observations does not exploit the panel structure for identification (see [[04 Pooled OLS and the Unobserved-Effects Model]]).

## 10. Cheat sheet

| Question | Answer |
|---|---|
| What does clustering fix? | Standard errors / inference only |
| What does clustering NOT fix? | Omitted-variable bias, reverse causality, $\operatorname{Cov}(X,\alpha)\neq0$ |
| Cluster by firm | Allows within-firm serial correlation |
| Cluster by year | Allows within-year cross-unit correlation |
| Two-way clustering | Allows both simultaneously — needs enough clusters in each dimension |
| Level of observation vs. clustering | Not necessarily the same — follow the source of dependence / treatment assignment |
| Year effects vs. year clustering | Effects change the conditional mean; clustering changes the covariance matrix — not substitutes |
| Few-cluster problem | Downward-biased SEs with too few independent clusters |

## 11. Open questions for revision

- [ ] "Clustered standard errors solve the unobserved-heterogeneity problem" — diagnose exactly what is wrong with this statement.
- [ ] Complete the sentence from the Grunfeld lab: "Estimator choice determines __________; covariance estimation determines __________."
- [ ] A policy varies at the country-year level but the dataset contains thousands of firms. Defend an appropriate clustering strategy and discuss the small-number-of-clusters problem.
- [ ] Explain why year indicators and year clustering are not substitutes, using the 2008 example above in your own words.
- [ ] Re-estimate (conceptually) a pooled model with conventional vs. firm-clustered standard errors — which quantities are identical across the two, and which can change?

---
*Source: BSE 3211/4211 Week 2 "Panel Data Foundations — Pooled Models" lecture (PDF, pp.21–84).*
