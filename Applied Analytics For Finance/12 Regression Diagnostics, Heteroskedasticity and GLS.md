# Regression Diagnostics, Heteroskedasticity, and Generalized Least Squares

## 1. Why diagnostics matter

A regression can look statistically fine (significant coefficients, decent $R^2$) while quietly violating the [[04 Supervised Learning Vs unsupervised learning.#Classical assumptions SL1SL4|SL1–SL4 assumptions]] that made OLS trustworthy in the first place. Four classic failure modes:

- **Non-constant variance** (heteroskedasticity) — violates SL3.
- **Nonlinearity** of the true relationship — the $Y=X^2$ example in [[05 Multiple Linear Regression and the F-test#Toy example why the model-fitting step matters as much as selection|ch.5's toy example]] is the cleanest illustration: a linear model can completely miss a deterministic nonlinear relationship, and *residual plots* are how you'd actually catch it.
- **Correlation of the error terms** (autocorrelation) — violates SL1; the standard failure mode for time series data.
- **Outliers** — a handful of extreme observations can dominate the fit; whether to treat them as errors or genuine information is context-dependent ("context — best," in the lecture's own phrasing: there's no universal rule).

Financial time series in particular tend to show **heavy-tailed** residuals (rather than the clean Normal-looking scatter an OLS fit assumes) — visually, a residual-vs-fitted or residual-vs-time plot for financial data often shows *volatility clustering*: calm stretches followed by bursts of large residuals, rather than uniform scatter.

### Reading residual plots

- **Ideal case:** residuals $\hat\epsilon_i$ scattered with no pattern against fitted values $\hat Y_i$ — random noise, consistent with SL2/SL3 holding.
- **Heteroskedastic case:** residuals fan out (or in) as $\hat Y_i$ increases — variance is clearly not constant across the range of fitted values.
- **Time series case:** residuals plotted against time $t$ show trending, cyclical, or clustered behavior instead of white noise — a direct visual signal that SL1 (no autocorrelation) is failing.

## 2. Testing for autocorrelation: the Durbin-Watson test

The **Durbin-Watson statistic** tests whether the residuals behave like white noise (i.e. whether SL1 holds) — this is the formal version of "does the residual-vs-time plot look patterned." Values near 2 indicate no autocorrelation; values pulling toward 0 or 4 indicate positive or negative autocorrelation respectively.

## 3. Testing for heteroskedasticity: Breusch-Pagan and White tests

Formal tests for whether $\sigma_i^2$ actually varies with $i$ (violating SL3):

- **Breusch-Pagan test** — tests whether the *variance* of the residuals depends linearly on the predictors.
- **White test** — a more general version, doesn't assume a specific functional form for how variance depends on the predictors.

## 4. The covariance structure of the errors

Writing the error vector $\epsilon = (\epsilon_1,\epsilon_2,\dots,\epsilon_n)^\top$, the assumption set SL1–SL3 (independence + homoscedasticity) is compactly expressed as:

$$\operatorname{Var}(\epsilon) = \sigma^2 I$$

i.e. the covariance matrix $\Sigma=\operatorname{Var}(\epsilon)$ is **diagonal with equal entries** — every error has the same variance $\sigma^2$, and $\operatorname{Cov}(\epsilon_i,\epsilon_j)=0$ for $i\neq j$. This is the "ideal" case OLS is built for.

When this fails, $\Sigma$ becomes a general (not necessarily diagonal, not necessarily equal-variance) covariance matrix:

$$\operatorname{Var}(\epsilon_i)=\sigma_i^2 \text{ (arbitrary, } \sigma_i^2\neq\sigma_j^2\text{)}, \qquad \operatorname{Cov}(\epsilon_i,\epsilon_j)\neq 0 \text{ for } i\neq j$$

> [!example] Concrete example: a factor covariance matrix
> Modeling a corporate bond yield using factors like the risk-free reference rate (e.g. the 91-day T-bill rate), a 10-year Treasury bond rate, the exchange rate, and inflation, a plausible (illustrative) error covariance matrix might look like:
> $$\Sigma = \begin{pmatrix}20 & 0 & 0 & 0\\ 0 & 60 & 0 & 0\\ 0 & 0 & 100 & 0\\ 0 & 0 & 0 & 150\end{pmatrix}$$
> Even though this particular example is still diagonal (no cross-correlation between factors' error contributions), the *unequal* diagonal entries already violate SL3 (homoscedasticity) — this is exactly the situation GLS (§5) is designed to correct for.

## 5. Generalized Least Squares (GLS) and its variants

When $\Sigma\neq\sigma^2 I$, plain OLS is still *unbiased* but is no longer the *most efficient* (minimum-variance) estimator — Gauss-Markov's BLUE guarantee (see [[04 Supervised Learning Vs unsupervised learning.|ch.4]]) specifically requires $\Sigma=\sigma^2I$. **Generalized Least Squares (GLS)** fixes this by weighting observations according to the actual error covariance structure:

$$\hat\beta_{\text{GLS}} = (X^\top V^{-1}X)^{-1}X^\top V^{-1}Y$$

where $V$ is (proportional to) the error covariance matrix $\Sigma$ — note this generalizes the ordinary OLS normal equations $\hat\beta=(X^\top X)^{-1}X^\top Y$ exactly (OLS is the special case $V=I$). $V$ must be **invertible** for this formula to exist — this is why the rank condition $\operatorname{rank}(X)=p+1$ (full column rank) and an invertible $V$ both matter; a singular $V$ or a rank-deficient $X$ means the "correction" can't be computed.

Three named variants, depending on how much you know about $\Sigma$:

- **GLS** — the general case above; requires knowing (or estimating) the full covariance structure $V$.
- **WLS (Weighted Least Squares)** — the special case where $\Sigma$ is diagonal but with unequal variances (heteroskedasticity, no cross-correlation) — each observation just gets weighted by the inverse of its own variance.
- **FGLS (Feasible GLS)** — since the true $V$ is essentially never known in practice, FGLS first *estimates* $V$ (e.g. from OLS residuals) and then applies the GLS formula using that estimated $\hat V$. This is the version actually used in practice — plain GLS is a theoretical ideal that assumes you already know something you almost never know in advance.

For **time series data** specifically, the covariance structure of the errors is often modeled explicitly via an **AR($p$)** process (autoregressive of order $p$) — the errors themselves follow their own autoregressive structure, which is exactly the kind of correlation Durbin-Watson (§2) is designed to detect and FGLS-with-a-time-series-covariance-model is designed to correct for.

## 6. Cheat sheet

| Problem | Detection | Fix |
|---|---|---|
| Nonlinearity | Residual-vs-fitted plot shows curvature | Transform variables, or use a nonlinear model |
| Heteroskedasticity | Residual-vs-fitted plot fans out; Breusch-Pagan / White test | WLS, or heteroskedasticity-robust standard errors |
| Autocorrelation | Residual-vs-time plot shows pattern; Durbin-Watson test | GLS/FGLS with an AR($p$)-type covariance model |
| General $\Sigma\neq\sigma^2I$ | Any of the above | GLS (known $V$) or FGLS (estimated $\hat V$) |

## 7. Open questions for revision

- [ ] Derive why OLS remains unbiased but not minimum-variance when $\Sigma\neq\sigma^2I$ — connect this explicitly to the Gauss-Markov theorem's assumptions.
- [ ] Given the illustrative bond-yield $\Sigma$ matrix in §4, compute what the WLS weights would be for each of the four factors, and explain intuitively why the highest-variance factor (inflation, $\sigma^2=150$) gets down-weighted relative to the others.
- [ ] Connect the "heavy tails, volatility clustering" description of financial residuals back to [[03 Asset Returns.|ch.3]]'s discussion of why raw returns are almost never Normally distributed — is heteroskedasticity in a regression's residuals the "same phenomenon" as heavy tails in a return series, or a related-but-distinct one?

---
*Source: BSF 3216 lecture, 11 Sept 2026 (regression assumption violations, diagnostics, GLS/WLS/FGLS).*
