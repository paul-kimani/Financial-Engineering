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
