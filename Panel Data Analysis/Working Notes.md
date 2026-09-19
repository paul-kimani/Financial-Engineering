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

---

## 2026-09-17 — Live Socratic session: ch.2 grounding, examples, and the differencing walkthrough

Closed-book recall prompts:

1. Using a City/Day/Temperature table, explain which column plays the role of "unit" and which plays "time," and what a single (unit, time) pair looks like.
2. A government survey has 500 households every year. Describe, in one sentence each, what would make this a pooled cross-section versus a true panel — and explain why a *single* year's wave, looked at alone, can never tell you which one it is.
3. Write the clean working definitions of pooled cross-section and true panel data, in your own words.
4. For the Grunfeld dataset ($N=10$ firms, $T=20$ years, balanced), compute the total number of rows, and explain what a specific $(i,t)$ pair like $i=3,t=1940$ identifies.
5. Starting from $Y_{i,t}=\beta_0+\beta_2X_{i,t}+\alpha_i+u_{i,t}$, write out Firm A's equation for 2020 and 2021, subtract them, and show exactly why $\alpha_A-\alpha_A=0$ is not an assumption. Which term from ch.1's omitted-variable-bias derivation does $\alpha_i$ play the role of, if you instead used a single cross-section of many firms?
6. Name the two distinct problems selective attrition creates (not one), and explain which of the two would still exist even if exit were pure random noise.

---

## 2026-09-17 — Live Socratic session: ch.3 between/within decomposition identity + worked example

Closed-book recall prompts:

1. Starting from $X_{it}-\bar X$, add and subtract $\bar X_i$ and regroup to derive the decomposition identity $X_{it}-\bar X=(X_{it}-\bar X_i)+(\bar X_i-\bar X)$. Explain in words why the first term is purely "within" and the second purely "between."
2. Why is this decomposition an algebraic identity rather than something that depends on any assumption?
3. For firms A and B with leverage $X$: A = (0.3, 0.5), B = (0.6, 0.7) over two years, compute the grand mean $\bar X$, both firm means $\bar X_A,\bar X_B$, and for Firm A Year 2 the between component, the within component, and verify they sum to $X_{A,2}-\bar X$ directly (not by assuming the parts are right).
4. In that worked example, the between and within components for Firm A Year 2 were $-0.125$ and $0.1$ — individually much larger than the overall deviation of $-0.025$. What does that gap already hint about how between and within variation can relate to each other?

---

## 2026-09-19 — Live Socratic session: ch.3 sign-flip mechanism (police/crime) + a documented near-miss

Closed-book recall prompts:

1. Explain, using $\alpha_i$ notation, why the between relationship between police spending and crime comes out positive across cities even though more police doesn't cause more crime.
2. Why does differencing (the within comparison) remove exactly the same $\alpha_i$ that contaminated the between comparison — connect this to the Firm A $\alpha_A-\alpha_A=0$ result from ch.2.
3. Explain why firm profitability and leverage do *not* produce a sign flip: state the between direction, the within direction, and the shared mechanism (pecking order) that makes them agree.
4. In general terms (not this specific example), what structural condition does a variable pair need to satisfy for the between and within relationships to genuinely differ in sign?

---

## 2026-09-19 — Live Socratic session: ch.3 SS-additivity (exact) derivation

Closed-book recall prompts:

1. Starting from $\text{SS}_{\text{overall}}=\sum_i\sum_t(a_i+b_{it})^2$ with $a_i=\bar X_i-\bar X$, $b_{it}=X_{it}-\bar X_i$, expand the square and identify the three resulting terms.
2. Prove $\sum_t(X_{it}-\bar X_i)=0$ for any unit $i$ directly from the definition of $\bar X_i$ — no reference to minimization needed.
3. Explain the deeper connection: why is that zero-sum fact "the same fact" as $\sum_i\hat u_i=0$ from ch.1's OLS normal equations, even though no regression was explicitly run here?
4. State the exact SS-additivity identity for a balanced panel, and explain precisely (two separate reasons) why the corresponding *standard deviations* don't simply add, even though the sums of squares do.
