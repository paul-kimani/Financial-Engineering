# Supervised vs Unsupervised Learning; Simple Linear Regression

## 1. The core ML setup

For machine learning we assume there's a relationship $Y = f(X) + \epsilon$, where $f$ represents the *systematic* information $X$ provides about $Y$, and $\epsilon$ is random noise (measurement error, omitted variables, genuine randomness) with $\mathbb{E}(\epsilon)=0$ and independent of $X$.

- **Supervised learning:** you have both the inputs $X_i$ and the "correct answer" (response) $Y_i$, so the algorithm's guesses can be checked and corrected against ground truth. Regression (predicting a number) and classification (predicting a label — see [[09 Logistic Regression and Classification]]) are both supervised.
- **Unsupervised learning:** you only have inputs $X_i$, no $Y_i$, so the algorithm has to find hidden structure on its own (clustering, dimensionality reduction — not covered in depth yet in this unit's material).

## 2. The Simple Linear Regression model (SLR)

SLR models the relationship between a single predictor (e.g. a central bank rate) and a response (e.g. a bond yield). The population/sample equation:

$$Y_i = \beta_0 + \beta_1 X_i + \epsilon_i$$

where:
- $Y_i$ — the response (dependent) variable
- $X_i$ — the predictor (explanatory) variable
- $\beta_0,\beta_1$ — the parameters (intercept, slope) to be estimated
- $\epsilon_i$ — the error term; captures unmodeled variability, uncertainty in the relationship, and measurement error

### Classical assumptions (SL1–SL4)

- **SL1 (no autocorrelation):** the $\epsilon_i$ are independently distributed, $\operatorname{Cov}(\epsilon_i,\epsilon_j)=0 \;\forall i\neq j$. Financial time series famously *violate* this (returns/volatility cluster in time), which is exactly why cross-validation needs special care on financial data — see [[07 Model Assessment, Bias-Variance and Resampling#Why plain CV can fail on financial data]].
- **SL2 (zero mean error):** $\mathbb{E}(\epsilon_i) = 0,\; \forall i=1,\dots,n$ — on average, the model isn't systematically over- or under-predicting.
- **SL3 (homoscedasticity):** $\mathbb{E}(\epsilon_i^2) = \sigma^2$ (constant) for all $i$ — the error variance doesn't depend on $i$ or on $X_i$. (If this fails you have *heteroscedasticity*, which shows up a lot in finance — volatility itself varies over time.)
- **SL4 (implicit):** $X_i$ is treated as fixed/non-random relative to $\epsilon_i$, i.e. $\operatorname{Cov}(X_i,\epsilon_i)=0$ — otherwise OLS estimates are biased.

Under SL1–SL4, the Ordinary Least Squares (OLS) estimators $\hat\beta_0,\hat\beta_1$ are **BLUE** (Best Linear Unbiased Estimators, Gauss–Markov theorem) — the lowest-variance unbiased linear estimators you can build from the data.

> [!question] Open item (from the unit's own to-do)
> Show that the OLS regression formulas for $\hat\beta_0,\hat\beta_1$ are the same estimators you'd get from Maximum Likelihood Estimation (MLE) under a normal-errors assumption — this is a standard, worthwhile derivation to do by hand once, since it's exactly the same MLE machinery used later for [[09 Logistic Regression and Classification|logistic regression]]. (Note: this is a *different* derivation from the unbiasedness proof in §3 below — unbiasedness is a small-sample property that holds regardless of the error distribution; MLE-equivalence specifically requires assuming $\epsilon_i$ is Normally distributed.)

## 3. Proof: OLS is unbiased (the "U" in BLUE)

This proves the unbiasedness half of the Gauss-Markov "BLUE" claim above — worked through the way it's actually derived, not just asserted.

### Setting up: rewriting $\hat\beta_1$ as a weighted sum of $Y$

Start from the standard OLS slope formula:

$$\hat\beta_1 = \frac{\sum_i(X_i-\bar X)(Y_i-\bar Y)}{\sum_i(X_i-\bar X)^2}$$

Expand the numerator: $(X_i-\bar X)(Y_i-\bar Y) = (X_i-\bar X)Y_i - \bar Y(X_i-\bar X)$. Summing the second piece over $i$ gives $\bar Y\sum_i(X_i-\bar X)$, and $\sum_i(X_i-\bar X)=0$ always (deviations from a mean sum to zero, by definition of the mean) — so that term vanishes, leaving:

$$\hat\beta_1 = \frac{\sum_i(X_i-\bar X)Y_i}{\sum_j(X_j-\bar X)^2}$$

Now define $w_i = \dfrac{X_i-\bar X}{\sum_j(X_j-\bar X)^2}$. Every ingredient of $w_i$ is built purely from the $X_i$'s, which SL4 treats as fixed/non-random — so $w_i$ is a **fixed, known constant**, computable before you ever see $Y$. That makes:

$$\hat\beta_1 = \sum_i w_iY_i$$

a **weighted sum of the observed $Y_i$'s** — exactly why $w_i$ is called a "weight": it's the fixed recipe saying how much each $Y_i$ counts toward the final estimate. (Unlike a plain average's weights, the $w_i$ needn't be positive or sum to 1 — some observations pull the slope up, others down, depending on the sign of $X_i-\bar X$.) This weighted-sum form is also *precisely* what "linear estimator" means — $\hat\beta_1$ is a linear function of the data $Y$ with fixed coefficients $w_i$, which is the "L" in BLUE.

### Two properties of $w_i$ that do all the work

**Property 1: $\sum_i w_i = 0$.** The denominator $\sum_j(X_j-\bar X)^2$ is a single number, so it factors out of the sum over $i$:
$$\sum_i w_i = \frac{1}{\sum_j(X_j-\bar X)^2}\sum_i(X_i-\bar X) = \frac{0}{\sum_j(X_j-\bar X)^2} = 0$$
(the remaining sum is zero by the same "deviations from the mean" fact used above).

**Property 2: $\sum_i w_iX_i = 1$.** Same factoring move gives $\sum_i w_iX_i = \dfrac{\sum_i(X_i-\bar X)X_i}{\sum_j(X_j-\bar X)^2}$. The numerator needs one extra trick: write $X_i = (X_i-\bar X)+\bar X$, so
$$(X_i-\bar X)X_i = (X_i-\bar X)^2 + \bar X(X_i-\bar X)$$
Summing over $i$: the first piece gives $\sum_i(X_i-\bar X)^2$ (exactly the denominator); the second piece gives $\bar X\sum_i(X_i-\bar X) = \bar X\cdot 0 = 0$ (Property-1-style fact again). So the numerator equals the denominator, and $\sum_i w_iX_i = 1$.

### The unbiasedness argument itself

Substitute the true model $Y_i=\beta_0+\beta_1X_i+\epsilon_i$ into $\hat\beta_1=\sum_i w_iY_i$ and distribute across the three terms:

$$\hat\beta_1 = \sum_i w_i(\beta_0+\beta_1X_i+\epsilon_i) = \beta_0\underbrace{\sum_i w_i}_{=\,0} + \;\beta_1\underbrace{\sum_i w_iX_i}_{=\,1} + \sum_i w_i\epsilon_i = \beta_1 + \sum_i w_i\epsilon_i$$

This intermediate result is worth sitting with: your **estimate** $\hat\beta_1$ equals the **true** $\beta_1$ plus a leftover noise term $\sum_i w_i\epsilon_i$ — that noise term is the *only* thing separating your estimate from the truth.

Now take the expectation of both sides:

$$\mathbb{E}(\hat\beta_1) = \mathbb{E}\left(\beta_1+\sum_i w_i\epsilon_i\right) = \beta_1 + \sum_i w_i\,\mathbb{E}(\epsilon_i)$$

Pulling $w_i$ outside the expectation is only legal because $w_i$ is a **fixed constant, not a random variable** — and it's only fixed because **SL4** (independence of $X$ and $\epsilon$) holds; if $X$ and $\epsilon$ were correlated, $w_i$ (built from $X$) would be secretly entangled with the randomness in $\epsilon$, and this step would break. Then **SL2** ($\mathbb{E}(\epsilon_i)=0$ for every $i$) gives $\sum_i w_i\cdot 0 = 0$, so:

$$\mathbb{E}(\hat\beta_1) = \beta_1$$

**Unbiased** means exactly this: rerun the experiment on fresh samples over and over, and $\hat\beta_1$ averages out to the true $\beta_1$, with no systematic drift in either direction.

### The $\hat\beta_0$ case (same trick, mirrored)

The OLS intercept formula $\hat\beta_0=\bar Y-\hat\beta_1\bar X$ can similarly be rewritten as a weighted sum $\hat\beta_0=\sum_i v_iY_i$, with $v_i = \dfrac1n-\bar Xw_i$ (fixed constants, same reasoning as $w_i$). The mirror-image properties hold: $\sum_i v_i=1$ and $\sum_i v_iX_i=0$ (flipped from $w_i$'s $0$ and $1$). Substituting the true model exactly as above:

$$\hat\beta_0 = \sum_i v_i(\beta_0+\beta_1X_i+\epsilon_i) = \beta_0\underbrace{\sum_i v_i}_{=\,1} + \;\beta_1\underbrace{\sum_i v_iX_i}_{=\,0} + \sum_i v_i\epsilon_i = \beta_0+\sum_i v_i\epsilon_i$$

Taking expectations the same way (SL2 + SL4) gives $\mathbb{E}(\hat\beta_0)=\beta_0$ — $\hat\beta_0$ is unbiased too.

> [!tip] The one sentence version
> Both OLS estimators are just fixed-weight linear combinations of $Y$; the weights are built entirely from $X$ so they carry no randomness of their own, and once you substitute in the true model, only the $\mathbb E(\epsilon_i)=0$ assumption (SL2) is needed to make the noise term vanish in expectation — leaving exactly the true parameter behind.

## 4. From simple to multiple regression

Real financial models almost always need more than one predictor — see [[05 Multiple Linear Regression and the F-test]] for the full multi-predictor model, the ANOVA decomposition (SSR/SSReg/TSS), and the F-test for overall significance, and [[06 CAPM and Multifactor Models]] for the concrete application (CAPM is literally an SLR; Fama-French is a multiple regression).

## 5. Where this sits in the unit

```
Data Life Cycle (01) → Datasets (02) → Asset Returns (03)
        → SLR fundamentals (04, this note)
        → Multiple regression & F-test (05) → CAPM/Fama-French (06)
        → Model assessment, bias-variance, resampling (07)
        → Regularization: Ridge/Lasso (08)
        → Logistic regression & classification (09)
        → Support Vector Machines (10)
```

[[05 Multiple Linear Regression and the F-test]]
