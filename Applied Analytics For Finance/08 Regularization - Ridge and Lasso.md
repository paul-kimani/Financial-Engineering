# Regularization: Ridge Regression and the Lasso

## 1. Why regularize?

In [[05 Multiple Linear Regression and the F-test|multiple regression]], instead of doing model selection by hand (choosing which predictors to include via AIC, BIC, adjusted $R^2$, stepwise selection — see [[05 Multiple Linear Regression and the F-test#4. Coefficient of Determination R2 and Adjusted R2|ch.5 §4]]), you can instead fit a model containing **all** $p$ predictors, but *constrain* or *regularize* the coefficient estimates — shrinking them toward zero. This is a direct, practical application of the [[07 Model Assessment, Bias-Variance and Resampling#2. The Bias-Variance Decomposition|bias-variance tradeoff]]: shrinking coefficients deliberately introduces a little bias in exchange for a (usually much larger) reduction in variance.

The two standard techniques are **Ridge regression** and the **Lasso**.

## 2. Ridge Regression

Ordinary least squares minimizes the residual sum of squares:

$$\text{RSS} = \sum_{i=1}^n\left(Y_i-\beta_0-\sum_{j=1}^p\beta_jX_{ij}\right)^2$$

Ridge regression instead minimizes RSS **plus a penalty** on the size of the coefficients:

$$\sum_{i=1}^n\left(Y_i-\beta_0-\sum_{j=1}^p\beta_jX_{ij}\right)^2 + \lambda\sum_{j=1}^p\beta_j^2 = \text{RSS} + \lambda\sum_{j=1}^p\beta_j^2$$

where $\lambda\ge0$ is a **tuning parameter** (typically chosen via cross-validation, see [[07 Model Assessment, Bias-Variance and Resampling#4.1 Cross-Validation CV|ch.7 §4.1]]). The term $\lambda\sum\beta_j^2$ is the **shrinkage penalty**: small when the $\beta_j$ are close to zero, so minimizing the whole expression pulls coefficients toward zero.

### Key properties

1. Unlike OLS (one set of estimates), Ridge produces a **different set of estimates $\hat\beta^R_{j,\lambda}$ for every value of $\lambda$** — $\lambda$ effectively indexes a whole family of models.
2. The shrinkage penalty applies to $\beta_1,\dots,\beta_p$ but **not** to the intercept $\beta_0$ (there's no reason to shrink the baseline level toward zero).
3. $\hat\beta^R_{j,\lambda}$ depends on the **scaling** of predictor $j$ (and even on the scaling of the *other* predictors) — so **always standardize predictors before applying Ridge**, otherwise the penalty falls unevenly across variables just because of arbitrary unit choices (e.g. a variable measured in basis points vs. one measured in whole percent).
4. As $\lambda\uparrow$: flexibility $\downarrow$, so variance $\downarrow$ but bias $\uparrow$ — the bias-variance tradeoff made concrete. At $\lambda=0$, Ridge reduces exactly to OLS: high variance, no added bias.

### Worked toy example

Special case: $n=p$, $\mathbf X$ a diagonal matrix (1's on the diagonal, 0's off-diagonal), $\beta_0=0$. OLS then simplifies to minimizing $\sum_j(y_j-\beta_j)^2$, giving the trivial solution $\hat\beta_j=y_j$. Ridge instead minimizes $\sum_j(y_j-\beta_j)^2+\lambda\sum_j\beta_j^2$, giving:

$$\hat\beta^R_{j,\lambda} = \frac{y_j}{1+\lambda}$$

This is the cleanest possible illustration of "shrinkage": every OLS coefficient gets uniformly divided down by $(1+\lambda)$ — bigger $\lambda$, more shrinkage, but **never exactly zero** unless $\lambda=\infty$.

## 3. The Lasso

Ridge's main weakness: it never actually sets coefficients to *exactly* zero (unless $\lambda=\infty$), so it never really does variable selection — with many predictors this can hurt interpretability. The **Lasso** (Least Absolute Shrinkage and Selection Operator) fixes this by swapping the $\ell_2$ penalty for an $\ell_1$ penalty:

$$\sum_{i=1}^n\left(Y_i-\beta_0-\sum_{j=1}^p\beta_jX_{ij}\right)^2 + \lambda\sum_{j=1}^p|\beta_j| = \text{RSS} + \lambda\sum_{j=1}^p|\beta_j|$$

Same shrinkage idea, same interpretation of $\lambda$ — but the $\ell_1$ penalty has a qualitatively different effect: for $\lambda$ sufficiently large, it forces *some* coefficients to be **exactly zero**, effectively performing variable selection and producing **sparse models** (models using only a subset of the available predictors). This mirrors best-subset selection, but arrived at via a continuous optimization instead of a discrete search.

### Worked toy example (same setup as Ridge)

Same $n=p$, diagonal-$\mathbf X$, $\beta_0=0$ setup. The Lasso solution is a **soft-thresholding** rule:

$$\hat\beta^L_{j,\lambda} = \begin{cases} y_j-\lambda/2, & y_j>\lambda/2 \\ y_j+\lambda/2, & y_j<-\lambda/2 \\ 0, & |y_j|\le\lambda/2 \end{cases}$$

Compare directly against Ridge's $\hat\beta^R_{j,\lambda}=y_j/(1+\lambda)$: Ridge *shrinks proportionally* (never hits zero exactly), while Lasso *shrinks by a constant amount* $\lambda/2$ and **clips to exactly zero** once $|y_j|$ falls below the threshold. This one comparison is the cleanest way to remember the qualitative Ridge-vs-Lasso difference.

## 4. Comparing and choosing between them

Both Ridge and Lasso can be compared on MSE, variance, and bias of the resulting estimates — in addition to cross-validated test error. The tuning parameter $\lambda$ for either method is itself typically selected by cross-validation (fit across a grid of $\lambda$ values, pick the one minimizing CV error — same machinery as [[07 Model Assessment, Bias-Variance and Resampling#4.1 Cross-Validation CV|ch.7]]).

| | Ridge ($\ell_2$) | Lasso ($\ell_1$) |
|---|---|---|
| Penalty | $\lambda\sum\beta_j^2$ | $\lambda\sum|\beta_j|$ |
| Sets coefficients to exactly 0? | No | Yes, for large enough $\lambda$ |
| Performs variable selection? | No | Yes — produces sparse models |
| Best when... | many predictors each contribute a little | a few predictors truly matter, rest are noise |

## 5. Open questions for revision

- [ ] Derive the soft-thresholding Lasso solution above from scratch (it requires subgradient calculus since $|\beta_j|$ isn't differentiable at 0 — worth seeing once even informally).
- [ ] Given the AXP/HML example from [[06 CAPM and Multifactor Models|ch.6]], predict qualitatively what a Lasso fit across many candidate multifactor predictors might zero out, versus what Ridge would merely shrink.
- [ ] Practice explaining, in one sentence each, why standardizing predictors matters for Ridge but not for plain OLS.

---
*Source: BSF 3216 Lecture Two — "Supervised Learning" (Regularization: Ridge Regression, the Lasso).*
