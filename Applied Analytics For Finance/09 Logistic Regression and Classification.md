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

#### Building the likelihood function explicitly

Let $\beta=(\beta_0,\beta_1)$ so $p(X)=\dfrac{1}{1+e^{-\beta X}}$ (writing $\beta X$ for $\beta_0+\beta_1X$ for brevity). Since each observation's label $Y_i\in\{0,1\}$ is an independent Bernoulli draw with success probability $p(X_i)$, the **likelihood function** — the joint probability of observing the actual data, as a function of the unknown $\beta$ — is a product over all $n$ observations:

$$L(\beta) = \prod_{i:\,y_i=1} p(x_i) \;\times\!\! \prod_{i':\,y_{i'}=0}\! \big(1-p(x_{i'})\big)$$

That is: for every observation actually labeled $1$, multiply in $p(x_i)$ (the model's probability of that outcome); for every observation labeled $0$, multiply in $1-p(x_{i'})$. This is exactly the same Bernoulli-likelihood-product construction behind MLE for the Simple Linear Regression assumptions in [[04 Supervised Learning Vs unsupervised learning.#2. The Simple Linear Regression model SLR|ch.4's open MLE-vs-OLS question]] — the same tool, applied to a Bernoulli response instead of a Normal one.

Since products of many small probabilities underflow numerically and are painful to differentiate, take logs to get the **log-likelihood** $\ell(\beta)=\log L(\beta)$ — turning the product into a sum:

$$\ell(\beta) = \sum_{i:\,y_i=1} \log p(x_i) \;+\! \sum_{i':\,y_{i'}=0} \log\big(1-p(x_{i'})\big)$$

Substituting the logistic form $p(x)=1/(1+e^{-\beta x})$ (so $1-p(x) = 1/(1+e^{\beta x})$):

$$\ell(\beta) = \sum_{i:\,y_i=1} \log\!\left(\frac{1}{1+e^{-\beta x_i}}\right) \;+\! \sum_{i':\,y_{i'}=0} \log\!\left(\frac{1}{1+e^{\beta x_{i'}}}\right)$$

**MLE fitting is then**: find $\hat\beta = \arg\max_\beta \ell(\beta)$. Because $\ell(\beta)$ has no closed-form maximizer (unlike OLS's normal equations), this maximization is solved numerically via Newton-Raphson, iterating toward the $\hat\beta$ where $\ell$'s gradient is zero.

> [!tip] Sanity-check the direction
> A **larger** (less negative, or more positive) log-likelihood value means the model assigns higher probability to the labels actually observed — i.e. a *better* fit. Since probabilities are $\le1$, $\log p(x_i)\le0$ always, so $\ell(\beta)$ is typically negative — "larger" here means closer to zero, not necessarily positive.

### The decision boundary

To classify, pick a threshold (commonly $0.5$) and predict $\hat Y=1$ if $p(X)>0.5$. Since the logit is linear in $X$, the resulting **decision boundary** — the set of points where $p(X)=0.5$, i.e. $\beta_0+\beta_1X=0$ — is a straight line (or hyperplane, with multiple predictors). This is the same "linear boundary" idea that Support Vector Machines start from in [[10 Support Vector Machines|ch.10]], just arrived at via a probabilistic model instead of a margin-maximization problem.

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
