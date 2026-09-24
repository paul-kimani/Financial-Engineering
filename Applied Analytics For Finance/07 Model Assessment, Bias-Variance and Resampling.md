# Model Assessment, Bias-Variance Tradeoff, and Resampling

## 1. Model assessment vs. model selection

- **Model assessment:** evaluating a chosen model's performance on new (unseen) data.
- **Model selection:** choosing the right level of flexibility for a given model, or choosing between models entirely.

Both rely on estimating **test error** — how the model performs on data it wasn't fit on — as opposed to **training error**, which is trivially easy to compute (just apply the model to the data it was trained on) but is a badly optimistic, biased estimate of real-world performance.

## 2. The Bias-Variance Decomposition

For a fitted model $\hat f$ predicting $Y=f(X)+\epsilon$ at a test point $x_0$, the expected test MSE decomposes as:

$$\mathbb{E}\Big[(Y-\hat f(x_0))^2\Big] = \underbrace{\operatorname{Var}(\hat f(x_0))}_{\text{variance}} + \underbrace{\big[\text{Bias}(\hat f(x_0))\big]^2}_{\text{bias}^2} + \underbrace{\operatorname{Var}(\epsilon)}_{\text{irreducible error}}$$

- **Bias** — error from approximating a genuinely complex real-world relationship with a simplified model (e.g. fitting a straight line to a curved relationship). High-bias models are typically simple/inflexible (low-order polynomial regression, few predictors).
- **Variance** — how much $\hat f$ would change if fit on a *different* training sample. High-variance models are flexible enough to chase noise in the specific training data they saw (deep trees, high-degree polynomials, models with many parameters relative to $n$).
- **Irreducible error** ($\operatorname{Var}(\epsilon)$) — noise inherent to the problem; no model, however good, can reduce this below its floor.

### The tradeoff

As model flexibility increases: bias tends to fall (the model can represent more complex true relationships) but variance tends to rise (the model has more freedom to overfit the specific sample it saw). The goal is the flexibility level that minimizes their *sum* — this is exactly the theoretical justification behind **regularization** (§ below and [[08 Regularization - Ridge and Lasso]]), which deliberately increases bias a little in exchange for a larger drop in variance.

> [!tip] Where you'll see this again
> This is the single most recurring idea in the rest of the unit: Ridge/Lasso ([[08 Regularization - Ridge and Lasso]]) explicitly trade bias for variance via $\lambda$; cross-validation (§4) exists specifically to *estimate* test error so you can find the flexibility level that balances the two; SVM's tuning parameter $C$ ([[10 Support Vector Machines]]) plays the same role for margin width vs. robustness.

## 3. Classification model evaluation

For classification (see [[09 Logistic Regression and Classification]]), performance is judged via a **confusion matrix**: $f_{ij}$ = number of records truly in class $i$ that the model assigned to class $j$. By convention the class of interest is the **positive** class, the rest **negative**.

| Metric | Formula | Meaning |
|---|---|---|
| Accuracy | $\dfrac{f_{11}+f_{00}}{f_{11}+f_{10}+f_{01}+f_{00}}$ | fraction of all predictions that were correct |
| Error rate | $\dfrac{f_{10}+f_{01}}{f_{11}+f_{10}+f_{01}+f_{00}}$ | fraction of predictions that were wrong ($=1-$accuracy) |
| Precision | $\dfrac{\text{TP}}{\text{TP}+\text{FP}}$ | of predicted-positives, what fraction were actually positive |
| Recall | $\dfrac{\text{TP}}{\text{TP}+\text{FN}}$ | of actual positives, what fraction did the model catch |
| F-measure | $\dfrac{2\cdot\text{Precision}\cdot\text{Recall}}{\text{Precision}+\text{Recall}}$ | harmonic mean, balances precision & recall |

**Why accuracy alone can mislead:** with **class imbalance** (e.g. rare fraud cases, rare default events — common in finance), a model that just predicts "no fraud" every time can have very high accuracy while being completely useless. This is exactly why precision/recall/F-measure matter more than raw accuracy in most real financial classification problems.

## 4. Resampling methods

Resampling means repeatedly drawing samples from a training set and refitting a model on each, to learn more about the fitted model than a single fit could tell you (e.g. how variable its estimates really are). Both cross-validation and bootstrap assume the data is drawn from an **IID process** — a condition worth flagging immediately, since §4.4 below explains why that assumption often fails for financial data.

