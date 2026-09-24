# Multiple Linear Regression and the F-test

## 1. The multiple regression model

Extending [[04 Supervised Learning Vs unsupervised learning.|SLR]] to $p$ predictors:

$$Y_i = \beta_0 + \beta_1 X_{i1} + \beta_2 X_{i2} + \dots + \beta_p X_{ip} + \epsilon_i$$

where $p$ is the number of regressors. In matrix form, $Y = X\beta + \epsilon$, and OLS finds $\hat\beta$ minimizing the residual sum of squares.

## 2. Decomposing variance: SSR, TSS, SSReg

This is the core ANOVA identity behind regression inference — it splits the total variation in $Y$ into the part the model explains and the part it doesn't.

- **Total Sum of Squares (TSS):** total variation in $Y$ around its mean, $\text{TSS} = (Y-\bar Y)\cdot(Y-\bar Y)$, with $n-1$ degrees of freedom.
- **Sum of Squared Residuals (SSR):**\* the *unexplained* variation — how far actual $Y$ is from the model's fitted values $X\hat\beta$:
$$\text{SSR} = (Y-X\hat\beta)^\top(Y-X\hat\beta)$$
  with $n-p-1$ degrees of freedom (you lose one d.f. per estimated parameter, including the intercept).
- **Regression Sum of Squares (SSReg):** the *explained* variation — how much of TSS the model accounts for:
$$\text{SSReg} = \text{TSS} - \text{SSR}$$
  with $p$ degrees of freedom.

$$\text{TSS} = \text{SSReg} + \text{SSR}$$

