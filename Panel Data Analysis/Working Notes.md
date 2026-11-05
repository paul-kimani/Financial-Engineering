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

---

## 2026-09-20 — Live Socratic session: ch.4 pooled-OLS-bias derivation + the stacking/alpha_i mental model

Closed-book recall prompts:

1. Explain concretely, with a small table (e.g. 3 firms x 2 years), what "pooled OLS" actually does to a panel dataset before running OLS — no special panel machinery involved.
2. Why doesn't $\alpha_i$ get its own estimated coefficient the way $X_{it}$ does? What is its implicit coefficient in the true model, and why does that make it fall into the error term when omitted?
3. Starting from $Y_{it}=\beta_0+\beta_1X_{it}+\alpha_i+u_{it}$, derive $\operatorname*{plim}\hat\beta_{1,\text{pooled}}=\beta_1+\operatorname{Cov}(X_{it},\alpha_i)/\operatorname{Var}(X_{it})$ by expanding $\operatorname{Cov}(X_{it},Y_{it})$ term by term and dividing through by $\operatorname{Var}(X_{it})$.
4. Under what single condition is pooled OLS consistent for $\beta_1$? Why doesn't increasing $N$ or $T$ fix a violation of that condition?

---

## 2026-09-21 — Live Socratic session: ch.4 exogeneity classification exercise + poolability test precision

Closed-book recall prompts:

1. Classify bank size, lagged profitability, capital ratio, contemporaneous loan-loss provisions, and national tax rate as strictly exogenous, predetermined, or endogenous, and justify each with the correct mechanism (not just the label).
2. What is the precise discriminating question between "strictly exogenous" and "predetermined" — not "is X related to past shocks," but what?
3. Why is "strictly endogenous" not a valid classification? What does "strictly" modify in this taxonomy, and why does endogenous have no further grading once it fails?
4. State $H_0$ for the Chow/F poolability test precisely, and explain why it is a joint F-test rather than a test involving $\operatorname{Cov}(X,\alpha)$ directly.
5. List, precisely, the three things rejecting the poolability test does NOT establish.

---

## 2026-10-19 — Restart session, Round 1: OVB (one-variable) + why differencing kills $\alpha_i$

Closed-book recall prompts:

1. Starting from $Y_i=\beta_0+\beta_1X_i+\gamma A_i+u_i$, expand $\operatorname{Cov}(X,Y)$ term by term and derive $\hat\beta_1\to\beta_1+\gamma\,\operatorname{Cov}(X,A)/\operatorname{Var}(X)$. State in words the two ingredients the bias needs.
2. Write Firm A's equation for 2020 and 2021, subtract, and list the three things that happen ($\alpha_A$ cancels, $\beta_0$ cancels, $\beta_1$ survives). Why is $\alpha_A-\alpha_A=0$ algebra, not an assumption?
3. Why does the OVB term become exactly zero in the differenced regression? (Write the omitted variable after differencing and plug it into the covariance.)
4. What price do you pay for differencing — which variables' effects can no longer be estimated, and why?
5. Differencing removes one source of bias but leaves a new condition. State it: what must be true of $\operatorname{Cov}(\Delta X,\Delta u)$, and what does that require about $X$ and $u$ across periods?

---

## 2026-10-25 — Week 1 paragraph: standard errors, estimator vs estimate + board-independence exercise

Material added as ch.01 §8. Closed-book recall prompts:

1. Why is the estimator random but the estimate fixed? What exactly has variance, and what doesn't?
2. We never draw repeated samples, so how does a formula give the SE? Derive $\operatorname{Var}(\bar Y)=\sigma^2/n$, name the assumption behind each step, then say what the one sample contributes.
3. Expand $\operatorname{Var}(Y_1+Y_2)$ without assuming independence. Where does the factor of 2 come from?
4. Derive $\operatorname{Var}(\bar Y)=\frac{\sigma^2}{n}[1+(n-1)\rho]$. For $\rho=0.3$, $T=10$, by how much is the naive SE too small?
5. Write $\hat\beta-\beta$ = bias + noise. Which part does the SE measure? Use $\text{MSE}=\operatorname{Var}+\text{Bias}^2$ to explain "precision is not validity".
6. Write a full estimand (treatment unit, population, outcome measure, timing) for "does board independence raise bank liquidity creation?"
7. Managerial quality is omitted. Sign each factor in $\gamma\delta$ and state the direction of bias. Why does an upward bias make a positive finding *less* convincing?
8. $\hat\beta=0.08$: identify parameter, estimator (name which one), estimate, and interpret the effect of a +10 pp increase.

**Spaced reviews:** 2026-10-26, 2026-10-28, 2026-11-01.

---

## 2026-11-02 — Week 1 lab + lecture §4–8: residuals vs errors, identification, OVB simulation, heteroskedasticity, functional form

Material added as ch.01 §9–13. Closed-book recall prompts:

1. Why do the residuals sum to *exactly* zero? Which first-order condition, and what breaks it?
2. Do the true errors sum to zero in a sample? Write $\hat u_i$ in terms of $u_i$, $(\hat\beta_0-\beta_0)$ and $(\hat\beta_1-\beta_1)$.
3. Derive $\operatorname{Corr}(x,\hat u)=0$ from the two normal equations. Why can't this test exogeneity?
4. Name the population condition violated when managerial quality is omitted and correlated with sales. Write $\operatorname{Cov}(\text{lsales},u)$ in terms of $\gamma$ and $\operatorname{Cov}(\text{lsales},q)$.
5. Why can't a high $R^2$ establish exogeneity? Give a high-fit/not-exogenous case and a low-fit/exogenous case.
6. State the leverage/ROA identification problem: two stories, same data.
7. List the four threats to exogeneity, each with its route to $\operatorname{Cov}(X,u)\neq0$.
8. From the simulation DGP alone, predict the short coefficient (≈ −0.068) and the correlation (≈ −0.57).
9. What happens to bias and to each model's SE when (a) $\delta=0$, (b) $\beta_2=0$?
10. Start from $\operatorname{Var}(\hat\beta_1)=\sum(x_i-\bar x)^2\sigma_i^2/SST_x^2$ and derive the default formula. What do robust SEs put in place of $\sigma_i^2$?
11. Using the seesaw, when is the robust SE bigger than the default, and when smaller? Which was ceosal1, and why?
12. Three things BP doesn't tell you. What does the Koenker version fix?
13. Derive $\Delta Y/h$ for the quadratic and take $h\to0$. Why is $\beta_1$ alone not "the effect"?
14. In $Y=\beta_0+\beta_1X+\beta_2D+\beta_3XD$, what is the slope for $D=1$? Why is $\beta_3$ not the effect for that group?
15. What does RESET detect, what doesn't it identify, and why is Stata's name `ovtest` misleading?

**Spaced reviews:** 2026-11-03, 2026-11-05, 2026-11-09.
