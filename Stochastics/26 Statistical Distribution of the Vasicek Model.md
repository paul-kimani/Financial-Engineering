# 26 — Statistical Distribution of the Vasicek Model

> From the closed form we read off the Vasicek conditional law: the mean pulls
> from $r_0$ toward $b$ as $r_0 e^{-at} + b(1 - e^{-at})$, with variance
> $\tfrac{\sigma^2}{2a}(1 - e^{-2at})$. As $t \to \infty$ the rate forgets its
> start and settles into the **stationary distribution**
> $r_\infty \sim N\!\left(b, \tfrac{\sigma^2}{2a}\right)$.

---

## 1. Isolating $r_t$ and Finding the Conditional Mean

This section cleanly wraps up the algebraic isolation of $r_t$ and starts calculating its conditional statistics.

### 1. The Closed-Form Solution

By multiplying our integrated equation through by the discount factor $e^{-at}$, we successfully isolate the short-term interest rate $r_t$ (see [[25 Vasicek Model]]):

$$r_t = r_0 e^{-at} + b(1 - e^{-at}) + \int_0^t \sigma e^{-a(t-s)} dW_s$$

### 2. Deriving the Conditional Mean: $E[r_t \mid r_0]$

We want to find the expected value of our interest rate, given we know where it starts ($r_0$).

Using the **linearity property of expectations**, we split the equation into our deterministic parts and our stochastic integral:

$$E[r_t \mid r_0] = E[r_0 e^{-at} + b(1 - e^{-at}) \mid r_0] + E\left[ \int_0^t \sigma e^{-a(t-s)} dW_s \mid r_0 \right]$$

- **First Term (The Constant):** Since $r_0$ is known, $r_0 e^{-at} + b(1 - e^{-at})$ is treated as a deterministic constant. Its expectation is simply itself:

    $$E[r_0 e^{-at} + b(1 - e^{-at}) \mid r_0] = r_0 e^{-at} + b(1 - e^{-at})$$

- **Second Term (The Noise):** Because $E[dW_s] = 0$, the expectation of any Itô integral is zero:

    $$E\left[ \int_0^t \sigma e^{-a(t-s)} dW_s \mid r_0 \right] = 0$$

Combining these gives the final conditional mean:

$$\mathbf{E[r_t \mid r_0] = r_0 e^{-at} + b(1 - e^{-at})}$$

### 3. Setting Up the Conditional Variance: $\operatorname{Var}(r_t \mid r_0)$

Next, we set up the variance of the entire expression:

$$\operatorname{Var}(r_t \mid r_0) = \operatorname{Var}\left( r_0 e^{-at} + b(1 - e^{-at}) + \int_0^t \sigma e^{-a(t-s)} dW_s \mid r_0 \right)$$

Applying our variance rule ($\operatorname{Var}(Y + C) = \operatorname{Var}(Y)$ for constant $C$), the deterministic term $r_0 e^{-at} + b(1 - e^{-at})$ drops out because its variance is $0$.

Because the remaining stochastic integral is independent of $r_0$, we can drop the conditioning bar:

$$\operatorname{Var}(r_t \mid r_0) = \operatorname{Var}\left( \int_0^t \sigma e^{-a(t-s)} dW_s \right)$$

---

## 2. Calculating the Variance and Finding Long-Term Limits

This section computes the variance integral using Itô Isometry and begins the analysis of how the Vasicek model behaves as time approaches infinity ($t \to \infty$).

### 1. Solving the Variance with Itô Isometry

Applying Itô Isometry ($\operatorname{Var}[\int f(s) dW_s] = \int f(s)^2 ds$):

$$\operatorname{Var}(r_t \mid r_0) = \int_0^t \left( \sigma e^{-a(t-s)} \right)^2 ds = \int_0^t \sigma^2 e^{-2a(t-s)} ds$$

Pulling out the constants and integrating $e^{2as}$:

$$\operatorname{Var}(r_t \mid r_0) = \sigma^2 e^{-2at} \int_0^t e^{2as} ds = \sigma^2 e^{-2at} \left[ \frac{e^{2as}}{2a} \right]_0^t$$

$$\operatorname{Var}(r_t \mid r_0) = \frac{\sigma^2 e^{-2at}}{2a} \left[ e^{2at} - e^0 \right] = \frac{\sigma^2 e^{-2at}}{2a} \left[ e^{2at} - 1 \right]$$

> **A Quick Tip on a Small Typo:** In the fifth line of this page, the notebook has a small slip-up writing $\left[ e^{2at} + 1 \right]$ instead of $\left[ e^{2at} - 1 \right]$. However, you correctly distributed the $e^{-2at}$ in the next step to land on the correct final variance:

$$\mathbf{\operatorname{Var}(r_t \mid r_0) = \frac{\sigma^2}{2a} \left( 1 - e^{-2at} \right)}$$

### 2. Long-Term Statistical Distribution of the Vasicek Model

Since the interest rate $r_t$ is normally distributed, we can summarize its conditional distribution at any time $t$ as:

$$r_t \mid r_0 \sim N\left( r_0 e^{-at} + b(1 - e^{-at}), \; \frac{\sigma^2}{2a} \left( 1 - e^{-2at} \right) \right)$$

Now, we analyze what happens to the interest rate in the long run ($t \to \infty$). Given that our speed of reversion $a > 0$, as $t \to \infty$, the exponential term decays to zero ($e^{-at} \to 0$).

#### I. Long-Term Mean

$$\lim_{t \to \infty} E[r_t \mid r_0] = \lim_{t \to \infty} \left[ r_0 e^{-at} + b(1 - e^{-at}) \right]$$

Since $e^{-at} \to 0$:

$$\lim_{t \to \infty} E[r_t \mid r_0] = r_0(0) + b(1 - 0) = b$$

#### II. Long-Term Variance

Next, we take the limit of the conditional variance as $t \to \infty$:

$$\lim_{t \to \infty} \operatorname{Var}(r_t \mid r_0) = \lim_{t \to \infty} \frac{\sigma^2}{2a} \left( 1 - e^{-2at} \right)$$

Since $e^{-2at} \to 0$ as $t \to \infty$:

$$\lim_{t \to \infty} \operatorname{Var}(r_t \mid r_0) = \frac{\sigma^2}{2a} (1 - 0) = \frac{\sigma^2}{2a}$$

#### III. The Stationary Vasicek Distribution

In the long run, the Vasicek interest rate completely forgets its starting value $r_0$ and stabilizes into a steady-state normal distribution:

$$\boxed{r_\infty \sim N\left( b, \; \frac{\sigma^2}{2a} \right)}$$

- **The Financial Meaning:** No matter where interest rates start today, they will eventually drift toward the long-term average rate $b$. Once they get there, they will continuously fluctuate around $b$ with a stable volatility spread determined by $\frac{\sigma^2}{2a}$.

---

**See also:** [[25 Vasicek Model]] · [[24 Long-Term Statistical Distribution Of the O-U Process]] · [[23 Statistical Distribution of the O-U Process]]
