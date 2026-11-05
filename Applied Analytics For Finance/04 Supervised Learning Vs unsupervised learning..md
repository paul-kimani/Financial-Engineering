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
>
> **Resolved:** derived in §7.8 (2026-10-11).

## 3. Proof: OLS is BLUE (unbiased *and* best)

This proves the full Gauss-Markov "BLUE" claim above — first the "U" (unbiasedness), then the "B" (minimum variance among *all* linear unbiased estimators) — worked through the way it's actually derived, not just asserted.

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

Start from the OLS intercept formula $\hat\beta_0=\bar Y-\hat\beta_1\bar X$, and substitute in $\hat\beta_1=\sum_i w_iY_i$ and $\bar Y=\frac1n\sum_i Y_i$:

$$\hat\beta_0 = \frac1n\sum_i Y_i - \bar X\sum_i w_iY_i = \sum_i\left(\frac1n-\bar Xw_i\right)Y_i$$

Both terms multiply $Y_i$, so factor it out and combine into one sum. Define $v_i=\dfrac1n-\bar Xw_i$ — built purely from $X$ (since $w_i$ is), so it's a fixed constant just like $w_i$ was, giving $\hat\beta_0=\sum_i v_iY_i$: again a weighted sum of the data.

**Property 1: $\sum_i v_i=1$.** Substitute the definition of $v_i$ and split the sum:
$$\sum_i v_i = \sum_i\left(\frac1n-\bar Xw_i\right) = \underbrace{\sum_i\frac1n}_{=\,n/n\,=\,1} - \;\bar X\underbrace{\sum_i w_i}_{=\,0} = 1-0=1$$
(the first piece is $\frac1n$ added to itself $n$ times; the second vanishes using $\sum_i w_i=0$, already proved above.)

**Property 2: $\sum_i v_iX_i=0$.** Same expansion:
$$\sum_i v_iX_i = \sum_i\left(\frac1n-\bar Xw_i\right)X_i = \underbrace{\frac1n\sum_i X_i}_{=\,\bar X} - \;\bar X\underbrace{\sum_i w_iX_i}_{=\,1} = \bar X-\bar X=0$$
(the first piece is just the definition of the sample mean $\bar X$; the second uses $\sum_i w_iX_i=1$, already proved above.)

These are exactly the mirror image of $w_i$'s properties ($0$ and $1$, flipped). Now substitute the true model $Y_i=\beta_0+\beta_1X_i+\epsilon_i$ into $\hat\beta_0=\sum_i v_iY_i$ and distribute:

$$\hat\beta_0 = \sum_i v_i(\beta_0+\beta_1X_i+\epsilon_i) = \beta_0\underbrace{\sum_i v_i}_{=\,1} + \;\beta_1\underbrace{\sum_i v_iX_i}_{=\,0} + \sum_i v_i\epsilon_i = \beta_0+\sum_i v_i\epsilon_i$$

Same shape as the $\hat\beta_1$ case: **estimate = true value + leftover noise**. Taking expectations:

$$\mathbb{E}(\hat\beta_0) = \mathbb{E}\left(\beta_0+\sum_i v_i\epsilon_i\right) = \beta_0+\sum_i v_i\,\mathbb{E}(\epsilon_i) = \beta_0+\sum_i v_i\cdot 0 = \beta_0$$

$\hat\beta_0$ is unbiased too — via exactly the same two assumptions as $\hat\beta_1$: **SL4** (independence of $X$ and $\epsilon$) is what lets $v_i$ pull outside the expectation as a fixed constant, and **SL2** ($\mathbb{E}(\epsilon_i)=0$) is what makes the remaining sum vanish.

> [!warning] A precision trap worth naming explicitly
> It's tempting to reach for "$\epsilon$ is Normally distributed" as the reason the noise term disappears in expectation — but **normality is never used anywhere in this proof**. The only property of $\epsilon$ that matters is its *mean* being zero (SL2); the *shape* of its distribution is irrelevant to unbiasedness. Normality only becomes necessary for a different question entirely — proving exact (rather than asymptotic) finite-sample distributions for hypothesis tests, or the MLE-equivalence question still open above. Keep "zero mean" (SL2) and "Normally distributed" mentally separate — conflating the two is one of the most common slips in this proof.

> [!tip] The one sentence version
> Both OLS estimators are just fixed-weight linear combinations of $Y$; the weights are built entirely from $X$ so they carry no randomness of their own, and once you substitute in the true model, only the $\mathbb E(\epsilon_i)=0$ assumption (SL2) is needed to make the noise term vanish in expectation — leaving exactly the true parameter behind.

### Proof: OLS is also Best (minimum variance among all linear unbiased estimators)

This closes out the "B" in BLUE. Unbiasedness above only proves $\hat\beta_0,\hat\beta_1$'s *own* variances are well-defined — it says nothing about how they compare to any other linear unbiased estimator you could dream up. This section proves no such competitor can ever beat OLS.

#### The $\hat\beta_0$ case

**Setup.** Define an arbitrary *other* linear estimator of $\beta_0$: $\hat\beta_0^*=\sum_i a_iY_i$, where $a_i$ are some fixed constants — not necessarily $v_i$. Substitute the true model and distribute, exactly as before:

$$\hat\beta_0^* = \sum_i a_i(\beta_0+\beta_1X_i+\epsilon_i) = \beta_0\sum_i a_i + \beta_1\sum_i a_iX_i + \sum_i a_i\epsilon_i$$

Taking expectations gives $\mathbb E(\hat\beta_0^*)=\beta_0\sum_i a_i+\beta_1\sum_i a_iX_i$. For this to equal $\beta_0$ — just $\beta_0$, with the $\beta_1$-term gone — the $\beta_0$-coefficient must survive as exactly 1, and the $\beta_1$-coefficient must vanish:

$$\sum_i a_i = 1 \qquad \sum_i a_iX_i = 0$$

These are the unbiasedness conditions on any candidate $a_i$ — and notice $v_i$ itself satisfies exactly these two conditions (proved above): $v_i$ is just the *particular* choice of weights that OLS happens to use.

**The decomposition trick.** Write $a_i = v_i + d_i$, where $d_i$ captures however $\hat\beta_0^*$'s weights *deviate* from OLS's own $v_i$. Since both $a_i$ and $v_i$ satisfy the same two conditions:

$$\sum_i d_i = \sum_i a_i - \sum_i v_i = 1-1=0 \qquad\qquad \sum_i d_iX_i = \sum_i a_iX_i-\sum_i v_iX_i = 0-0=0$$

**Expanding the variance.** Since $\hat\beta_0^*-\beta_0=\sum_i a_i\epsilon_i = \sum_i v_i\epsilon_i+\sum_i d_i\epsilon_i$, and using $\operatorname{Var}(A+B)=\operatorname{Var}(A)+\operatorname{Var}(B)+2\operatorname{Cov}(A,B)$:

$$\operatorname{Var}(\hat\beta_0^*) = \operatorname{Var}\left(\sum_i v_i\epsilon_i\right)+\operatorname{Var}\left(\sum_i d_i\epsilon_i\right)+2\operatorname{Cov}\left(\sum_i v_i\epsilon_i,\sum_i d_i\epsilon_i\right)$$

