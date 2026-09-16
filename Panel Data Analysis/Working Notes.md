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

---

## 2026-09-14 — Live Socratic session: ch.1 normal-equations derivation + OLS-as-projection

Closed-book recall prompts:

1. Write $\text{SSR}(\beta_0,\beta_1)=\sum_i(Y_i-\beta_0-\beta_1X_i)^2$ and derive both normal equations from scratch — $\sum_i\hat u_i=0$ and $\sum_iX_i\hat u_i=0$ — by taking $\partial\,\text{SSR}/\partial\beta_0$ and $\partial\,\text{SSR}/\partial\beta_1$ and setting each to zero.
2. From the first normal equation, derive $\hat\beta_0=\bar Y-\hat\beta_1\bar X$ — what one step (divide by what, then rearrange) gets you there?
3. Why are the normal equations "corollaries, not assumptions"? Give the ice-cream-sales-on-shark-attacks example in your own words and explain why it still satisfies $\sum_i\hat u_i=0$ exactly.
4. Describe the OLS-as-projection picture from memory: what is the "tabletop," what is $Y$, what is $\hat Y$, and why must $\hat u$ end up perpendicular to the tabletop?
5. Why does the true population error $u_i$ never appear anywhere in the projection diagram — what would you need to know to even attempt drawing it?
6. What's the precise difference between "OLS is trying to satisfy $X'\hat u=0$" and "$X'\hat u=0$ is the definition of what OLS's answer has to be"?

---

## 2026-09-16 — Live Socratic session: ch.1 omitted-variable-bias full derivation

Closed-book recall prompts:

1. Given the true model $Y_i=\beta_1+\beta_2X_i+\beta_3Z_i+u_i$ and the short regression $Y_i=\beta_1+\beta_2X_i+v_i$, derive $v_i=\beta_3Z_i+u_i$ by comparing the two equations.
2. What does the "short" in $\hat\beta_{2,\text{short}}$ actually mean? What's the other regression it's implicitly being contrasted with?
3. Starting from $\operatorname{Cov}(X,Y)=\operatorname{Cov}(X,\beta_1+\beta_2X+\beta_3Z+u)$, expand every term using linearity of covariance.
4. Two terms in that expansion vanish — $\operatorname{Cov}(X,\beta_1)$ and $\operatorname{Cov}(X,u)$. Explain precisely why each is zero, and why those are *different kinds* of reasons (identity vs. assumption).
5. Finish the derivation: divide $\operatorname{Cov}(X,Y)=\beta_2\operatorname{Var}(X)+\beta_3\operatorname{Cov}(X,Z)$ through by $\operatorname{Var}(X)$ and state the final OVB formula.
6. Under what two conditions does the omitted-variable bias term vanish completely, even though a variable was genuinely left out?
