# Working Notes — Panel Data Analysis

Single running scratch file for active-recall writing across sessions. Add a new dated section per study session; don't split per chapter.

---

## 2026-09-14 — Chapters 01–06 built (Estimation Foundations through Running Examples)

Closed-book recall prompts — try these before checking the chapter notes:

1. Write out the chain: estimand → parameter → estimator → estimate → sampling distribution. Give a finance example for each link.
2. State the difference between $X'\hat u=0$ and $\mathbb E(Xu)=0$. Which one is guaranteed by OLS's own minimization, and which one is the substantive identifying assumption?
3. Derive the omitted-variable-bias formula $\operatorname{plim}\hat\beta_{2,\text{short}}=\beta_2+\beta_3\cdot\operatorname{Cov}(X,Z)/\operatorname{Var}(X)$ from the auxiliary regression of $Z$ on $X$.
4. What single test settles whether a dataset is a panel, a pooled cross-section, or something else? Explain why "has a date column" is not enough.
5. Write the between/within decomposition identity $X_{it}-\bar X=(\bar X_i-\bar X)+(X_{it}-\bar X_i)$ from memory, and explain in words what each term asks.
6. Construct a finance example where the between relationship between $X$ and $Y$ is negative but the within relationship is positive.
7. Write the unobserved-effects model and the panel error decomposition $v_{it}=\alpha_i+u_{it}$. What does it mean for $\operatorname{Cov}(X_{it},\alpha_i)\neq0$, and why doesn't more data fix it?
8. Define contemporaneous exogeneity, strict exogeneity, and predetermined regressors — give one example variable for each in a bank-profitability model.
9. State precisely what rejecting the Chow/F poolability test does and does not establish. Name two specific wrong conclusions students commonly draw from it.
10. Explain why "clustered standard errors solve the unobserved-heterogeneity problem" is wrong — what exactly does clustering change, and what does it leave untouched?
11. Explain the difference between adding a year indicator and clustering by year, using the 2008 shock example. Which one changes the conditional mean, and which changes the covariance matrix?
12. Why is "level of observation" not necessarily "the correct clustering level"? Work through the country-regulation/bank-year example.
13. Name the five-model progression this unit is building toward (cross-section, time series, pooled OLS, fixed effects, first differences), and say in one sentence what's structurally new about each of the last two relative to pooled OLS.
