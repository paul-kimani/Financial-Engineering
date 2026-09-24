# Logistic Regression, Naive Bayes, and Classification

## 1. From regression to classification

[[04 Supervised Learning Vs unsupervised learning.|Regression]] predicts a *number*; **classification** predicts a *label/category* — up/down, default/no-default, fraud/legitimate. Directly applying linear regression to a 0/1 response (the **Linear Probability Model**, LPM) is tempting but flawed: predicted "probabilities" from an LPM aren't bounded to $[0,1]$ and can come out negative or above 1, which is nonsensical for a probability.

## 2. Logistic Regression

### The logistic function

Instead of modeling $\mathbb{P}(Y=1\mid X)$ linearly, logistic regression passes the linear predictor through the **logistic (sigmoid) function**, which squashes any real number into $(0,1)$:

$$p(X) = \mathbb{P}(Y=1\mid X) = \frac{e^{\beta_0+\beta_1X}}{1+e^{\beta_0+\beta_1X}}$$

### Odds and the logit

Rearranging gives the **odds**:

$$\frac{p(X)}{1-p(X)} = e^{\beta_0+\beta_1X}$$

Taking logs gives the **logit** (log-odds), which is linear in $X$ — this is the key trick that makes logistic regression a "generalized linear model":

$$\ln\left(\frac{p(X)}{1-p(X)}\right) = \beta_0+\beta_1X$$

### Estimation via Maximum Likelihood (MLE)

Unlike OLS (which has a closed-form solution), logistic regression coefficients are fit by **maximum likelihood**: choose $\hat\beta_0,\hat\beta_1$ to maximize the likelihood of observing the actual labels in the training data, given the model. This has no closed form, so it's solved iteratively — typically via **Newton-Raphson** (iteratively re-weighted least squares under the hood).

### The decision boundary

To classify, pick a threshold (commonly $0.5$) and predict $\hat Y=1$ if $p(X)>0.5$. Since the logit is linear in $X$, the resulting **decision boundary** — the set of points where $p(X)=0.5$, i.e. $\beta_0+\beta_1X=0$ — is a straight line (or hyperplane, with multiple predictors). This is the same "linear boundary" idea that Support Vector Machines start from in [[10 Support Vector Machines|ch.10]], just arrived at via a probabilistic model instead of a margin-maximization problem.

### Multiple logistic regression example (credit card approval)

```r
glm(formula = card ~ log(reports+1) + income + log(share) + age + owner + dependents + months,
    family = "binomial", data = CreditCard_clean)
```

Reading the coefficients table the same way as an `lm()` table (see [[05 Multiple Linear Regression and the F-test|ch.5]]) but interpreting each $\hat\beta_j$ as a **log-odds** effect: e.g. `log(reports+1)` coefficient $\approx-2.91$ means more derogatory reports sharply *decrease* the log-odds (and hence the probability) of card approval, holding other variables fixed. `income` and `log(share)` both have positive, highly significant coefficients — higher income and higher spending-to-income share both increase approval odds. Model fit for GLMs is judged via **deviance** (residual deviance vs. null deviance — analogous to SSR vs. TSS in OLS) and **AIC**, rather than $R^2$.

## 3. Naive Bayes Classifier

An alternative to logistic regression that applies Bayes' theorem directly. Setup:

- $K\;(K\ge2)$ — number of classes
- $\pi_k$ — prior probability a random observation is from class $k$
- $f_k(X) = \mathbb{P}(X\mid Y=k)$ — the density of $X$ for observations from class $k$

By Bayes' theorem, the **posterior probability**:

$$p_k(x) = \mathbb{P}(Y=k\mid X=x) = \frac{\pi_k f_k(x)}{\sum_{l=1}^K \pi_l f_l(x)}$$

Estimating $\hat\pi_k$ is easy (just the proportion of training observations in class $k$). The hard part is estimating $f_k(x)$ when $X$ is multi-dimensional — Naive Bayes makes this tractable via one key (and often unrealistic, hence "naive") assumption:

> **Within class $k$, the $p$ predictors are independent:**
> $$f_k(x) = f_{k1}(x_1)\times f_{k2}(x_2)\times\dots\times f_{kp}(x_p)$$

This turns a hard joint-density estimation problem into $p$ separate one-dimensional density estimation problems. The full posterior becomes:

$$p_k(x) = \frac{\pi_k\prod_{j=1}^p f_{kj}(x_j)}{\sum_{l=1}^K \pi_l\prod_{j=1}^p f_{lj}(x_j)}$$

**Estimating each $f_{kj}$:**
- If $X_j$ is quantitative: assume $X_j\mid Y=k\sim\mathcal N(\mu_{jk},\sigma_{jk}^2)$, or use a nonparametric estimate like the [[Kernel Density Function|Kernel Density Estimator]] (directly reusing the KDE machinery from [[03 Asset Returns.|ch.3]]).
- If $X_j$ is qualitative: just count the proportion of training observations in class $k$ for each category.

### Example: S&P 500 direction (Smarket data)

```r
nb.fit <- naiveBayes(Direction ~ Lag1 + Lag2, data = Smarket, subset = train)
nb.class <- predict(nb.fit, Smarket.2005)
table(nb.class, Direction.2005)
mean(nb.class == Direction.2005)   # 0.591 accuracy
```

Predicting market direction (Up/Down) from lagged returns — accuracy $\approx59\%$, modestly better than a coin flip, which is broadly consistent with weak-form market efficiency (past returns alone have only limited predictive power over future direction).

## 4. Logistic regression vs. Naive Bayes

| | Logistic Regression | Naive Bayes |
|---|---|---|
| Models directly | $\mathbb{P}(Y\mid X)$ | $\mathbb{P}(X\mid Y)$, then inverts via Bayes' theorem |
| Key assumption | Linear logit in $X$ | Predictors independent within each class |
| Fitting method | MLE (Newton-Raphson) | Simple proportions / density estimates per class |
| Boundary shape | Always linear in $X$ (or the logit-transformed features) | Can be non-linear, depending on the $f_{kj}$ chosen |

## 5. Open questions for revision

- [ ] Derive the logit-linearity claim explicitly: start from $p(X)=e^{\beta_0+\beta_1X}/(1+e^{\beta_0+\beta_1X})$ and show $\ln(p/(1-p))=\beta_0+\beta_1X$ algebraically.
- [ ] Compare the accuracy-only metric in the Smarket example against [[07 Model Assessment, Bias-Variance and Resampling#3. Classification model evaluation|the precision/recall/F-measure critique of accuracy]] — is 59% "accuracy" actually a meaningful number here, given the (roughly balanced) Up/Down class split?
- [ ] Work through why Naive Bayes' independence assumption, though usually false in practice (predictors in finance are rarely independent), still often gives a workable classifier — this is a famous, somewhat counterintuitive result worth understanding rather than memorizing.

---
*Source: BSF 3216 Lecture Two — "Supervised Learning" (Logistic Regression, Naive Bayes Classifier).*
