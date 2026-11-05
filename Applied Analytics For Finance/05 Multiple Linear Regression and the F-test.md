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

## 8. Additions from the lecture notes (§2.1.6 — the MLR model and its assumptions)

**Model.** $Y_i=\beta_0+\beta_1X_{i1}+\dots+\beta_pX_{ip}+\varepsilon_i$. Each $\beta_j$ is the effect of $X_j$ **holding the other regressors fixed**. "Linear" means linear in the parameters, so $X^2$ or $\log X$ columns are allowed.

**Matrix form.** $Y=X\beta+\varepsilon$, where $X$ is $n\times(p+1)$ with a first column of ones. Row $i$: $Y_i=X_i'\beta+\varepsilon_i$. **Typo:** the notes write $Y=X'\beta$; it should be $Y=X\beta$ (the transpose belongs only on the single row $X_i'$).

**Assumptions ↔ SLR.**

| MLR | Says | SLR analogue |
|---|---|---|
| ML1 | Linear in parameters | SL1 |
| ML2 | Random sample / i.i.d. rows | SL2 |
| ML3 | $E(\varepsilon_i\mid X_i)=0$ | SL4 (zero conditional mean) |
| ML4 | Identifiability: $E(X_iX_i')=Q_{XX}$ positive definite; $\text{rank}(X)=p+1<n$ | SL3 (variation in $X$) |

**ML4 in words.** No regressor is an exact linear combination of the others. Otherwise $X'X$ is singular, $(X'X)^{-1}$ doesn't exist, and infinitely many $\beta$ fit equally well. Examples: the dummy trap (all dummies plus an intercept), or a constant column besides the intercept. You also need more observations than parameters ($n>p+1$).

**Positive definite vs positive semi-definite.**
- $Q$ is **positive semi-definite (PSD)** if $c'Qc\ge 0$ for every vector $c$. It is **positive definite (PD)** if $c'Qc>0$ for every non-zero $c$.
- For $Q_{XX}=E(X_iX_i')$: $c'Q_{XX}c=E[(c'X_i)^2]$. This is the average squared size of a weighted combination of the regressors.
- A squared quantity can't be negative, so $Q_{XX}$ is **always PSD**.
- **Exactly zero** for some $c\ne0$ means that combination $c'X_i$ is always 0. That is perfect collinearity: a direction in which the data carry no information.
- **PD** says every combination genuinely varies, so $X'X$ is invertible and $\hat\beta=(X'X)^{-1}X'Y$ exists.
- Analogy: $\text{Var}(Z)\ge 0$ always (PSD); $\text{Var}(Z)>0$ unless $Z$ is constant (PD). PD is the matrix version of "$X$ must vary" from SL3.

**Near collinearity (ML4 holds, barely).**
- If two columns are almost identical (e.g. RSI(14) and RSI(15)), $Q_{XX}$ is PD but close to singular, so $(X'X)^{-1}$ has huge entries.
- The data can't tell which variable deserves credit, so the **individual** coefficients are unstable. They have **inflated standard errors** and can even take opposite signs.
- Their **sum** (the combined effect) stays well estimated.
- Symptom: a high $R^2$ / significant overall F, yet no single t-stat is significant.
- Measured by the VIF; see the limitations section.

**Applications.**
- 10-yr and 30-yr yields in one regression → keep one, or use the spread.
- Fama-French factors are built to be roughly uncorrelated, partly for this reason.
- In ML feature sets, drop redundant indicators or use ridge regression, which adds $\lambda I$ to make $X'X$ safely PD.

## 9. Additions from the lecture notes (§2.1.7 — least squares in matrix form)

**Objective.** $S(\beta)=\sum_i(Y_i-X_i'\beta)^2=\varepsilon'\varepsilon=(Y-X\beta)'(Y-X\beta)$, and $\hat\beta=\arg\min_\beta S(\beta)$.

**Expansion.** $S(\beta)=Y'Y-\beta'X'Y-Y'X\beta+\beta'X'X\beta=Y'Y-2\beta'X'Y+\beta'X'X\beta$.
- The two middle terms are $1\times1$ scalars, and each is the transpose of the other, so they are equal and combine.

**Derivative.** Two matrix-calculus rules: $\partial(\beta'a)/\partial\beta=a$, and $\partial(\beta'A\beta)/\partial\beta=2A\beta$ for symmetric $A$. So
$$\frac{\partial S}{\partial\beta}=-2X'Y+2X'X\beta=0\;\Rightarrow\;X'X\hat\beta=X'Y\quad\text{(normal equations)}$$
- Under ML4, $X'X$ is invertible, so $\hat\beta=(X'X)^{-1}X'Y$.
- It is a minimum because the second derivative $2X'X$ is positive definite (again ML4).

**Intuition of the normal equations.** $X'(Y-X\hat\beta)=X'\hat e=0$: the residuals are orthogonal to every column of $X$.
- The ones column gives $\sum\hat e_i=0$.
- Each $X_j$ column gives $\sum X_{ij}\hat e_i=0$.
- These are the matrix version of the two sums-to-zero conditions from SLR (ch.04 §7).

**Hat matrix.** $\hat Y=X\hat\beta=X(X'X)^{-1}X'Y=HY$ (it "puts the hat on $Y$").
- $H$ is the **orthogonal projection** onto the column space of $X$. $\hat Y$ is the shadow of $Y$ on the space of all linear combinations of the regressors, and $\hat e$ is the perpendicular leftover.
- Properties: symmetric ($H'=H$) and idempotent ($HH=H$; projecting twice changes nothing). The same holds for $I-H$.
- $\hat e=(I-H)Y$ and $SSR=\hat e'\hat e=Y'(I-H)'(I-H)Y=Y'(I-H)Y$, which uses both properties.
- $H$ is $n\times n$, so it is used for theory, not computation. Software solves $X'X\hat\beta=X'Y$ via QR or Cholesky instead of inverting.
- The diagonal entries $h_{ii}$ are the **leverages**: how much $Y_i$ pulls its own fitted value.

**Typos on this page.**
1. $Y-X'\beta$ should be $Y-X\beta$.
2. The expansion term $-2\beta X'Y$ is missing a transpose: it should be $-2\beta'X'Y$.
3. The normal equation $(Y-X\hat\beta)X=0$ is dimensionally impossible ($n\times1$ times $n\times(p+1)$). It should be $X'(Y-X\hat\beta)=0$.

**Applications.**
- **Hedging/replication:** regressing a fund's returns on factor returns projects them onto the factor space. $HY$ is the replicable part; $(I-H)Y$ is the unhedgeable residual (its SD is the tracking error).
- **Influence diagnostics:** a large $h_{ii}$ flags observations like a crisis day that dominate the fit.
- **Code:** use `np.linalg.lstsq` or `statsmodels`, never `inv(X.T@X)`.

**Hat matrix idempotence (worked).** $HH=X\underbrace{(X'X)^{-1}X'X}_{I_{p+1}}(X'X)^{-1}X'=H$.
- The identity is $(p+1)\times(p+1)$, not $I_n$, and it stays in the middle (order matters).
- $H\neq I_n$, because $X$ is $n\times(p+1)$ and not square, so $X^{-1}$ doesn't exist. The fit space has only $p+1$ dimensions, so some of $Y$ is always left over as $\hat e$.

**Exercise: MLE of $\beta$ (worked).**
- Assume $\varepsilon\sim N(0,\sigma^2I_n)$. Then $\ell(\beta,\sigma^2)=-\frac n2\log(2\pi\sigma^2)-\frac{1}{2\sigma^2}(Y-X\beta)'(Y-X\beta)$.
- $\beta$ appears only in the second term, which carries a minus sign. So maximising $\ell$ over $\beta$ is the same as minimising $S(\beta)=(Y-X\beta)'(Y-X\beta)$.
- $\partial\ell/\partial\beta=\frac{1}{\sigma^2}X'(Y-X\beta)=0\Rightarrow\hat\beta_{MLE}=(X'X)^{-1}X'Y=\hat\beta_{OLS}$.
- It is a maximum because the second derivative is $-X'X/\sigma^2$, which is negative definite by ML4.
- **Interpretation:** the MLE is the $\beta$ under which the *observed data* are most probable. It is **not** "the probability that $\beta$ is the true value"; that would be a Bayesian posterior.
- The result relies on normality. With fat-tailed errors (e.g. Laplace) the MLE becomes least *absolute* deviations, not OLS.

**Why divide by $n-p-1$ (trace argument).**
- $E[SSR]=E[Y'(I-H)Y]=\sigma^2\,\mathrm{tr}(I-H)=\sigma^2(n-\mathrm{tr}H)$.
- $\mathrm{tr}H=\mathrm{tr}\big((X'X)^{-1}X'X\big)=\mathrm{tr}(I_{p+1})=p+1$, using the cyclic property of the trace.
- So $E[SSR]=\sigma^2(n-p-1)$ and $E[\hat\sigma^2_{MLE}]=E[SSR/n]=\sigma^2\frac{n-p-1}{n}$. That is biased **downward** by $\sigma^2(p+1)/n$.
- The unbiased estimator is $s^2=SSR/(n-p-1)$. In SLR ($p=1$) this is the familiar $n-2$.
- **Intuition:** each fitted coefficient "eats" one dimension of noise, so the residuals live in only $n-p-1$ dimensions.
- **Application:** an overfit model with many features makes $SSR/n$ look tiny, so risk is underestimated and positions are oversized.

## 10. Additions from the lecture notes (unbiasedness and variance of $\hat\beta$)

**Key decomposition.**
$$\hat\beta=(X'X)^{-1}X'(X\beta+\varepsilon)=\underbrace{(X'X)^{-1}X'X}_{I_{p+1}}\beta+(X'X)^{-1}X'\varepsilon=\beta+A\varepsilon,\qquad A=(X'X)^{-1}X'$$
- The substitution uses ML1. The collapse to $I_{p+1}$ uses ML4.
- **Reading:** the estimate is the truth plus a fixed linear mix of the errors. Every property of $\hat\beta$ comes from this line.

**Unbiasedness (worked).**
$$E(\hat\beta\mid X)=\beta+A\,E(\varepsilon\mid X)=\beta$$
- $\beta$ is a fixed parameter. $A$ depends only on $X$, so it is constant given $X$ and comes out **on the left** (matrix order matters).
- $E(\varepsilon\mid X)=0$ comes from ML3 plus ML2. ML3 is stated row by row, $E(\varepsilon_i\mid X_i)=0$. Independent rows (ML2) lift it to $E(\varepsilon_i\mid X)=0$ for the whole design.
- **Hierarchy:** independence $\Rightarrow$ mean independence $E(\varepsilon\mid X)=0$ $\Rightarrow$ orthogonality $E(X'\varepsilon)=0$. The proof needs the **middle** one. Independence is more than needed; orthogonality is not enough.
- **Counterexample:** $X\sim N(0,1)$, $\varepsilon=X^2-1$. Then $E(X\varepsilon)=0$ (orthogonal), but $E(\varepsilon\mid X)=X^2-1\ne0$.
- "Linear" means $\hat\beta=AY$ with $A$ built from $X$ only.

**Variance (worked).**
- Because $E(\hat\beta\mid X)=\beta$, $\text{Var}(\hat\beta\mid X)=E[(\hat\beta-\beta)(\hat\beta-\beta)'\mid X]$. **The variance proof uses the unbiasedness result.** Without it, this expression would be the MSE matrix.
- $(\hat\beta-\beta)(\hat\beta-\beta)'=A\varepsilon\varepsilon'A'$, since $(AB)'=B'A'$.
- $A'=X\big[(X'X)^{-1}\big]'=X(X'X)^{-1}$, using $(M^{-1})'=(M')^{-1}$ and the symmetry of $X'X$: $(X'X)'=X'(X')'=X'X$.
- Taking $E(\cdot\mid X)$, the $X$ parts are constant and come out on both sides:
$$\text{Var}(\hat\beta\mid X)=(X'X)^{-1}X'\,E(\varepsilon\varepsilon'\mid X)\,X(X'X)^{-1}$$
- **Spherical errors:** $E(\varepsilon\varepsilon'\mid X)=\sigma^2I_n$. This is $n\times n$, not $(p+1)\times(p+1)$; otherwise $X'(\cdot)X$ wouldn't conform.
  - **Diagonal:** $E(\varepsilon_i^2\mid X)=\text{Var}(\varepsilon_i\mid X)+[E(\varepsilon_i\mid X)]^2=\sigma^2+0$. This is **homoskedasticity**, a separate assumption.
  - **Off-diagonal:** $E(\varepsilon_i\varepsilon_j\mid X)=0$ for $i\ne j$. This is **no autocorrelation**; with independent rows (ML2) and ML3 it follows as $E(\varepsilon_i\mid X_i)E(\varepsilon_j\mid X_j)=0$.
  - $\varepsilon\varepsilon'$ (outer product, an $n\times n$ matrix of $\varepsilon_i\varepsilon_j$) is **not** $\varepsilon'\varepsilon=\sum\varepsilon_i^2$ (inner product, a scalar).
- **Cancellation:**
$$(X'X)^{-1}X'\,\sigma^2I_n\,X(X'X)^{-1}=\sigma^2(X'X)^{-1}\underbrace{(X'X)(X'X)^{-1}}_{I_{p+1}}=\sigma^2(X'X)^{-1}$$

**Intuition.** Noise ÷ information. $\sigma^2$ is how noisy the errors are. $(X'X)^{-1}$ shrinks with more observations, more spread-out regressors, and less overlap between them.
- The diagonal gives each $\text{Var}(\hat\beta_j)$. The off-diagonal gives the covariances between coefficients.
- With one regressor this reduces to the SLR result $\sigma^2/S_{xx}$ (ch.04).
- Without spherical errors you are stuck at the "sandwich" $(X'X)^{-1}X'\Omega X(X'X)^{-1}$, and the textbook formula is wrong.

**Lecturer's typos and gaps on this page.**
1. The labels "ML2a,b" and "ML3a" are never defined; §2.1.6 lists only ML1–ML4.
2. The variance proposition lists the same assumptions as unbiasedness, but the proof uses $E(\varepsilon\varepsilon'\mid X)=\sigma^2I$ (homoskedasticity), which is not among ML1–ML4.
3. The first line of the variance proof drops the conditioning: it should be $E[\,\cdot\mid X]$.
4. The step $(\hat\beta-\beta)(\hat\beta-\beta)'=A\varepsilon\varepsilon'A'$ with $A'=X(X'X)^{-1}$ is skipped.

**Applications.**
- **Look-ahead bias** (a feature that uses bar $t+1$ information) breaks $E(\varepsilon\mid X)=0$. The backtest coefficients are biased and the edge disappears live.
- **Omitted factors:** a single-factor beta is biased if an omitted factor correlated with the market sits in $\varepsilon$.
- **Beta precision:** $\text{SE}(\hat\beta)\approx\sigma_\varepsilon/(\sigma_m\sqrt n)$. Short windows in quiet markets give noisy betas and hedge ratios.
- **Volatility clustering** breaks homoskedasticity. $\hat\beta$ stays unbiased, but the reported SEs are wrong (usually too small), so you get overconfident factor exposures.
- **Collinear indicators** inflate $(X'X)^{-1}$, giving huge, sign-flipping coefficient uncertainty.

**Volatility clustering in the $E(\varepsilon\varepsilon'\mid X)$ grid (worked question).**
- **Diagonal:** this is what breaks. $E(\varepsilon_t^2\mid X)=\sigma_t^2$ changes with $t$ (calm periods, then wild ones), so it is no longer one common $\sigma^2$.
- **Off-diagonal:** this usually still holds. Returns, and so the errors, are close to uncorrelated: $E(\varepsilon_t\varepsilon_s\mid X)\approx0$. What is correlated is the **squares**, $\text{Corr}(\varepsilon_t^2,\varepsilon_s^2)>0$. That is a fourth-moment relationship and doesn't appear in this second-moment matrix.
- **Result:** $E(\varepsilon\varepsilon'\mid X)=\text{diag}(\sigma_1^2,\dots,\sigma_n^2)$, which is diagonal but not $\sigma^2I$.
- $\hat\beta$ is **still unbiased**, because that proof used only $E(\varepsilon\mid X)=0$. The variance becomes the sandwich, and $\sigma^2(X'X)^{-1}$ misstates it.
- **Common slip:** "clustering = correlation ⇒ off-diagonals non-zero". The correlation is in the magnitudes, not the signs.
- Non-zero off-diagonals come from **autocorrelated** errors instead, e.g. overlapping multi-day returns or a missing momentum term.

## 11. Additions from the lecture notes (§2.1.8 — efficiency, Cramér–Rao and the information matrix)

**Symbols.** $p$ = number of explanatory variables, not counting the intercept. $p+1$ = number of $\beta$'s. $p+2$ = all unknowns in $\theta=(\beta',\sigma^2)'$. $n$ = number of observations.

**Information from first principles (1-D case).**
- Model: $Y_i=\mu+\varepsilon_i$, with $\varepsilon_i\sim N(0,\sigma^2)$ and $\sigma^2$ known. Then $l(\mu)=\text{const}-\frac{1}{2\sigma^2}\sum(y_i-\mu)^2$. This is a downward parabola with its peak at $\bar y$.
- The likelihood answers "how probable is the data I saw, if $\mu$ were this value?" It is **not** the probability that $\mu$ is true.
- **Sharp peak** = the data pin $\mu$ down. **Flat peak** = many values fit almost equally well.
- Sharpness is the curvature: $l''(\mu)=-n/\sigma^2$.
- **Information** $\mathcal I=-E[l'']=n/\sigma^2$. The minus makes it positive at a maximum. The $E$ averages over datasets you could have drawn.
- **Why 1/information is the variance floor:** a new sample tilts the hill and slides the peak. Peak shift ≈ random tilt ÷ steepness $=\frac{\sqrt n/\sigma}{n/\sigma^2}=\frac{\sigma}{\sqrt n}$, so the variance is $\sigma^2/n=1/\mathcal I$.
- $\text{Var}(\bar y)=\sigma^2/n$ hits the floor, so $\bar y$ is efficient.
- **Check:** the intercept-only regression has $X=\mathbf 1_n$, $X'X=n$, $\hat\beta_0=\bar Y$ and $\sigma^2(X'X)^{-1}=\sigma^2/n$. The matrix formula is the 1-D case generalised, and $X'X/\sigma^2$ is "$n/\sigma^2$" for many directions.
- **Matrix version:** the Hessian holds the curvature in every direction. The off-diagonals show trade-offs between parameters; collinearity makes a flat ridge, so the variance in that direction is huge.

**Cramér–Rao.** Any unbiased $\tilde\theta$ satisfies $\text{Var}(\tilde\theta)\ge\mathcal I_n^{-1}$, with $\mathcal I_n=-E\big[\partial^2l/\partial\theta\partial\theta'\big]$. **Efficient** = attains the bound. (MVUE is slightly weaker: best among unbiased, but not necessarily on the bound.)

**The four blocks (worked).** With $l=-\frac n2\log2\pi-\frac n2\log\sigma^2-\frac{1}{2\sigma^2}(Y-X\beta)'(Y-X\beta)$ and $\partial l/\partial\beta=\frac{1}{\sigma^2}(X'Y-X'X\beta)$:
- **$\beta\beta$:** $\partial^2l/\partial\beta\partial\beta'=-X'X/\sigma^2$. This is non-random given $X$, so $-E$ gives $X'X/\sigma^2$.
- **Cross:** $\partial^2l/\partial\beta\partial\sigma^2=-\frac{1}{\sigma^4}(X'Y-X'X\beta)=-\frac{1}{\sigma^4}X'\varepsilon$ at the true $\beta$. Then $-E(\cdot\mid X)=\frac{1}{\sigma^4}X'E(\varepsilon\mid X)=0$ by **ML3** (+ML2). It is **not** "no autocorrelation".
  - **Rule: count the $\varepsilon$'s.** One → mean assumption (ML3). Two with different indices → no autocorrelation. Two with the same index → homoskedasticity.
- **$\sigma^2\sigma^2$:** write $s=\sigma^2$ and $S=(Y-X\beta)'(Y-X\beta)$, which contains no $s$. Then $l=c-\frac n2\log s-\frac S2s^{-1}$.
  - $l'=-\frac{n}{2s}+\frac{S}{2s^2}$ and $l''=\frac{n}{2s^2}-\frac{S}{s^3}=\frac{ns-2S}{2s^3}$.
  - $S=\varepsilon'\varepsilon=\sum\varepsilon_i^2$ is an **inner** product. $E(\varepsilon_i^2\mid X)=\text{Var}+\text{mean}^2=\sigma^2+0$, using homoskedasticity and ML3. No autocorrelation is needed (no cross-products), and normality isn't needed for this step. So $E(S\mid X)=n\sigma^2$.
  - $-E[l'']=-\frac{n\sigma^2-2n\sigma^2}{2\sigma^6}=\frac{n}{2\sigma^4}$.
  - This is the first block where $E$ does real work, because the curvature depends on the sample drawn.

**Assemble and invert.**
$$\mathcal I_n=\begin{bmatrix}X'X/\sigma^2&0\\0&n/2\sigma^4\end{bmatrix}\;\Rightarrow\;\mathcal I_n^{-1}=\begin{bmatrix}\sigma^2(X'X)^{-1}&0\\0&2\sigma^4/n\end{bmatrix}$$
- A block-diagonal matrix inverts block by block. Check: $\text{diag}(A,d)\,\text{diag}(A^{-1},1/d)=\text{diag}(I_{p+1},1)=I_{p+2}$.
- The scalar $\frac{1}{ad}\begin{pmatrix}d&0\\0&a\end{pmatrix}$ trick isn't valid when $a$ is a matrix: there is no "dividing by a matrix". Write $\mathcal I_n^{-1}$, never $1/\mathcal I_n$.
- **Conclusion:** the top-left block equals $\text{Var}(\hat\beta\mid X)$ from §10. OLS sits on the floor, so it is **efficient** under normality.
- The zero off-diagonal blocks mean information about $\beta$ and about $\sigma^2$ don't mix. Not knowing $\sigma^2$ costs nothing when estimating $\beta$.

**BLUE vs Cramér–Rao.** CR efficiency competes against **all** unbiased estimators but needs normality. BLUE (Gauss–Markov) competes only against **linear** unbiased estimators, but needs no normality.

**Lecturer's typos and gaps (§2.1.8).**
1. "Crammer Rao" should be Cramér–Rao.
2. $\mathbf I_n$ is used for both the information matrix and the $n\times n$ identity. $\mathbf I_{11}$ and $\mathbf I_{22}$ label blocks of the **inverse**.
3. "Assume ML1, ML2, ML3a, ML4. **Therefore** the errors are normal" is a non sequitur. Normality (and constant $\sigma^2$) are extra assumptions. The same gap appears in the distribution proposition.
4. $-2\beta X'Y$ should be $-2\beta'X'Y$.
5. "he log-likelihood" should be "the"; "$l==$"; $\partial\sigma^2\partial\sigma^2$ should be written $\partial(\sigma^2)^2$.
6. $\partial^2l/\partial\sigma^2\partial\beta'$ must be a $1\times(p+1)$ row, so the expression needs a transpose.
7. The $-E(\cdot)$ step and $\mathcal I_n$ itself are skipped; the notes jump straight to the inverse.
8. "Efficient = minimum variance unbiased" is loose: efficient means it attains the bound.

**Applications.**
- **Backtest length:** information ∝ $n$, so SE ∝ $1/\sqrt n$. Halving the SE of an edge needs 4× the data. Example: an edge of 0.05%/day with 1% daily vol over 250 days has SE ≈ 0.063%, which is larger than the edge.
- **Flat likelihood/optimiser surface** (e.g. EMA 18–30 all equal) means no information about that parameter. Picking the "best" value there is fitting noise.
- **Vol vs mean:** relative precision on $\sigma^2$ is far better than on the mean. This is why vol-based sizing is more robust than return-based sizing.
- **Software SEs** = square roots of the diagonal of $(-H)^{-1}$. Observed and expected Hessian can differ on samples with crashes.
- **Fat tails:** OLS is no longer on the CR floor, and LAD can beat it.

**Why $\hat\beta$ hits the floor exactly but $s^2$ doesn't (worked question).**
- $\partial^2l/\partial\beta\partial\beta'=-X'X/\sigma^2$ contains no $Y$, no $\varepsilon$ and no $\beta$. The curvature is the same for every dataset and at every $\beta$.
- **1-D view:** $l(\mu)=c-\frac{1}{2\sigma^2}\big[\sum y_i^2-2\mu\sum y_i+n\mu^2\big]$. The data set only the height ($\mu^0$ term) and position ($\mu^1$ term). The width ($\mu^2$ coefficient $-n/2\sigma^2$) has no data in it.
  - **Rigid stencil:** $\{4,5,6\}$ and $\{7,8,9\}$ give the same shape, with peaks at 5 and 8.
- Peak shift = tilt ÷ steepness. With fixed steepness this is exact, not approximate, so $\hat\beta=\beta+A\varepsilon$ and $\text{Var}=1/\mathcal I$ exactly.
- The $\sigma^2$ curvature $\frac{n}{2s^2}-\frac{S}{s^3}$ contains the data ($S$). Dividing by a random steepness adds wobble, so $s^2$ has $\text{Var}=2\sigma^4/(n-p-1)>2\sigma^4/n$ and misses its floor.

## 12. Additions from the lecture notes (Exercise — $\hat\beta$ is BLUE, matrix Gauss–Markov)

**Claim.** Among all estimators that are **linear** in $Y$ ($\tilde\beta=CY$, with $C$ built from $X$) and **unbiased**, OLS has the smallest variance.
- Assumptions: ML1–ML4 plus $E(\varepsilon\varepsilon'\mid X)=\sigma^2I_n$, i.e. homoskedastic and **uncorrelated** errors. **No normality and no independence needed.** That is the difference from Cramér–Rao (§11).

**"Smaller" for matrices.** $\text{Var}(\tilde\beta)\ge\text{Var}(\hat\beta)$ means the difference is PSD. So for **every** combination $c'\beta$ (a single coefficient, a spread, anything), OLS's variance is at least as small.

**Proof (worked).**
1. **Split** $C=A+D$ with $A=(X'X)^{-1}X'$ (OLS). This is the matrix version of SLR's $c_i=w_i+d_i$.
2. **Unbiasedness.** $\tilde\beta=CX\beta+C\varepsilon=\beta+DX\beta+(A+D)\varepsilon$, using $AX=I_{p+1}$ (not $I_n$). So $E(\tilde\beta\mid X)=\beta+DX\beta$, by ML3+ML2.
   - This must equal $\beta$ for **every** $\beta$, because the true $\beta$ is unknown. Plugging in the unit vectors kills each column, so $DX=0$ (equivalently $CX=I_{p+1}$).
   - $DX\beta=0$ for one lucky $\beta$ is not enough.
3. **Variance.** $\tilde\beta-\beta=C\varepsilon$, so $\text{Var}(\tilde\beta\mid X)=C\,E(\varepsilon\varepsilon'\mid X)\,C'=\sigma^2CC'$.
4. **Expand.** $CC'=AA'+AD'+DA'+DD'$.
   - $AD'=(X'X)^{-1}X'D'=(X'X)^{-1}(DX)'=0$, and $DA'=(AD')'=0$.
   - $AD'$ and $DA'$ are transposes of each other, not equal, so they **can't** be merged into $2AD'$.
   - $AA'=(X'X)^{-1}X'X(X'X)^{-1}=(X'X)^{-1}$.
5. **Result.** $\text{Var}(\tilde\beta\mid X)=\sigma^2(X'X)^{-1}+\sigma^2DD'=\text{Var}(\hat\beta\mid X)+\sigma^2DD'$.
6. **$DD'$ is PSD.** $c'DD'c=(D'c)'(D'c)=v'v=\sum_jv_j^2\ge0$, where $v=D'c$ and each $v_j=\sum_kD_{kj}c_k$ (a sum, not a single $d_ic_i$).
7. **Uniqueness.** A tie needs $DD'=0$. $\text{tr}(DD')$ is the sum of squares of all entries of $D$, so this forces $D=0$, i.e. $\tilde\beta=\hat\beta$. OLS is the **unique** BLUE.

**Intuition.** Any rival linear unbiased estimator = OLS + $D\varepsilon$, where $D\varepsilon$ is "blind" to $X$ ($DX=0$) and uncorrelated with the OLS part ($AD'=0$). Adding uncorrelated noise can only widen the spread.

**Applications.**
- BLUE holds with fat tails, which is why OLS betas and hedge ratios are defensible on non-normal returns. **Volatility clustering** (breaking equal variance) is the assumption that actually removes the "best" guarantee; then reweighting periods can beat OLS.
- Adding an unrelated random signal to a model can't reduce forecast variance. It's the same $+\sigma^2DD'$ penalty.

## 13. Additions from the lecture notes (distribution of the least squares estimators)

These results need **normal** errors, $\varepsilon\mid X\sim N(0,\sigma^2I_n)$. The notes list only ML1, ML2, ML3a and ML4, which is a gap.

**(a) $\hat\beta\mid X\sim N(\beta,\sigma^2(X'X)^{-1})$.**
- Fact: $M\varepsilon+b\sim N(b,\sigma^2MM')$. A linear map of a normal is normal.
- $\hat\beta=\beta+A\varepsilon$, so the mean is $\beta$ and the variance is $\sigma^2AA'=\sigma^2(X'X)^{-1}$. Normality upgrades "mean + variance" to the full distribution.

**(b) $(\hat\beta-\beta)'X'X(\hat\beta-\beta)/\sigma^2\sim\chi^2_{p+1}$.**
- Definition: $\chi^2_k=\sum_{j=1}^kZ_j^2$ with $Z_j$ i.i.d. $N(0,1)$.
- Identity: $z\sim N(0,\Sigma)$ ($k$-dim) $\Rightarrow z'\Sigma^{-1}z\sim\chi^2_k$. $\Sigma^{-1}$ "whitens" the correlated components into $k$ independent standard normals. 1-D: $(z/\sigma)^2\sim\chi^2_1$.
- Here $z=\hat\beta-\beta$, $k=p+1$, $\Sigma=\sigma^2(X'X)^{-1}$, so $\Sigma^{-1}=X'X/\sigma^2$, which is the **information matrix** (§11).
- Used for joint tests and confidence ellipsoids for all coefficients at once.

**(c) $\hat\beta$ is independent of $\hat\sigma^2$.**
- $\hat\beta-\beta=A\varepsilon$ (fitted part) and $\hat e=(I-H)\varepsilon$ (residual part).
- $\text{Cov}(A\varepsilon,(I-H)\varepsilon\mid X)=\sigma^2A(I-H)'=\sigma^2(A-AH)$, and $AH=(X'X)^{-1}X'X(X'X)^{-1}X'=A$, so it is $0$.
- Both are linear in the same normal $\varepsilon$, so they are jointly normal, and zero covariance ⇒ independent. (This step is only valid under joint normality.)
- $\hat\sigma^2$ ($SSR/n$ or $s^2=SSR/(n-p-1)$) is a function of $\hat e$ only, so it is independent of $\hat\beta$.
- **Why it matters:** in a $t$-ratio, the numerator ($\hat\beta_j$) and the denominator (built from $s^2$) are independent. That is what makes the ratio exactly $t$-distributed.

## 14. Additions from the lecture notes (§2.1.9 — ANOVA for MLR, overall F, $R^2$, adjusted $R^2$)

**Table.**

| Source | df | SS | MS |
|---|---|---|---|
| Regression | $p$ | SSReg | MSReg = SSReg$/p$ |
| Error | $n-p-1$ | SSR | MSE = SSR$/(n-p-1)$ |
| Total | $n-1$ | TSS | |

- The df add up just as the SS do: $p+(n-p-1)=n-1$.
- $\text{MSE}=s^2$, the unbiased estimate of $\sigma^2$ (§9).

**Overall F.** $H_0:\beta_1=\dots=\beta_p=0$ (the intercept is **not** tested) against $H_1$: at least one $\beta_j\ne0$.
$$F^*=\frac{\text{MSReg}}{\text{MSE}}\sim F_{p,\,n-p-1}\text{ under }H_0$$
- **Why it is $F$:** under $H_0$, $\text{SSReg}/\sigma^2\sim\chi^2_p$ and $\text{SSR}/\sigma^2\sim\chi^2_{n-p-1}$, and they are independent (§13c logic: fitted vs residual parts). A ratio of independent $\chi^2$/df is $F$, and $\sigma^2$ cancels.
- **Intuition:** explained variance per regressor ÷ noise variance per leftover df. Under $H_0$ it is ≈ 1; large means signal.
- Not rejecting means the best prediction is just $\bar Y$.

**$R^2$ and $F$ link.** Divide SSReg and SSR by TSS:
$$F^*=\frac{R^2/p}{(1-R^2)/(n-p-1)}$$

**$R^2=\text{SSReg}/\text{TSS}=1-\text{SSR}/\text{TSS}$** (requires an intercept). $R=\sqrt{R^2}=\text{corr}(Y,\hat Y)$, the multiple correlation.
- **$R^2$ never falls when a variable is added.** The real reason: OLS minimises SSR, and the bigger model could always set the new coefficient to 0. So SSR can't rise.

**Adjusted $R^2$.**
$$\bar R^2=1-\frac{\text{SSR}/(n-p-1)}{\text{TSS}/(n-1)}=1-(1-R^2)\frac{n-1}{n-p-1}$$
- It penalises each extra parameter and can fall, or even go negative.
- **Exact rule:** adding one variable raises $\bar R^2$ iff its $|t|>1$ (partial $F>1$), **not** iff it is significant at 5%.

**Lecturer's typos and gaps.**
1. The notation switches between SSRg/SSReg and RSS/SSR. Here RSS = SSR = residual SS.
2. "at at least one $j$".
3. The reason "$R^2$ never decreases" is given as "any variable has some correlation with $Y$". The real reason is that SSR is minimised over a larger set.
4. "A statistically insignificant variable will increase $R^2$ but decrease adj-$R^2$" is only roughly true. The threshold is $|t|=1$, not significance.

**Problem Set Q9 check (assume $\sum Y=352$).** $b_1=1.1716$, $b_0=0.8389$, $R^2=0.7973$, $F=983.6$.
- SLR has $p=1$, so $F=\frac{R^2}{(1-R^2)/(n-2)}$. Solving gives $n-2=983.6\times0.2027/0.7973\approx250$.
- So $n\approx252$. Check this against the question's $n$ to validate the $F$.

**Applications.**
- **Overall F = "does my bot's feature set predict anything at all?"** It protects against cherry-picking one significant indicator out of many.
- **$R^2$ inflation:** adding indicators always raises in-sample $R^2$. Use $\bar R^2$ (or out-of-sample tests) to choose features.
- A low $R^2$ (e.g. 0.02) can still be a tradable, significant edge with large $n$. Look at $F$/$t$, not $R^2$ alone.

## 15. Additions from the lecture notes (overall significance and the partial F-test)

**Overall F (restated).** $H_0:\beta_1=\dots=\beta_p=0$, equivalently $H_0:$ population $R^2=0$. The mean of $Y$ doesn't depend on any $X$.
$$F^*=\frac{\text{SSReg}/p}{\text{SSR}/(n-p-1)}=\frac{R^2/p}{(1-R^2)/(n-p-1)}$$
A significant $F$ means the observed $R^2$ isn't just chance.

**Partial F: does a group of extra variables add anything?**
- **Reduced model (R):** $p_R$ regressors. **Full model (F):** $p_F$ regressors, which contains R.
- $H_0$: the $p_F-p_R$ extra coefficients are all 0.
- $\text{SSR(R)}\ge\text{SSR(F)}$ always, because the full model could set the extras to 0 (same logic as "$R^2$ never falls").
- Even under $H_0$ the drop is $>0$, from sampling noise. The test asks whether the drop is **bigger than noise**.

**Correct statistic** (the notes have a typo; see below):
$$F^*=\frac{\big[\text{SSR(R)}-\text{SSR(F)}\big]/(p_F-p_R)}{\text{SSR(F)}/(n-p_F-1)}=\frac{(R_F^2-R_R^2)/(p_F-p_R)}{(1-R_F^2)/(n-p_F-1)}\sim F_{p_F-p_R,\;n-p_F-1}$$
- **Intuition:** the drop in error per extra variable ÷ the full model's noise per df. The denominator is always the **full** model's MSE.
- **$R^2$ form:** $\text{SSR}=\text{TSS}(1-R^2)$, and TSS cancels, so $\text{SSR(R)}-\text{SSR(F)}=\text{TSS}(R_F^2-R_R^2)$. This needs the same $Y$ and the same sample in both models.
- **Special case:** reduced = intercept only ($p_R=0$, $R_R^2=0$) gives back the overall F.
- **Special case:** one extra variable ($p_F-p_R=1$) gives $F^*=t^2$ for that coefficient.
- The p-value is the right tail.

**Lecturer's typos (important for the exam).**
1. **Denominator of the SSR form:** the notes write $\text{SSR(R)}/\text{SSR(F)}$. It should be $\text{SSR(F)}/df_F$ with $df_F=n-p_F-1$.
2. **Denominator of the $R^2$ form:** the notes write $R_F^2/(n-p_F-1)$. It should be $(1-R_F^2)/(n-p_F-1)$.
3. "statistically significance effect" should be "significant"; "$H_1:=$" should be "$H_1:$"; "altleast" should be "at least".
4. The garbled sentence "Even if $H_0$ were true, we would not expect the estimates of the extra parameters and the difference … = 0" should read: we would not expect the extra estimates to be exactly 0, nor $\text{SSR(R)}-\text{SSR(F)}=0$.
5. The example $H_0$ is written for $p=3$ specifically.

**Applications.**
- **Bot features:** test "do RSI + MACD + Stoch add anything beyond the trend filter?" as one partial F. Individually insignificant correlated indicators can be jointly significant.
- **Factor models:** CAPM (reduced) vs Fama–French 3-factor (full). The partial F asks whether SMB and HML jointly add explanatory power.

## 16. Additions from the lecture notes (model selection: F, AIC, BIC, adjusted $R^2$, log-likelihood)

**Formulas** ($\hat\sigma^2=\text{SSR}/n$, the MLE):
$$\text{AIC}=n\log\hat\sigma^2+2(p+1),\qquad \text{BIC}=n\log\hat\sigma^2+(p+1)\log n$$
- **Where they come from:** the maximised normal log-likelihood is $\ell_{\max}=-\frac n2\big(\log2\pi+1\big)-\frac n2\log\hat\sigma^2$. So $-2\ell_{\max}=n\log\hat\sigma^2+\text{const}$. AIC/BIC = $-2\ell_{\max}$ + a penalty, with the constant dropped.
- **Structure:** a **fit term** (falls as SSR falls) plus a **complexity penalty** (rises with parameters). Smaller is better.
- **BIC is stricter:** its penalty per parameter is $\log n$ vs 2, and $\log n>2$ once $n\ge8$ ($e^2\approx7.39$). So BIC picks smaller models.
  - **AIC** targets prediction accuracy.
  - **BIC** targets the true model and is consistent: as $n\to\infty$ it selects the true model.
- Only compare models fitted to the **same $Y$ and the same sample**. Only differences matter, not absolute values.
- **Log-likelihood and $R^2$:** larger is better, but both always improve with more parameters. Use them to compare same-size models, or use $\bar R^2$.
- **F vs AIC/BIC:** the partial F needs **nested** models. AIC/BIC can compare **non-nested** models too.

**Worked example** ($n=100$). A: $p=2$, SSR=50. B: $p=3$, SSR=48.
- $n\log\hat\sigma^2$: A $=100\ln0.5=-69.31$; B $=100\ln0.48=-73.40$.
- AIC: A $=-69.31+6=-63.31$; B $=-73.40+8=-65.40$. AIC picks **B**.
- BIC ($\ln100=4.605$): A $=-69.31+13.82=-55.49$; B $=-73.40+18.42=-54.98$. BIC picks **A**.
- They disagree because BIC's heavier penalty outweighs B's small SSR gain.

**Lecturer's typos.**
1. The BIC formula is labelled "AIC =".
2. "F test is used to test the significance of the entire model". For model *selection* the relevant tool is the **partial** F (nested comparison).
3. $\hat\sigma^2$ isn't defined. It is $\text{SSR}/n$ (MLE), not $s^2$.
4. The parameter count $p+1$ ignores $\sigma^2$. This is harmless for comparisons, because every model has one $\sigma^2$.

**Applications.** Choosing indicator sets for a bot: BIC resists overfitting more strongly. In noisy markets prefer BIC, or better, out-of-sample validation. AIC/BIC also let you compare non-nested models, e.g. momentum-based vs value-based return models.