> [!warning] Notation clash to watch for
> Some textbooks use **SSR** for the *regression* sum of squares and **SSE** for the *error/residual* sum of squares — the opposite of the convention above (which matches the unit's own lecture notation: SSR = residual, SSReg = regression). Always check which convention a given source is using before comparing formulas.

## 3. Mean squares and the F-statistic

Dividing each sum of squares by its degrees of freedom gives a *mean square*:

$$\text{MSReg} = \frac{\text{SSReg}}{p}, \qquad \text{MSE} = \frac{\text{SSR}}{n-p-1}$$

(MSE here is also written MC$\bar E$ / MC Reg in the raw lecture notation — same quantities.) The **F-statistic** for overall model significance is their ratio:

$$F = \frac{\text{MSReg}}{\text{MSE}} \sim F(p,\, n-p-1)$$

### Where the F-distribution comes from

This is worth understanding rather than memorizing, since it's the same machinery behind most classical hypothesis tests:

- If $X \sim \mathcal N(\mathbb{E}(X),\sigma^2)$, then the standardized variable $Z = \dfrac{X-\mathbb E(X)}{\sqrt{\operatorname{Var}(X)}} \sim \mathcal N(0,1)$ (**Standard Normal**).
- $Z^2 \sim \chi^2(1)$ (**Chi-squared** with 1 d.f.) — squaring a standard normal gives a chi-squared. The chi-squared distribution itself is a special case of the Gamma distribution.
- If $U \sim \chi^2(m)$ and $V \sim \chi^2(n)$ (independent), then:
$$\frac{U/m}{V/n} \sim F(m,n)$$
  This is literally the definition of the **F-distribution**: a ratio of two independent chi-squared variables, each normalized by its own degrees of freedom. MSReg and MSE are (under $H_0$) each proportional to independent chi-squared variables, which is why their ratio follows $F(p, n-p-1)$.

### The hypothesis test

$$H_0: \beta_1=\beta_2=\dots=\beta_p=0 \qquad \text{vs.} \qquad H_1: \beta_j\neq0 \text{ for at least one } j\in\{1,\dots,p\}$$

Decision rule (two equivalent forms):
- Using the **F-statistic**: reject $H_0$ if $F_{\text{statistic}} > F_{\text{critical}}$ (the F-table value at your chosen significance level $\alpha$ and $(p,n-p-1)$ degrees of freedom).
- Using the **p-value**: reject $H_0$ if $p\text{-value} < \alpha$.

Rejecting $H_0$ means: *at least one* predictor has real explanatory power — it does **not** tell you which one(s). For that you'd look at the individual t-tests on each $\hat\beta_j$ (as seen in the `summary(lm(...))` R output in [[06 CAPM and Multifactor Models]] — the `Coefficients:` table with individual `Pr(>|t|)` columns).

## 4. Coefficient of Determination: $R^2$ and Adjusted $R^2$

$$R^2 = \frac{\text{SSReg}}{\text{TSS}} = \frac{\text{TSS}-\text{SSR}}{\text{TSS}}, \qquad 0\le R^2\le 1$$

$R^2$ is the proportion of total variance in $Y$ explained by the model. Bigger is "better fit" — but $R^2$ has a critical flaw: it **never decreases** when you add more predictors, even useless ones (**nuisance variables** — predictors that only add spurious correlation to the response, with no real explanatory power). This makes plain $R^2$ a bad tool for comparing models with different numbers of predictors.

**Adjusted $R^2$** fixes this by penalizing for the number of predictors, via the degrees-of-freedom-normalized ratio:

$$R^2_{\text{adj}} = \frac{\text{SSReg}/p}{\text{TSS}/(n-1)} = 1 - \frac{\text{SSR}/(n-p-1)}{\text{TSS}/(n-1)}$$

Adding a genuinely useless predictor can actually *decrease* adjusted $R^2$ (because the d.f. penalty in the denominator outweighs the tiny SSReg gain), which is exactly the behavior you want from a model-comparison metric.

## 5. Variable and Model Selection

Once $p$ is large, using *all* available predictors is rarely wise — some add only noise (recall the **nuisance variables** warning above). **Variable selection** is the task of choosing a good subset $S\subseteq\{0,1,\dots,p\}$ of predictors, before ever getting to [[08 Regularization - Ridge and Lasso|Ridge/Lasso]]'s continuous-shrinkage alternative to this discrete search.

### Selection criteria

You can't just pick the subset with the highest $R^2$ — like plain $R^2$ itself, it never penalizes adding predictors. Criteria that *do* penalize model size, so smaller values indicate a "better" model:

$$\hat\sigma^2 = \frac{\sum_i(Y_i-\hat Y_i)^2}{n-2} \qquad(\text{residual variance estimate, SLR case})$$

$$\text{AIC} = n\log(\hat\sigma^2) + 2(1+p)$$

$$\text{BIC} = n\log(\hat\sigma^2) + \log(n)(1+p)$$

- **AIC** (Akaike Information Criterion) and **BIC** (Bayesian Information Criterion) both trade off fit (the $n\log(\hat\sigma^2)$ term — larger residual variance is worse) against complexity (the $p$-dependent penalty term). BIC's penalty, $\log(n)(1+p)$, grows faster with $n$ than AIC's flat $2(1+p)$, so **BIC penalizes larger models more harshly** and tends to pick more parsimonious models than AIC, especially as $n$ grows.
- **Adjusted $R^2$** — as above.
- **Best-subset selection** — literally fit *every possible subset* of predictors $\{0,1,2,\dots,p\}$ and pick whichever minimizes AIC/BIC (or maximizes adjusted $R^2$). Guaranteed to find the actual best subset for a given criterion, but computationally explosive: $2^p$ models to fit. Fine for small $p$, infeasible once $p$ gets large (say, $p>20$–$30$).

> [!example] Reading the notation
> $S_1=\{0,2,6,8,10\}$, $S_2=\{0,1,3,6,7,9\}$, $S_3=\{0,7,8\}$ — each $S_i$ is just a *candidate subset* of predictor indices (0 = intercept, always included) that best-subset selection would compare against every other candidate subset via AIC/BIC.

### Stepwise selection (the tractable alternative)

When $p$ is too large for best-subset ($2^p$ is infeasible, or when $p>n$ and best-subset can't even be computed), use a greedy search instead:

- **Forward stepwise selection** — start from the empty model (intercept only), and at each step add whichever remaining predictor most improves the criterion (e.g. largest drop in AIC/BIC, or the predictor whose F-statistic exceeds the critical value $F_{p,n-p-1}>CV_\alpha$ from [[05 Multiple Linear Regression and the F-test#3. Mean squares and the F-statistic|§3]]). Stop when no addition improves the model. Works even when $p>n$, unlike best-subset.
- **Backward stepwise selection** — start from the *full* model (all $p$ predictors) and remove the least useful predictor at each step. Requires $n>p$ to even fit the starting full model, so it's **not** usable when $p>n$ — this is the key practical tradeoff against forward selection.

Neither stepwise method is guaranteed to find the actual best subset (they're greedy, so an early bad choice can't be undone) — the tradeoff is tractability ($p$ or $p+1$ models fit total, not $2^p$) for a loss of the best-subset guarantee.

> [!tip] Context still matters
> A statistically "best" model by AIC/BIC isn't automatically the right model — check that the selected predictors are **contextually logical** and consistent with existing economic theory before trusting a purely algorithmic selection. A model that includes a predictor with no plausible causal or theoretical link to the response is a red flag, however good its AIC looks.

### Toy example: why the model-fitting step matters as much as selection

Take $X\sim\mathcal N(0,1)$ and the true relationship $Y=X^2$ (a deterministic, purely nonlinear relationship — no noise term at all). Note $\mathbb{E}(Y)=\mathbb{E}(X^2)=\operatorname{Var}(X)=1$ and $\operatorname{Cov}(X,Y)=\mathbb E(X^3)=0$ (odd moment of a symmetric distribution). If you naively regress $Y$ on $X$ with a **simple linear regression**, you'd find **zero correlation** and might wrongly conclude $X$ has no explanatory power over $Y$ — when in fact $X$ determines $Y$ *completely*, just through a nonlinear relationship a linear model is blind to. This is the sharpest possible illustration of why regression diagnostics (checking residual plots for structure, not just checking $R^2$/significance) matter — see [[12 Regression Diagnostics, Heteroskedasticity and GLS|ch.12]] for the full diagnostic toolkit this motivates.

## 6. Cheat sheet

| Quantity | Formula | Degrees of freedom | Meaning |
|---|---|---|---|
| TSS | $(Y-\bar Y)^\top(Y-\bar Y)$ | $n-1$ | total variation in $Y$ |
| SSR (residual) | $(Y-X\hat\beta)^\top(Y-X\hat\beta)$ | $n-p-1$ | unexplained variation |
| SSReg | TSS $-$ SSR | $p$ | explained variation |
| MSReg | SSReg$/p$ | — | avg. explained variance per predictor |
| MSE | SSR$/(n-p-1)$ | — | avg. residual variance |
| $F$ | MSReg/MSE | $F(p,n-p-1)$ | test of overall significance |
| $R^2$ | SSReg/TSS | — | fraction of variance explained (always $\uparrow$ with more predictors) |
| $R^2_{\text{adj}}$ | penalizes $R^2$ by $p$ | — | fair model-comparison metric |
| AIC | $n\log(\hat\sigma^2)+2(1+p)$ | — | penalized fit, for model/variable selection |
| BIC | $n\log(\hat\sigma^2)+\log(n)(1+p)$ | — | like AIC, penalizes model size more as $n$ grows |

## 7. Open questions for revision

- [ ] Derive $\text{Cov}(\epsilon_i,\epsilon_j)=0$'s role explicitly — re-derive why violating SL1 (autocorrelated errors, common in financial time series) invalidates the standard F-test's distributional assumptions.
- [ ] Practice reading an R `anova(lm(...))` table (see [[06 CAPM and Multifactor Models]]) and mapping each row/column back to the SSR/SSReg/F formulas above.
- [ ] Work through why $2^2\sim\chi^2(1)$ — this single identity is the seed of the whole $\chi^2\to F$ chain in §3.
- [ ] Given the $Y=X^2$ toy example, sketch what a residual-vs-fitted plot from the (wrongly) fit SLR would look like, and connect it to the diagnostic plots in [[12 Regression Diagnostics, Heteroskedasticity and GLS|ch.12]].

---
*Source: BSF 3216 lectures, 7 Sept 2026 and 11 Sept 2026 (ANOVA/F-test, variable/model selection).*
