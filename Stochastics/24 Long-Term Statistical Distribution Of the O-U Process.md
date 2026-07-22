# 24 — Long-Term Statistical Distribution of the O-U Process

> Taking $t \to \infty$ in the conditional law strips away the starting value:
> the mean decays to $0$ and the variance settles at $\tfrac{\sigma^2}{2\theta}$.
> The process reaches a **stationary distribution**
> $X_\infty \sim N\!\left(0, \tfrac{\sigma^2}{2\theta}\right)$ — mean reversion
> and random shocks reach a permanent tug-of-war balance.

---

This page of notes is highly satisfying—it takes the conditional statistics we just derived and calculates the **long-term (limiting or asymptotic) distribution** of the Ornstein-Uhlenbeck process by taking the mathematical limit as time goes to infinity ($t \to \infty$).

Let's go through this final piece of the puzzle step-by-step.

---

## 1. The Baseline O-U Conditional Distribution

At the top of the page, we summarize the conditional distribution we just finished building (in [[23 Statistical Distribution of the O-U Process]]):

$$X_t \mid X_0 \sim N\left( X_0 e^{-\theta t}, \frac{\sigma^2}{2\theta} \left( 1 - e^{-2\theta t} \right) \right)$$

Now, we investigate what happens in the "long run" as $t \to \infty$.

---

## 2. Long-Term Mean

First, we analyze the limit of the expected value of our process as time stretches to infinity:

$$\lim_{t \to \infty} E[X_t \mid X_0] = \lim_{t \to \infty} X_0 e^{-\theta t}$$

- **The Key Behavior:** Since the mean-reversion rate $\theta > 0$, the exponent $-\theta t$ becomes a larger and larger negative number as $t$ grows.

- Because $e^{-\infty} = 0$, we have:

    $$e^{-\theta t} \to 0 \quad \text{as} \quad t \to \infty$$

- Substituting this back into our limit calculation:

    $$\lim_{t \to \infty} E[X_t \mid X_0] = X_0 \cdot 0 = 0$$

### The Financial Meaning:

If this O-U process is modeling a variable like a short-term interest rate, this result means that **in the long run, the interest rate will revert back to its long-term average, which is zero in this generalized model.** The memory of where the rate started ($X_0$) fades away completely.

---

## 3. Long-Term Variance

Next, we apply the same limit to the conditional variance of the process:

$$\lim_{t \to \infty} \operatorname{Var}(X_t \mid X_0) = \lim_{t \to \infty} \frac{\sigma^2}{2\theta} \left( 1 - e^{-2\theta t} \right)$$

- **The Key Behavior:** Just like before, since $2\theta > 0$, the negative exponent term decays to zero:

    $$e^{-2\theta t} \to 0 \quad \text{as} \quad t \to \infty$$

- Substituting this zero into our limit equation:

    $$\lim_{t \to \infty} \operatorname{Var}(X_t \mid X_0) = \lim_{t \to \infty} \frac{\sigma^2}{2\theta} \left( 1 - 0 \right) = \frac{\sigma^2}{2\theta}$$

### The Financial Meaning:

Even though the process always tries to pull back to its mean of $0$, it is constantly being buffeted by random market shocks (the $\sigma dW_t$ term).

As time goes on, the uncertainty doesn't grow to infinity (unlike a standard Brownian motion). Instead, the speed of mean reversion ($\theta$) and the strength of the random shocks ($\sigma$) reach a perfect tug-of-war balance. The long-term spread of the process's possible values stabilizes at a fixed variance of:

$$\frac{\sigma^2}{2\theta}$$

---

## 4. Final Summary: The Stationary Distribution

This page of notes concludes the entire mathematical journey of the Ornstein-Uhlenbeck process. In the long run, the process is no longer dependent on its starting value $X_0$ and reaches a steady, **stationary distribution**:

$$\boxed{X_\infty \sim N\left( 0, \frac{\sigma^2}{2\theta} \right)}$$

---

## 5. Stationarity, Ergodicity, and the AR(1) Link

Reaching a limiting law that is independent of the start $X_0$ is exactly the property of **stationarity**. Because the O-U process is also **ergodic**, long *time averages* of a single path converge to this *ensemble* average $N(0, \tfrac{\sigma^2}{2\theta})$ — which is what makes it possible to estimate the parameters from one historical trajectory. Contrast this sharply with [[10 - Geometric Brownian Motion|GBM]], whose variance grows without bound and which is therefore **non-stationary**: GBM wanders off, the O-U process settles down. The discrete mirror of this whole story is the stationarity condition $\lvert\phi\rvert < 1$ for an [[../Time Series/10 April 2026|AR(1)]] series, with $\phi = e^{-\theta\Delta}$ automatically in $(0,1)$ whenever $\theta > 0$.

---

## Connections

**Within Stochastics**
- [[23 Statistical Distribution of the O-U Process]] — the finite-time law whose limit is taken here.
- [[10 - Geometric Brownian Motion]] — the non-stationary counterpoint.
- [[25 Vasicek Model]] · [[26 Statistical Distribution of the Vasicek Model]] — the same limit, mean-reverting to $b$ instead of $0$.

**Across the programme**
- [[../Time Series/10 April 2026]] — stationarity and the $\lvert\phi\rvert<1$ condition for AR(1).
- [[../Fixed Income Securities/29 April online excercise]] — mean-reverting rate models used in short-rate trees.