### 4.1 Cross-Validation (CV)

CV holds out part of the training data to *simulate* test-error estimation, since a genuine held-out test set often isn't available.

**a) The Validation Set approach.** Randomly split the data into a training set and a validation (hold-out) set; fit on training, evaluate (via MSE, for a quantitative response) on validation. Simple, but the resulting error estimate is highly variable (depends heavily on the random split) and tends to *overestimate* the true test error rate, since the model is trained on less data than it otherwise would be.

**b) Leave-One-Out CV (LOOCV).** Use a single observation $(x_1,y_1)$ as validation, fit on the remaining $n-1$; repeat for every observation. Since $(x_1,y_1)$ wasn't used in fitting, $\text{MSE}_1=(y_1-\hat y_1)^2$ is an approximately unbiased test-error estimate. Average over all $n$:

$$\text{CV}_n = \frac{1}{n}\sum_{i=1}^n \text{MSE}_i$$

For classification, the analogous error-rate version uses the indicator $\text{Err}_i = I(y_i\neq\hat y_i)$:

$$\text{CV}_n = \frac{1}{n}\sum_{i=1}^n \text{Err}_i$$

**c) k-Fold CV.** Randomly split into $k$ roughly equal folds; treat each fold in turn as the validation set, fitting on the other $k-1$ folds, computing $\text{MSE}_i$ on the held-out fold:

$$\text{CV}_k = \frac{1}{k}\sum_{i=1}^k \text{MSE}_i$$

LOOCV is the special case $k=n$. In practice $k=5$ or $k=10$ is standard — a good middle ground between the high variance of the validation-set approach and the high computational cost of LOOCV (which requires fitting the model $n$ times).

CV is generally used both to estimate test error *and* to tune hyperparameters (e.g. choosing $\lambda$ in Ridge/Lasso, or $C$ in SVM).

### 4.4 Why plain CV can fail on financial data

Two specific reasons, both worth remembering as exam-ready "gotchas":

1. **Violated IID assumption.** Financial data is serially correlated — $\rho(X_t,X_{t-1})\neq0$ — and adjacent observations can be nearly identical ($Y_t\approx Y_{t+1}$). Randomly splitting into folds can put near-duplicate, time-adjacent observations into both the training and validation sets, letting information "leak" from train into test and making the CV estimate overoptimistic. The fix: **purge** the training set of any observations whose labels overlap in time with observations in the test set (this ties directly back to violating assumption **SL1** in [[04 Supervised Learning Vs unsupervised learning.|ch.4]] — non-independent errors).
2. **Multiple testing / selection bias.** If the same test/validation set gets reused repeatedly while you iterate on model choices, you're implicitly fitting to that hold-out set too, inflating your apparent performance.

### 4.5 The Bootstrap

Instead of splitting the data, the bootstrap builds new datasets by **sampling $n$ observations from the original data set with replacement** — the same observation can appear more than once in a given bootstrap sample. This gives you $B$ bootstrap replicate estimates $\hat\alpha^{*1},\dots,\hat\alpha^{*B}$ of any quantity $\hat\alpha$ computed from the data, whose spread estimates the standard error of $\hat\alpha$ itself:

$$\text{SE}_B(\hat\alpha) = \sqrt{\frac{1}{B-1}\sum_{r=1}^B\left(\hat\alpha^{*r} - \frac{1}{B}\sum_{r'=1}^B \hat\alpha^{*r'}\right)^2}$$

This is just the ordinary sample-standard-deviation formula, applied across bootstrap replicates instead of across raw data points — the bootstrap turns "how much would this estimate vary if I re-ran the experiment" into something you can actually simulate from a single dataset.

## 5. Open questions for revision

- [ ] Work through why LOOCV, despite being (almost) unbiased, tends to have *higher variance* than k-fold CV as a test-error estimator — this is the flip side of the bias-variance tradeoff, applied to the CV estimate itself rather than to the model.
- [ ] Practice the "purging" fix for time-series CV on a toy example: given a small time-ordered dataset and a naive 5-fold split, identify which folds would leak information.
- [ ] Connect precision/recall to a concrete financial scenario (e.g. fraud detection, credit default) and decide which metric you'd prioritize and why.

---
*Source: BSF 3216 Lecture Two — "Supervised Learning" (Model Assessment/Selection, Bias-Variance Decomposition, Resampling).*