The identical double-sum collapse from §4 below (SL1–SL3: off-diagonal vanishes, diagonal survives with $\sigma^2$) applies to every term here too:

$$\operatorname{Var}(\hat\beta_0^*) = \sigma^2\sum_i v_i^2+\sigma^2\sum_i d_i^2+2\sigma^2\sum_i v_id_i$$

**Showing the cross term vanishes.** Substitute $v_i=\frac1n-\bar Xw_i$ and use $\sum_i d_i=0$:

$$\sum_i v_id_i = \sum_i\left(\frac1n-\bar Xw_i\right)d_i = \frac1n\underbrace{\sum_i d_i}_{=\,0}-\;\bar X\sum_i d_iw_i = -\bar X\sum_i d_iw_i$$

Then substitute $w_i=\frac{X_i-\bar X}{S}$ into the remaining piece, using $\sum_i d_iX_i=0$ and $\sum_i d_i=0$ again:

$$\sum_i d_iw_i = \frac1S\sum_i d_i(X_i-\bar X) = \frac1S\left(\sum_i d_iX_i-\bar X\sum_i d_i\right) = \frac1S(0-\bar X\cdot0)=0$$

so $\sum_i v_id_i = -\bar X\cdot0=0$ — the cross term drops out entirely.

**Conclusion.**

$$\operatorname{Var}(\hat\beta_0^*) = \underbrace{\sigma^2\sum_i v_i^2}_{=\,\operatorname{Var}(\hat\beta_0)}+\;\sigma^2\sum_i d_i^2 \qquad\Longrightarrow\qquad \boxed{\operatorname{Var}(\hat\beta_0^*)\;\ge\;\operatorname{Var}(\hat\beta_0)}$$

since $\sigma^2\sum_i d_i^2$ is a sum of squares times a positive constant, and can never be negative. Equality holds only when every $d_i=0$ — i.e. $a_i=v_i$ for all $i$ — meaning $\hat\beta_0^*$ was secretly OLS all along.

#### The $\hat\beta_1$ case (same trick, mirrored)

Define $\hat\beta_1^*=\sum_i a_iY_i$ for arbitrary fixed $a_i$. The unbiasedness conditions flip relative to the intercept case — mirroring $w_i$'s own properties (proved above: $\sum_i w_i=0,\ \sum_i w_iX_i=1$):

$$\sum_i a_i=0 \qquad\qquad \sum_i a_iX_i=1$$

Write $a_i=w_i+d_i$. Since both $a_i$ and $w_i$ satisfy these same two conditions:

$$\sum_i d_i = \sum_i a_i-\sum_i w_i = 0-0=0 \qquad\qquad \sum_i d_iX_i = \sum_i a_iX_i-\sum_i w_iX_i = 1-1=0$$

— the identical-looking $d_i$ properties as the intercept case, just arrived at from $w_i$'s conditions instead of $v_i$'s. Expanding the variance the same way:

$$\operatorname{Var}(\hat\beta_1^*) = \sigma^2\sum_i w_i^2+\sigma^2\sum_i d_i^2+2\sigma^2\sum_i w_id_i$$

and the cross term vanishes by the identical substitution $w_i=\frac{X_i-\bar X}{S}$:

$$\sum_i w_id_i = \frac1S\left(\sum_i d_iX_i-\bar X\sum_i d_i\right) = \frac1S(0-0)=0$$

So:

$$\operatorname{Var}(\hat\beta_1^*) = \underbrace{\sigma^2\sum_i w_i^2}_{=\,\operatorname{Var}(\hat\beta_1)}+\;\sigma^2\sum_i d_i^2 \qquad\Longrightarrow\qquad \boxed{\operatorname{Var}(\hat\beta_1^*)\;\ge\;\operatorname{Var}(\hat\beta_1)}$$

again with equality only when $\hat\beta_1^*$ was OLS all along ($d_i=0$ for every $i$).

> [!tip] What "Best" actually buys you
> This is a genuinely strong result — not "OLS performs well in practice" but a proof that *no* linear unbiased estimator, however cleverly constructed, can ever beat OLS's variance. Combined with the unbiasedness proof above, this completes BLUE: **B**est **L**inear **U**nbiased **E**stimator, all three letters now proven rather than asserted. And exactly as with unbiasedness, **normality of $\epsilon$ is never invoked anywhere in this proof** — only SL1–SL4 do all the work.

## 4. The variances of $\hat\beta_0$ and $\hat\beta_1$, and their covariance

Unbiasedness (§3) only tells you the *center* of $\hat\beta_0,\hat\beta_1$'s distribution matches the truth — it says nothing about how spread out the estimator is around that center. That's what these three quantities capture, and they're what "**lowest-variance**" in the BLUE claim (§2) is actually referring to.

### Setting up: a double sum, not a single one

You already have $\hat\beta_1-\beta_1=\sum_i w_i\epsilon_i$ from §3. Variance is the expected square of that:

$$\operatorname{Var}(\hat\beta_1) = \mathbb{E}\big[(\hat\beta_1-\beta_1)^2\big] = \mathbb{E}\left[\left(\sum_i w_i\epsilon_i\right)^{\!2}\right]$$

A sum squared isn't a sum of squares — it expands into a **double sum** over two indices $i,j$ (every pair, including a term against itself):

$$\left(\sum_i w_i\epsilon_i\right)^{\!2} = \sum_i\sum_j w_iw_j\,\epsilon_i\epsilon_j \quad\Longrightarrow\quad \operatorname{Var}(\hat\beta_1) = \sum_i\sum_j w_iw_j\,\mathbb{E}(\epsilon_i\epsilon_j)$$

### Collapsing the double sum

Split into diagonal ($i=j$) and off-diagonal ($i\neq j$) terms:

