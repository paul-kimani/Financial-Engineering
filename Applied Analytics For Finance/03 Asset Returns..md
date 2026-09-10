# Asset Returns

## 1. Simple (net) returns

To build trading algorithms you must quantify price changes precisely. The simple net return $R_t$ between periods $t-1$ and $t$ is the percentage change in price:

$$R_t = \frac{P_t - P_{t-1}}{P_{t-1}}$$

If the asset pays a dividend $D_t$ during the period, add it to the numerator (it's cash you actually received, so it counts as part of the return):

$$R_t = \frac{P_t + D_t - P_{t-1}}{P_{t-1}}$$

The **gross return** is just $1$ plus the net return — it tells you the multiplicative growth factor of your money:

$$1 + R_t = \frac{P_t}{P_{t-1}}$$

## 2. Log (compound) returns

In quant finance we almost exclusively work with **log returns** instead of simple returns:

$$r_t = \ln(1+R_t) = \ln\!\left(\frac{P_t}{P_{t-1}}\right) = \ln P_t - \ln P_{t-1}$$

### Why log returns, not simple returns?

- **Time-additivity.** Simple returns don't add across periods (you have to multiply gross returns, which is clunky). Log returns *do* add:
$$r_{t-1,t+1} = \ln\frac{P_{t+1}}{P_{t-1}} = \ln\frac{P_{t+1}}{P_t}+\ln\frac{P_t}{P_{t-1}} = r_t + r_{t+1}$$
  This is why in the R code (see [[04 Supervised Learning Vs unsupervised learning.|ch.4]] and the regression notes) you'll always see `diff(log(data))` used to build the return series — it's just this additive property in one line.
- **Approximate symmetry for small moves.** For small $R_t$, $\ln(1+R_t)\approx R_t$, so log returns behave like simple returns most of the time, but don't have the awkward floor at $R_t=-1$ (price can't go negative) that simple returns do — log returns can range over all of $\mathbb{R}$, which plays nicer with models that assume normality (e.g. $X_j\mid Y=k \sim \mathcal N(\mu_{jk},\sigma_{jk}^2)$ in Naive Bayes, see [[09 Logistic Regression and Classification]]).
- **Better statistical behavior.** Log returns are closer to normally/symmetrically distributed than simple returns, which is part of why volatility models, factor models (see [[06 CAPM and Multifactor Models]]) and most of classical statistics are built on them.

> [!tip] Practical takeaway
> Whenever you see `X <- diff(log(data_set), na.pad = FALSE)` in the R workflows for this course, that line **is** the log return calculation above, applied to every column of a price/index matrix at once.

## 3. Distributional shape — why KDE matters here

Asset returns (log or simple) essentially never look like a clean textbook bell curve — real return series show **heavy tails** (extreme moves happen more often than a Normal distribution predicts) and **skewness** (up-moves and down-moves aren't symmetric). A plain histogram is too blocky and bin-choice-dependent to reveal this reliably, which is exactly the motivation for the [[Kernel Density Function|Kernel Density Estimator]]: it lets you see the true shape — heavy tails, skew, multimodality — of a return distribution with much higher fidelity.

## 4. Where this feeds into the rest of the course

Log returns are the *input* to almost everything else in this unit:
- The regression examples (CAPM beta, Fama-French factor loadings) are all fit on log-return series, not raw prices — see [[06 CAPM and Multifactor Models]].
- Bias-variance, cross-validation, and the classification/SVM material (see [[07 Model Assessment, Bias-Variance and Resampling]], [[09 Logistic Regression and Classification]], [[10 Support Vector Machines]]) are all model-fitting techniques that get applied *to* return series like these, or to derived features (predicting up/down direction, etc.) once you have them.

[[Kernel Density Function]]
#Kernel #density #returns
