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
> Show that the OLS regression formulas for $\hat\beta_0,\hat\beta_1$ are the same estimators you'd get from Maximum Likelihood Estimation (MLE) under a normal-errors assumption — this is a standard, worthwhile derivation to do by hand once, since it's exactly the same MLE machinery used later for [[09 Logistic Regression and Classification|logistic regression]].

## 3. From simple to multiple regression

Real financial models almost always need more than one predictor — see [[05 Multiple Linear Regression and the F-test]] for the full multi-predictor model, the ANOVA decomposition (SSR/SSReg/TSS), and the F-test for overall significance, and [[06 CAPM and Multifactor Models]] for the concrete application (CAPM is literally an SLR; Fama-French is a multiple regression).

## 4. Where this sits in the unit

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