- **Off-diagonal ($i\neq j$):** $\mathbb{E}(\epsilon_i\epsilon_j) = \operatorname{Cov}(\epsilon_i,\epsilon_j) + \mathbb{E}(\epsilon_i)\mathbb{E}(\epsilon_j)$. **SL1** gives $\operatorname{Cov}(\epsilon_i,\epsilon_j)=0$, and **SL2** gives $\mathbb{E}(\epsilon_i)=\mathbb{E}(\epsilon_j)=0$, so the whole thing is $0$. (SL2 is doing quiet work here too, not just SL1 — it's what lets you equate $\mathbb{E}(\epsilon_i\epsilon_j)$ with the covariance in the first place.)
- **Diagonal ($i=j$):** $\mathbb{E}(\epsilon_i^2) = \operatorname{Var}(\epsilon_i)+[\mathbb{E}(\epsilon_i)]^2$. **SL3** gives $\operatorname{Var}(\epsilon_i)=\sigma^2$, and **SL2** kills the second term, leaving $\mathbb{E}(\epsilon_i^2)=\sigma^2$.

So every off-diagonal term vanishes and only the diagonal survives:

$$\operatorname{Var}(\hat\beta_1) = \sum_i w_i^2\cdot\sigma^2 = \sigma^2\sum_i w_i^2$$

### Closing the form: $\sum_i w_i^2$

Recall $w_i=\dfrac{X_i-\bar X}{S}$ where $S=\sum_j(X_j-\bar X)^2$. Squaring and summing:

$$\sum_i w_i^2 = \frac{\sum_i(X_i-\bar X)^2}{S^2} = \frac{S}{S^2} = \frac1S$$

(the numerator is exactly $S$ again, so one power of $S$ cancels). Therefore:

$$\boxed{\operatorname{Var}(\hat\beta_1) = \frac{\sigma^2}{S} = \frac{\sigma^2}{\sum_j(X_j-\bar X)^2}}$$

### $\operatorname{Var}(\hat\beta_0)$ — same collapse, one new piece of algebra

By the identical double-sum argument applied to $\hat\beta_0-\beta_0=\sum_i v_i\epsilon_i$:

$$\operatorname{Var}(\hat\beta_0) = \sigma^2\sum_i v_i^2$$

Expand $v_i^2=\left(\frac1n-\bar Xw_i\right)^2 = \frac1{n^2}-\frac{2\bar X}{n}w_i+\bar X^2w_i^2$ and sum over $i$, using $\sum_i w_i=0$ and $\sum_i w_i^2=\frac1S$ (both already established):

$$\sum_i v_i^2 = \frac1n - \frac{2\bar X}{n}\cdot 0 + \bar X^2\cdot\frac1S = \frac1n+\frac{\bar X^2}{S} = \frac{S+n\bar X^2}{nS}$$

One more standard identity closes it: $S=\sum_i(X_i-\bar X)^2 = \sum_i X_i^2 - n\bar X^2$ (expand $(X_i-\bar X)^2$, sum, use $\sum_iX_i=n\bar X$), so $S+n\bar X^2=\sum_iX_i^2$. Substituting:

$$\sum_i v_i^2 = \frac{\sum_iX_i^2}{nS} \qquad\Longrightarrow\qquad \boxed{\operatorname{Var}(\hat\beta_0) = \frac{\sigma^2\sum_iX_i^2}{n\,S} = \frac{\sigma^2\sum_iX_i^2}{n\sum_j(X_j-\bar X)^2}}$$

> [!warning] A likely source-material typo
> Some sources (including a proposition transcribed from this course's material) state $\operatorname{Var}(\hat\beta_0)=\dfrac{\sigma^2\sum X_i^2}{\sum(X_i-\bar X)^2}$ — **missing the $n$** in the denominator. The derivation above was independently checked twice (matches standard references such as Gujarati's *Basic Econometrics*), and there's a fast sanity check that confirms which version is right: if the data is centered so $\bar X=0$, then $\hat\beta_0=\bar Y$ exactly, so $\operatorname{Var}(\hat\beta_0)$ must reduce to $\operatorname{Var}(\bar Y)=\sigma^2/n$. Plug $\bar X=0$ into both candidate formulas and check which one actually gives $\sigma^2/n$ — only the version with the $n$ in the denominator survives that test. Worth flagging if this formula appears on an assessment matching the no-$n$ version.

### $\operatorname{Cov}(\hat\beta_0,\hat\beta_1)$ — the cross term

Same double-sum technique, but now squaring two *different* weighted sums against each other:

$$\operatorname{Cov}(\hat\beta_0,\hat\beta_1) = \mathbb{E}\big[(\hat\beta_0-\beta_0)(\hat\beta_1-\beta_1)\big] = \mathbb{E}\left[\left(\sum_i v_i\epsilon_i\right)\left(\sum_j w_j\epsilon_j\right)\right] = \sum_i\sum_j v_iw_j\,\mathbb{E}(\epsilon_i\epsilon_j)$$

The exact same collapse applies (off-diagonal vanishes, diagonal survives with $\sigma^2$):

$$\operatorname{Cov}(\hat\beta_0,\hat\beta_1) = \sigma^2\sum_i v_iw_i$$

Substitute $v_i=\frac1n-\bar Xw_i$ and use $\sum_i w_i=0$, $\sum_i w_i^2=\frac1S$ again:

$$\sum_i v_iw_i = \frac1n\sum_i w_i - \bar X\sum_i w_i^2 = \frac1n\cdot 0 - \bar X\cdot\frac1S = -\frac{\bar X}{S}$$

$$\boxed{\operatorname{Cov}(\hat\beta_0,\hat\beta_1) = -\frac{\sigma^2\bar X}{S} = -\frac{\sigma^2\bar X}{\sum_j(X_j-\bar X)^2}}$$

This one matches the source proposition exactly — a useful cross-check that the technique itself is sound, since two of these three results (Var($\hat\beta_1$) and this covariance) match the source cleanly, isolating Var($\hat\beta_0$) as the one with the likely transcription error rather than a flaw in the method.

### Cheat sheet

| Quantity | Formula | Notes |
|---|---|---|
| $\operatorname{Var}(\hat\beta_1)$ | $\sigma^2/S$ | shrinks as $X$'s spread ($S$) grows — more spread-out predictor data pins down the slope more precisely |
| $\operatorname{Var}(\hat\beta_0)$ | $\sigma^2\sum_iX_i^2/(nS)$ | source material's stated version omits the $n$ — see warning above |
| $\operatorname{Cov}(\hat\beta_0,\hat\beta_1)$ | $-\sigma^2\bar X/S$ | sign flips with $\bar X$; zero exactly when $\bar X=0$ (centered data) |

### Estimating $\sigma^2$ itself: $\hat\sigma^2$ is unbiased

$\sigma^2$ shows up in every variance formula above — $\operatorname{Var}(\hat\beta_1)=\sigma^2/S$, and so on — but it's itself unknown, since it's a property of the unobservable true errors $\epsilon_i$. The natural estimator (used throughout [[05 Multiple Linear Regression and the F-test|ch.5]]'s selection criteria and [[12 Regression Diagnostics, Heteroskedasticity and GLS|ch.12]]'s diagnostics) is:

$$\hat\sigma^2 = \frac{1}{n-2}\sum_i\hat\epsilon_i^2$$

where $\hat\epsilon_i=Y_i-\hat\beta_0-\hat\beta_1X_i$ is the OLS residual (also called the **mean square error**, MSE; its square root $\hat\sigma=\sqrt{\text{MSE}}$ is the **standard error of the residuals**, SER). This proves $\mathbb E(\hat\sigma^2)=\sigma^2$.

#### Relating the residual to the true error

Substitute the true model into the residual definition and regroup. Two genuinely different pairs of objects are involved: $\hat\beta_0,\hat\beta_1$ (the OLS estimates, computed from data) and $\beta_0,\beta_1$ (the true, unknown parameters that actually generated the data, via $Y_i=\beta_0+\beta_1X_i+\epsilon_i$):

$$\hat\epsilon_i = Y_i-\hat\beta_0-\hat\beta_1X_i = \epsilon_i - (\hat\beta_0-\beta_0) - (\hat\beta_1-\beta_1)X_i$$

Eliminate $(\hat\beta_0-\beta_0)$ using the OLS intercept formula $\hat\beta_0=\bar Y-\hat\beta_1\bar X$ together with the true model averaged over $i$ ($\bar Y=\beta_0+\beta_1\bar X+\bar\epsilon$, where $\bar\epsilon=\frac1n\sum_i\epsilon_i$):

$$\hat\beta_0-\beta_0 = \bar\epsilon-\bar X(\hat\beta_1-\beta_1)$$

Substituting back in and collecting the two $(\hat\beta_1-\beta_1)$ terms into one:

$$\boxed{\hat\epsilon_i = (\epsilon_i-\bar\epsilon) - (\hat\beta_1-\beta_1)(X_i-\bar X)}$$

A clean result: if the slope estimate were exactly right ($\hat\beta_1=\beta_1$), the residual would collapse to just $\epsilon_i-\bar\epsilon$.

#### Squaring and summing

Square and sum over $i$, treating $(\hat\beta_1-\beta_1)$ as a constant (it doesn't depend on $i$):

$$\sum_i\hat\epsilon_i^2 = \sum_i(\epsilon_i-\bar\epsilon)^2 - 2(\hat\beta_1-\beta_1)\sum_i(\epsilon_i-\bar\epsilon)(X_i-\bar X) + (\hat\beta_1-\beta_1)^2\sum_i(X_i-\bar X)^2$$

The middle sum simplifies: $\sum_i(\epsilon_i-\bar\epsilon)(X_i-\bar X)=\sum_i(X_i-\bar X)\epsilon_i-\bar\epsilon\sum_i(X_i-\bar X)=\sum_i(X_i-\bar X)\epsilon_i$ (the second piece vanishes, deviations from the mean sum to zero). Substituting $X_i-\bar X=Sw_i$ and recalling $\hat\beta_1-\beta_1=\sum_iw_i\epsilon_i$ (from §3):

$$\sum_i(X_i-\bar X)\epsilon_i = S\sum_iw_i\epsilon_i = S(\hat\beta_1-\beta_1)$$

So the middle term is $-2S(\hat\beta_1-\beta_1)^2$, which combines with the last term to give $-S(\hat\beta_1-\beta_1)^2$, leaving:

$$\sum_i\hat\epsilon_i^2 = \sum_i(\epsilon_i-\bar\epsilon)^2 - S(\hat\beta_1-\beta_1)^2$$

#### Taking expectations

Three separate pieces close this out:

- $\mathbb E\big[S(\hat\beta_1-\beta_1)^2\big] = S\cdot\mathbb E\big[(\hat\beta_1-\beta_1)^2\big] = S\cdot\operatorname{Var}(\hat\beta_1) = S\cdot\dfrac{\sigma^2}{S}=\sigma^2$ — using $\mathbb E(\hat\beta_1)=\beta_1$ (unbiasedness, §3), so $\mathbb E[(\hat\beta_1-\beta_1)^2]$ is exactly the definition of $\operatorname{Var}(\hat\beta_1)=\sigma^2/S$ (§4 above).
- $\mathbb E\left[\sum_i\epsilon_i^2\right] = \sum_i\mathbb E(\epsilon_i^2) = n\sigma^2$ — each $\mathbb E(\epsilon_i^2)=\sigma^2$ is the diagonal-term result from the double-sum collapse earlier in §4.
- $\mathbb E\left[n\bar\epsilon^2\right] = n\operatorname{Var}(\bar\epsilon) = \sigma^2$ — since $\bar\epsilon=\sum_i\frac1n\epsilon_i$ is itself a weighted sum with constant weight $\frac1n$, the same double-sum technique gives $\operatorname{Var}(\bar\epsilon)=\sigma^2\sum_i(1/n)^2=\sigma^2/n$, and $\mathbb E(\bar\epsilon)=0$ (SL2) means $\mathbb E(\bar\epsilon^2)=\operatorname{Var}(\bar\epsilon)$.

Using $\sum_i(\epsilon_i-\bar\epsilon)^2=\sum_i\epsilon_i^2-n\bar\epsilon^2$ (the same deviations-from-mean identity behind $S=\sum_iX_i^2-n\bar X^2$, applied to $\epsilon$ instead of $X$):

$$\mathbb E\left[\sum_i(\epsilon_i-\bar\epsilon)^2\right] = n\sigma^2-\sigma^2 = (n-1)\sigma^2$$

$$\mathbb E\left[\sum_i\hat\epsilon_i^2\right] = (n-1)\sigma^2 - \sigma^2 = (n-2)\sigma^2$$

$$\mathbb E(\hat\sigma^2) = \frac{1}{n-2}\,\mathbb E\left[\sum_i\hat\epsilon_i^2\right] = \frac{(n-2)\sigma^2}{n-2} = \sigma^2$$

$\hat\sigma^2$ is unbiased.

> [!tip] Why $n-2$, not $n$ — degrees of freedom
> Every parameter estimated from the data (rather than known in advance) removes one **degree of freedom** — one independent piece of information the residuals are free to vary over. OLS's own first-order conditions force $\sum_i\hat\epsilon_i=0$ and $\sum_iX_i\hat\epsilon_i=0$ — two exact constraints tying the $n$ residuals together, one from estimating $\beta_0$ and one from estimating $\beta_1$. So only $n-2$ of the $n$ residuals are actually free; dividing by the raw count $n$ instead would systematically *underestimate* $\sigma^2$, since OLS explicitly minimizes $\sum_i\hat\epsilon_i^2$ and so squeezes the residuals smaller than the true errors would be on average. This generalizes directly: with $p$ predictors plus an intercept ($p+1$ estimated parameters), the divisor becomes $n-p-1$ — see [[05 Multiple Linear Regression and the F-test]] for the multiple-regression version of this same $\hat\sigma^2$.

## 5. From simple to multiple regression

Real financial models almost always need more than one predictor — see [[05 Multiple Linear Regression and the F-test]] for the full multi-predictor model, the ANOVA decomposition (SSR/SSReg/TSS), and the F-test for overall significance, and [[06 CAPM and Multifactor Models]] for the concrete application (CAPM is literally an SLR; Fama-French is a multiple regression).

## 6. Where this sits in the unit

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

---

## 7. Additions — study session 2026-10-11

> Material worked through live that the sections above did not yet cover:
> deriving OLS itself, the normal equations, a full worked example (Problem
> Set Q8), the lever-arm reading of $\operatorname{Var}(\hat\beta_0)$, an
> assumption inventory, and the OLS = MLE equivalence that closes the open
> item in §2.

### 7.1 Supervised vs. unsupervised — what actually decides it

The deciding feature is not the algorithm but **whether an answer column
$Y$ exists to check against**. Regime detection ("calm" vs "turbulent" days)
from volatility alone is unsupervised (clustering); if someone hand-labels past
days and you train on those labels, the *same task* becomes supervised
classification.

| Task | Type |
| :--- | :--- |
| Predict next month's bond yield from the Treasury rate | Supervised — regression |
| Group NSE stocks by how their returns co-move, no labels | Unsupervised — clustering |
| Predict whether gold closes Up or Down tomorrow | Supervised — classification |
| Detect market regimes from volatility, no labels | Unsupervised |

### 7.2 Diagnosing which assumption fails

**Quick test:** SL1 is *error vs. error*; SL3 is *error vs. its own spread*;
SL4 is *error vs. $X$*.

- **Big errors follow big errors (volatility clustering)** → SL3 (variance
  changes over time) and the independence behind SL1. Subtle point: the errors
  themselves can be uncorrelated (sign unpredictable) while their *sizes* are
  correlated — so strictly SL3 is what fails. Not SL4: no $X$ is involved.
- **Bond-yield errors larger when rates are high** → SL3.
- **Return regressed on volume, with news driving both** → SL4. The omitted
  variable (news) sits inside $\epsilon$ and is correlated with $X$, so
  $\operatorname{Cov}(X_i,\epsilon_i)\neq0$: **omitted variable bias**.

**Normality is a separate, fifth assumption.** Problem Set Q2 writes
$\epsilon\sim\text{iid }\mathcal N(0,\sigma^2)$; SL1–SL4 alone deliver
unbiasedness and BLUE, while normality adds exact $t$/$F$ distributions, the
MLE equivalence (§7.6) and the Cramér–Rao result.

### 7.3 Deriving OLS: minimise the sum of squared residuals

$$
S(\beta_0,\beta_1)=\sum_{i=1}^{n}\big(Y_i-\beta_0-\beta_1X_i\big)^2
$$

**Why squared misses:** squares are positive, so over- and under-predictions
cannot cancel; large misses are punished more than small ones; and $S$ is a
smooth bowl, so calculus finds the minimum exactly. The cost is sensitivity to
outliers.

**First condition (chain rule):**

$$
\frac{\partial S}{\partial\beta_0}=-2\sum_i(Y_i-\beta_0-\beta_1X_i)=0
\;\Longrightarrow\;
\sum_iY_i-n\beta_0-\beta_1\sum_iX_i=0
\;\Longrightarrow\;
\hat\beta_0=\bar Y-\hat\beta_1\bar X
$$

Two consequences: **OLS residuals sum to zero** (with an intercept), and the
fitted line **always passes through $(\bar X,\bar Y)$**.

**Second condition:**

$$
\frac{\partial S}{\partial\beta_1}=-2\sum_iX_i(Y_i-\beta_0-\beta_1X_i)=0
$$

(the extra $X_i$ is the inner derivative). It says $\sum_iX_i\hat\epsilon_i=0$:
**residuals are uncorrelated with $X$** — OLS removes every bit of *linear*
information $X$ carries about $Y$. The two conditions are the **normal
equations**.

**Solving for the slope.** Substitute $\beta_0=\bar Y-\beta_1\bar X$ and use
$\sum_iX_i=n\bar X$:

$$
\sum X_iY_i-n\bar X\bar Y+n\beta_1\bar X^2-\beta_1\sum X_i^2=0
$$

**Collect every term containing $\beta_1$ and factor it out**, then move it
across and divide:

$$
\sum X_iY_i-n\bar X\bar Y=\beta_1\Big(\sum X_i^2-n\bar X^2\Big)
\quad\Longrightarrow\quad
\boxed{\hat\beta_1=\frac{\sum X_iY_i-n\bar X\bar Y}{\sum X_i^2-n\bar X^2}=\frac{S_{xy}}{S_{xx}}}
$$

The numerator holds the terms **without** $\beta_1$. Equivalent deviation
form (the meaning — sample covariance over sample variance of $X$):

$$
\hat\beta_1=\frac{\sum(X_i-\bar X)(Y_i-\bar Y)}{\sum(X_i-\bar X)^2}
$$

**Interpretation.** $\hat\beta_1$ is the predicted change in $Y$ for a
one-unit increase in $X$; $\hat\beta_0$ is the predicted $Y$ at $X=0$ —
meaningful only if $X=0$ lies within or near the data.

### 7.4 Two different "sums to zero"

| Statement | Why it is zero |
| :--- | :--- |
| $\sum_i(X_i-\bar X)=0$ | **Definition of the mean**: $\sum X_i-n\bar X=n\bar X-n\bar X=0$. Pure algebra, any data. |
| $\sum_i\hat\epsilon_i=0$ | **OLS normal equation** for $\beta_0$. True only because of how OLS chose the line. |

Citing "because of OLS" for the first one is a common, mark-losing slip.

**A related algebra rule** used in the BLUE proof: a sum of products is not a
product of sums, $\sum a_ib_i\neq\sum a_i\cdot\sum b_i$ (e.g.
$a=(1,2),b=(3,4)$: $11\neq21$). Only constants leave a sum. So

$$
\sum_iw_id_i=\frac{1}{S_{xx}}\sum_i(X_i-\bar X)d_i=\frac{1}{S_{xx}}\Big(\sum d_iX_i-\bar X\sum d_i\Big)=0
$$

uses **both** unbiasedness conditions on $d_i$. A "proof" that makes the
cross term vanish without using them would prove it for *any* $d_i$ — a sign a
step is wrong.

### 7.5 Worked example — Problem Set Q8 (bond yield on Treasury rate)

| Month | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| :--- | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| Treasury $X$ (%) | 5 | 6.5 | 7.2 | 8.0 | 8.3 | 8.5 | 9 | 9.5 |
| Bond yield $Y$ (%) | 7.5 | 7.9 | 8.5 | 9.2 | 9.5 | 10.4 | 10.9 | 11.2 |

$\sum X_i=62$, $\sum Y_i=75.1$, $\sum X_i^2=495.48$, $\sum X_iY_i=595.4$,
$n=8$, so $\bar X=7.75$, $\bar Y=9.3875$.

$$
\hat\beta_1=\frac{595.4-8(7.75)(9.3875)}{495.48-8(7.75)^2}=\frac{595.4-582.025}{495.48-480.5}=\frac{13.375}{14.98}=0.8929
$$

$$
\hat\beta_0=9.3875-0.8929(7.75)=2.4679\qquad\Longrightarrow\qquad\boxed{\hat Y=2.4679+0.8929X}
$$

**Interpretation.** Each 1 percentage-point rise in the Treasury rate goes with
an average 0.89 pp rise in the 20-year yield — close to, but slightly less than,
one-for-one. $\hat\beta_0$ is the predicted yield at a 0% Treasury rate, an
extrapolation far outside the observed 5–9.5% range, so it anchors the line
rather than forecasting anything. $R^2=0.904$.

**Observations for Q8(c).** Strong positive, nearly linear relationship; the
line passes through $(7.75,\,9.39)$; the residuals
$(+0.57,-0.37,-0.40,-0.41,-0.38,+0.34,+0.40,+0.25)$ run positive–negative–
positive, hinting at slight curvature (with 8 points, a hint rather than proof).

**Slips to guard against:** $\sum Y_i$ mis-added (75.1, not 84.6) and the
wrong $n$ (8, not 6). **Ten-second check:** the line must pass through
$(\bar X,\bar Y)$, and $\bar Y$ must look like the middle of the $Y$ column;
plugging one data point in should give a value near the actual.

### 7.6 Reading $\operatorname{Var}(\hat\beta_0)$ as a lever arm

$$
\operatorname{Var}(\hat\beta_0)=\sigma^2\left(\frac1n+\frac{\bar X^2}{S_{xx}}\right)=\frac{\sigma^2\sum X_i^2}{nS_{xx}}
$$

(the second form follows from $S_{xx}+n\bar X^2=\sum X_i^2$). Since
$\hat\beta_0=\bar Y-\hat\beta_1\bar X$ starts at the pivot $(\bar X,\bar Y)$
and slides a distance $\bar X$ along the estimated slope back to $X=0$:

$$
\operatorname{Var}(\hat\beta_0)=\underbrace{\frac{\sigma^2}{n}}_{\text{pivot height }\bar Y}+\underbrace{\bar X^2\cdot\frac{\sigma^2}{S_{xx}}}_{(\text{distance})^2\times\operatorname{Var}(\hat\beta_1)}
$$

**Sanity check:** if $\bar X=0$ then $\hat\beta_0=\bar Y$ and
$\operatorname{Var}(\hat\beta_0)=\sigma^2/n$ — the variance of a mean of $n$
uncorrelated, equal-variance observations (SL1, SL3). This is why the Q8
intercept is imprecise: $X=0$ is 7.75 units from the centre of the data.

**Intuition for $\operatorname{Var}(\hat\beta_1)=\sigma^2/S_{xx}$:** spread-out
$X$ values pin the line firmly (points bunched together let it wobble); each
extra observation adds a non-negative term to $S_{xx}$; noisier errors raise
the variance.

**Gauss–Markov in one sentence:** under SL1–SL4, OLS has the smallest variance
of all linear unbiased estimators. Equality
$\operatorname{Var}(\tilde\beta_1)=\operatorname{Var}(\hat\beta_1)$ requires
$\sum d_i^2=0$, i.e. every $d_i=0$ — the rival *is* OLS.

### 7.7 Assumption inventory

| Result | Assumptions used |
| :--- | :--- |
| $\hat\beta_1=\sum w_iY_i$ is linear with fixed weights | SL4 only (weights built from fixed $X$) |
| $\mathbb E(\hat\beta_1)=\beta_1$, $\mathbb E(\hat\beta_0)=\beta_0$ | SL2, SL4 |
| $\operatorname{Var}(\hat\beta_1)=\sigma^2/S_{xx}$ | SL1, SL3, SL4 |
| OLS is BLUE | SL1–SL4 |
| $\mathbb E\big[\sum(\epsilon_i-\bar\epsilon)^2\big]=(n-1)\sigma^2$ | SL1, SL2, SL3 |
| $\hat\sigma^2=\text{RSS}/(n-2)$ unbiased | SL1–SL4 |
| Exact $t$ and $F$ tests; OLS = MLE | Normality **on top of** SL1–SL4 |

**Watch the labels in the $\hat\sigma^2$ proof:** $\operatorname{Var}(\bar\epsilon)=\sigma^2/n$
uses SL1 (no covariance terms, so the variance of a sum is the sum of
variances) and SL3 — not SL4, since no $X$ appears. And the expectation needs
**both** pieces, $\mathbb E\big[\sum\epsilon_i^2\big]=n\sigma^2$ and
$n\,\mathbb E(\bar\epsilon^2)=\sigma^2$. Alarm bell: a sum of squares can never
have a negative expectation.

**Same principle, one level down:** the $(n-1)$ in
$\mathbb E\big[\sum(\epsilon_i-\bar\epsilon)^2\big]$ is why the ordinary sample
variance divides by $n-1$ — estimating one mean costs one degree of freedom;
regression estimates two parameters and loses two.

### 7.8 OLS as maximum likelihood (closes the §2 open item)

Add normality, $\epsilon_i\sim\text{iid }\mathcal N(0,\sigma^2)$, so
$Y_i\sim\mathcal N(\beta_0+\beta_1X_i,\sigma^2)$ with density

$$
f(Y_i)=\frac{1}{\sqrt{2\pi\sigma^2}}\exp\!\left(-\frac{(Y_i-\beta_0-\beta_1X_i)^2}{2\sigma^2}\right)
$$

**Likelihood:** treat the data as fixed and ask which parameters make it most
probable. Independence makes the joint density a product,
$L=\prod_{i=1}^nf(Y_i)$. Logs turn the product into a sum (and, being
increasing, leave the maximiser unchanged):

$$
\ell=\ln L=\sum_i\ln f(Y_i)=-\frac n2\ln(2\pi)-\frac n2\ln\sigma^2-\frac{1}{2\sigma^2}\sum_i(Y_i-\beta_0-\beta_1X_i)^2
$$

**Slope and intercept.** $\beta_0,\beta_1$ appear only in
$-\frac{1}{2\sigma^2}S(\beta_0,\beta_1)$; since $\frac{1}{2\sigma^2}>0$,
maximising $\ell$ is *identical* to minimising $S$. Hence

$$
\hat\beta_{0,\text{MLE}}=\hat\beta_{0,\text{OLS}},\qquad\hat\beta_{1,\text{MLE}}=\hat\beta_{1,\text{OLS}}
$$

— under normality. Without normality OLS is still BLUE (Gauss–Markov); with it,
OLS is also the MLE.

**Error variance.** Differentiating with respect to $\sigma^2$ and evaluating
at the estimates, where the sum becomes residuals $\hat\epsilon_i$ (not true
errors):

$$
\frac{\partial\ell}{\partial\sigma^2}=-\frac{n}{2\sigma^2}+\frac{1}{2\sigma^4}\sum_i\hat\epsilon_i^2=0
\quad\Longrightarrow\quad
\hat\sigma^2_{\text{MLE}}=\frac{\text{RSS}}{n}
$$

**Biased downward:** using $\mathbb E(\text{RSS})=(n-2)\sigma^2$,

$$
\mathbb E\big(\hat\sigma^2_{\text{MLE}}\big)=\frac{(n-2)\sigma^2}{n}=\sigma^2-\frac{2\sigma^2}{n}<\sigma^2
$$

Dividing the same RSS by a larger number ($n$ rather than $n-2$) must give a
smaller answer. The bias is $2\sigma^2/n$, not a fixed amount: for Q8
($n=8$) the MLE averages $6/8=75\%$ of the truth; for $n=252$, $250/252\approx99.2\%$.
As $n\to\infty$ the bias vanishes — the MLE is **asymptotically unbiased**,
typical of MLEs.

---

## 8. Finite-sample inference, confidence intervals and tests (lecture notes §2.1.3–2.1.4)

> Worked through from the lecture notes, 2026-10-12. Intuition first; the
> lecturer's typos are flagged where they appear.

### 8.1 Why the estimate has a distribution

$\hat\beta_1$ is built from the random $Y_i$, so a different sample (another 8
months of Q8 data) gives a different slope. The spread of all the values it
*could* take is its **sampling distribution**; inference reasons from the one
estimate you have back to the true $\beta_1$ using that distribution.

Chapter 4 already gave its **centre** ($\mathbb E(\hat\beta_1)=\beta_1$) and
**width** ($\sigma^2/S_{xx}$). **SL5** ($\epsilon_i\sim\mathcal N(0,\sigma^2)$)
gives its **shape**: $\hat\beta_1=\beta_1+\sum w_i\epsilon_i$ is a constant plus
a fixed-weight sum of independent normals, and such a sum is normal. Same for
$\hat\beta_0=\sum v_iY_i$ and for fitted values $\hat Y_i$.

$$
\hat\beta_1\sim\mathcal N\!\left(\beta_1,\frac{\sigma^2}{\sum(X_i-\bar X)^2}\right),\qquad
\hat\beta_0\sim\mathcal N\!\left(\beta_0,\frac{\sigma^2\sum X_i^2}{n\sum(X_i-\bar X)^2}\right)
$$

"**Exact / finite sample**": true for any $n$ (even 8), not just
approximately for large $n$ via the Central Limit Theorem. This is the first
result in the course that genuinely needs SL5.

**Typo (p.21, p.23–24):** the notes omit the $n$ in the denominator of
$\operatorname{Var}(\hat\beta_0)$. Check: with $\bar X=0$, $\hat\beta_0=\bar Y$,
whose variance must be $\sigma^2/n$ — only the corrected formula gives that.

### 8.2 Standardising: a universal ruler

$$
Z=\frac{\hat\beta_1-\beta_1}{\sigma/\sqrt{S_{xx}}}\sim\mathcal N(0,1)
$$

$Z$ is "how many standard deviations the estimate is from the truth" — the same
scale whatever the units. **Typo (p.24):** the notes put an extra $n$ under the
square root; correct forms:

$$
\frac{(\hat\beta_1-\beta_1)\sqrt{\sum(X_i-\bar X)^2}}{\sigma}\sim\mathcal N(0,1),\qquad
\frac{(\hat\beta_0-\beta_0)\sqrt{n\sum(X_i-\bar X)^2}}{\sigma\sqrt{\sum X_i^2}}\sim\mathcal N(0,1)
$$

### 8.3 Replacing $\sigma$ by $\hat\sigma$ gives a $t$

$\sigma$ is unknown, so plug in $\hat\sigma=\text{SER}=\sqrt{\text{SSR}/(n-2)}$.
The **standard error** $\hat\sigma_{\hat\beta_1}=\hat\sigma/\sqrt{S_{xx}}$ is the
"Std. Error" column in R. Because $\hat\sigma$ itself wobbles (and is sometimes
too small by chance), the ratio produces extreme values more often than a
normal: same bell, **fatter tails** — Student's $t$ with $n-2$ degrees of
freedom (the same $n-2$ as in $\hat\sigma^2$):

$$
t_{\hat\beta_1}=\frac{\hat\beta_1-\beta_1}{\hat\sigma_{\hat\beta_1}}\sim t_{n-2}
$$

Few df → fat tails (Q8, 6 df: 95% cutoff **2.447**, not 1.96); many df → almost
normal. **Typo (p.25):** $\operatorname{Var}(t_\nu)=\nu/(\nu-2)$, i.e.
$(n-2)/(n-4)$ for $t_{n-2}$, not $n/(n-2)$ — the point (variance $>1$) stands.

### 8.4 Confidence interval

The middle $1-\alpha$ of the $t$ bell:

$$
\mathbb P\!\left(-t_{n-2,1-\alpha/2}\le\frac{\hat\beta_1-\beta_1}{\hat\sigma_{\hat\beta_1}}\le t_{n-2,1-\alpha/2}\right)=1-\alpha
$$

(symmetry gives $t_{n-2,\alpha/2}=-t_{n-2,1-\alpha/2}$). Multiply through by the
**standard error** (not its square), subtract $\hat\beta_1$, multiply by $-1$ —
which **reverses the inequalities** ($2<5$ but $-2>-5$) — to isolate $\beta_1$:

$$
\hat\beta_1\pm t_{n-2,1-\alpha/2}\,\hat\sigma_{\hat\beta_1}\qquad(\text{same form for }\beta_0)
$$

**Idea in one line:** if the estimate is within $k$ SEs of the truth, the truth
is within $k$ SEs of the estimate. **Meaning of 95%:** a property of the
procedure — across repeated samples, 95% of such intervals capture the fixed
true $\beta_1$.

**Q8:** $0.8929\pm2.447\times0.1187=0.8929\pm0.2905\Rightarrow(0.602,\ 1.183)$.
Contains 1 (one-for-one cannot be ruled out); excludes 0 (there is a
relationship). Wide because $n=8$. *Slip to avoid:* $0.1187\times2.447=0.2905$,
not $0.029$ — an interval of $\pm0.03$ from 8 points would be implausibly tight.

### 8.5 Hypothesis tests

**Courtroom logic.** $H_0$ is the default (often "no effect"); reject only if
the data make it implausible. Otherwise **fail to reject** — never "accept":
lack of evidence is not proof.

| | $H_0$ true | $H_1$ true |
| :--- | :--- | :--- |
| Reject $H_0$ | **Type I** error (false alarm), probability $\alpha$ | correct |
| Fail to reject | correct | **Type II** error (missed real effect) |

$\alpha$ is chosen **before** seeing the data.

**Test statistic** — how many SEs the estimate is from the hypothesised value
$\beta_{10}$:

$$
t_{\hat\beta_1}=\frac{\hat\beta_1-\beta_{10}}{\hat\sigma_{\hat\beta_1}}\sim t_{n-2}\ \text{under }H_0\ (\text{SL1–SL5})
$$

Reject if $|t|>$ critical value, equivalently p-value $<\alpha$ (p-value = chance
of an estimate at least this far from $\beta_{10}$ if $H_0$ were true).
$\beta_{10}$ need not be 0.

- **Two-sided** $H_1:\beta_1\neq\beta_{10}$: $\alpha$ split over both tails (Q8:
  2.447).
- **One-sided** $H_1:\beta_1>\beta_{10}$ (or $<$): all of $\alpha$ in one tail
  (Q8: 1.943) — easier to reject, but only in the direction stated in advance.
  The claim being tested goes in $H_1$, putting the burden of proof on it.

**Typos (p.26):** "$H_0:\beta_1\neq0$" should be $H_1$; "$t=0$ if
$\hat\beta_1\neq\beta_{10}$" should read $t\neq0$; "(SL1)(SL4)" means SL1–SL4.

**Q8 worked tests.**

- $H_0:\beta_1=0$: $t=0.8929/0.1187=7.52>2.447$ → reject; p $\approx0.0003$.
- $H_0:\beta_1=1$ vs $\neq1$: $t=(0.8929-1)/0.1187=-0.902$, $|t|<2.447$ → **fail
  to reject $H_0$** at 5%. Agrees with the CI containing 1 — a two-sided 5%
  test and a 95% CI always agree.
- **Why not "accept $\beta_1=1$":** $H_0:\beta_1=0.9$ or $0.8$ would *also* not
  be rejected; you cannot accept several different values at once. The data are
  merely *consistent* with 1.

**R output (lecturer's p.19 example, `aaa_dif ~ cm10_dif`, 878 residual df ⇒
$n=880$):** slope $t=0.615762/0.012117=50.82$ → overwhelmingly significant; a
1-pt change in the 10-yr Treasury rate goes with ~0.62-pt change in AAA yields,
95% CI $\approx(0.592,0.640)$. Intercept $t=-0.05$, p $=0.96$ → indistinguishable
from 0. Each row of an R coefficient table is a test of $H_0:\beta_j=0$.

### 8.6 Applications

- **Stock beta:** CAPM $\hat\beta=1.19$ (AAPL) — the CI says whether the stock is
  *really* more volatile than the market (interval above 1) or not
  distinguishable.
- **Hedge ratios** (pairs trading): a wide CI on the regression slope means the
  hedge is shaky and residual market risk remains.
- **Is a bot's edge real?** Regress strategy returns on a constant; one-sided
  $H_1$: mean $>0$. A small $t$ means the backtest profit is indistinguishable
  from luck.
- **Type I in strategy mining:** test 100 random strategies at 5% and about 5
  look significant by chance — data snooping with a number on it.
- **Type II with small samples:** a real modest edge on 3 months of data often
  fails to reject — "not significant" can mean "not enough data yet".
- Beyond finance: poll margins of error and drug-trial effect intervals are the
  same "estimate ± cutoff × SE".

---

## 9. ANOVA approach to simple regression (lecture notes §2.1.5)

**Big idea.** $Y$ varies; ANOVA splits that total variation into the part the
line explains and the part left over. For one point:

$$
\underbrace{Y_i-\bar Y}_{\text{total}}=\underbrace{(\hat Y_i-\bar Y)}_{\text{explained: line vs mean}}+\underbrace{(Y_i-\hat Y_i)}_{\text{residual: point vs line}}
$$

- **TSS** $=\sum(Y_i-\bar Y)^2$ — spread of $Y$ before any model.
- **SSReg** $=\sum(\hat Y_i-\bar Y)^2$ — how far the line departs from just
  predicting $\bar Y$ (explained).
- **SSR** $=\sum(Y_i-\hat Y_i)^2$ — what the line still misses; the quantity
  behind $\hat\sigma^2$.

**Typo (p.27):** $Y_i-(\hat\beta_0-\hat\beta_1X_i)$ should be
$Y_i-(\hat\beta_0+\hat\beta_1X_i)$.

**Why the squares add (the step the notes skip).** Squaring gives a cross
term $2\sum(\hat Y_i-\bar Y)(Y_i-\hat Y_i)$. Since
$\hat Y_i-\bar Y=\hat\beta_1(X_i-\bar X)$, it equals
$2\hat\beta_1\sum(X_i-\bar X)\hat\epsilon_i=0$ by the two normal equations
($\sum\hat\epsilon_i=0$, $\sum X_i\hat\epsilon_i=0$). Intuition: residuals
contain nothing the line could explain — explained and unexplained are at
right angles, so it is Pythagoras: **TSS = SSReg + SSR**.

**Degrees of freedom:** TSS $n-1$ (one used by $\bar Y$), SSReg 1 (one slope),
SSR $n-2$ (two parameters); $n-1=1+(n-2)$.

| Source | df | SS | MS | F |
| :--- | :-: | :-: | :-: | :-: |
| Regression | 1 | SSReg | MSReg = SSReg/1 | $F^*$ = MSReg/MSE |
| Error | $n-2$ | SSR | MSE = SSR/$(n-2)$ | |
| Total | $n-1$ | TSS | | |

**MS = SS ÷ df** (variation per degree of freedom), so
$F^*=\dfrac{\text{explained per parameter}}{\text{noise per spare observation}}$.
If $X$ is useless both estimate noise and $F\approx1$; large $F$ → reject
$\beta_1=0$. **In SLR, $F=t^2$** for the slope, so the F-test and the slope
t-test always agree.

**Q8 check:** SSReg $=\hat\beta_1S_{xy}=0.8929\times13.375=11.94$,
SSR $=1.267$, MSE $=1.267/6=0.211$, so $F=11.94/0.211=56.6$, and
$t^2=7.52^2=56.6$ ✓. $R^2=11.94/13.21=0.904$.

**Applications.** CAPM risk decomposition: SSReg is systematic (market,
undiversifiable) risk, SSR idiosyncratic (diversifiable) risk; $R^2$ is the
market's share of a stock's risk (lecture examples: AAPL ≈ 57%, PFE ≈ 18%).
A trading signal with tiny SSReg relative to SSR (low $F$) explains almost
none of the return variation. In experiments the same split tests whether a
treatment group differs from control — hence "analysis of variance".

---

## 10. Reading an ANOVA table; $R^2$ and adjusted $R^2$ (lecture notes p.28–29)

**Lecturer's example (`aaa_dif ~ cm10_dif`).** SSReg $=11.21$, SSR $=3.81$,
so TSS $=15.02$; df 1 and 878, so $n=880$. MSE $=3.81/878=0.00434$ (R prints
"0.0" from rounding). $F=11.21/0.00434\approx2582$, matching
$t^2=50.82^2$ — simple regression again.

**Coefficient of determination.**

$$
R^2=\frac{\text{SSReg}}{\text{TSS}}=1-\frac{\text{SSR}}{\text{TSS}}=\frac{11.21}{15.02}=0.746
$$

— the 10-yr Treasury rate explains about 75% of the variation in AAA yield
changes. $R=\sqrt{R^2}$ (here $0.864$) is the **coefficient of multiple
correlation**: the correlation between $Y$ and the fitted $\hat Y$; in SLR it
equals $|\text{corr}(X,Y)|$.

**Why plain $R^2$ is a bad comparison tool.** Adding *any* variable, even pure
noise, cannot raise SSR — OLS could set its coefficient to 0 and recover the
old fit — so $R^2$ never falls.

**Adjusted $R^2$** compares sums of squares *per degree of freedom*:

$$
\bar R^2=1-\frac{\text{SSR}/(n-p-1)}{\text{TSS}/(n-1)}=1-(1-R^2)\frac{n-1}{n-p-1}
$$

(SLR: $p=1$, giving the notes' $n-2$ version; **typo p.29:** that formula is
labelled $R^2$ but is the adjusted one). Each variable costs a degree of
freedom; one that barely lowers SSR makes $\text{SSR}/(n-p-1)$ *rise*, so
$\bar R^2$ falls. It rewards only variables that earn their place.

**The penalty depends on $p$ relative to $n$:**

| Model | $R^2$ | $\bar R^2$ | Factor $\frac{n-1}{n-p-1}$ | Drop |
| :--- | :-: | :-: | :-: | :-: |
| AAA ($n=880$, $p=1$) | 0.7463 | 0.7461 | $879/878=1.001$ | 0.0003 |
| Q8 bonds ($n=8$, $p=1$) | 0.904 | 0.888 | $7/6=1.167$ | 0.016 |

The *drop* is about 50 times larger for Q8 because each parameter is a much
bigger share of 8 observations — the degrees-of-freedom idea of
[[02 Data-sets]] §2. (Compare drops, not levels: Q8's $\bar R^2$ is still
higher than AAA's.)

**Applications.** A bot model with 30 indicators shows a high $R^2$ almost
automatically; a $\bar R^2$ well below $R^2$ signals overfitting. When
comparing CAPM with Fama-French 3-factor, compare $\bar R^2$ — FF3 wins plain
$R^2$ regardless, because it has two extra variables.
