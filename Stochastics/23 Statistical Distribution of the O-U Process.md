# 23 — Statistical Distribution of the O-U Process

> With the closed-form solution in hand, we read off the **conditional law** of
> the Ornstein–Uhlenbeck process. The mean decays geometrically to zero,
> $E[X_t \mid X_0] = X_0 e^{-\theta t}$, and Itô isometry gives the variance
> $\tfrac{\sigma^2}{2\theta}(1 - e^{-2\theta t})$ — so $X_t \mid X_0$ is Gaussian.
> This is the theoretical foundation for mean-reverting rate models like Vasicek.

---

These sections cover a great, concrete example of applying **Itô Isometry** to compute the variance of a specific stochastic integral, followed by the formal derivation of the **Conditional Expectation** (and the **Conditional Variance**) of the Ornstein-Uhlenbeck (O-U) process.

---

## 1. Practical Example: Computing the Variance of $\int_0^T t dW_t$

This section demonstrates how to use **Itô Isometry** to easily solve a stochastic variance problem.

### The Problem

Let $W_t$ be a standard Brownian motion, and define the Itô integral:

$$I(t) = \int_0^T t dW_t$$

We want to find its mean and variance, and specifically evaluate the variance for the interval $T = 1$.

### Finding the Mean

Because the expectation of any stochastic integral driven by a Wiener process is zero:

$$E[I(t)] = E\left[\int_0^T t dW_t\right] = 0$$

### Finding the Variance (Using Itô Isometry)

Applying Itô Isometry allows us to convert this stochastic variance into a deterministic Riemann integral by squaring the integrand ($f(t) = t$) and replacing $dW_t$ with $dt$:

$$\operatorname{Var}(I(t)) = \operatorname{Var}\left(\int_0^T t dW_t\right) = \int_0^T t^2 dt$$

To find the specific variance on the interval $[0, 1]$ (where $T = 1$):

$$\operatorname{Var}\left(\int_0^1 t dW_t\right) = \int_0^1 t^2 dt$$

Using the basic power rule of integration ($\int t^2 dt = \frac{t^3}{3}$):

$$\operatorname{Var}\left(\int_0^1 t dW_t\right) = \left[ \frac{t^3}{3} \right]_0^1 = \frac{1^3}{3} - \frac{0^3}{3} = \frac{1}{3}$$

---

## 2. Statistical Distribution of the O-U Process

This section formally derives the conditional properties of the Ornstein-Uhlenbeck process, starting from its general solution:

$$X_t = X_0 e^{-\theta t} + \int_0^t \sigma e^{-\theta(t-s)} dW_s$$

### A. Conditional Expectation: $E[X_t \mid X_0]$

We want to find the expected value of $X_t$ given that we know its starting value at time zero ($X_0$).

Substitute the general solution into the conditional expectation:

$$E[X_t \mid X_0] = E\left[ X_0 e^{-\theta t} + \int_0^t \sigma e^{-\theta(t-s)} dW_s \mid X_0 \right]$$

By the linearity of expectations, we can split this into two parts:

$$E[X_t \mid X_0] = E\left[ X_0 e^{-\theta t} \mid X_0 \right] + E\left[ \int_0^t \sigma e^{-\theta(t-s)} dW_s \mid X_0 \right]$$

To evaluate these two terms, we use basic probability properties:

1. **The Deterministic Term:** Since $X_0$ and the exponential term $e^{-\theta t}$ are completely known (deterministic) at time $t$, they act as constants. The expectation of a constant is simply the constant itself ($\because E(c) = c$):

    $$E\left[ X_0 e^{-\theta t} \mid X_0 \right] = X_0 e^{-\theta t}$$

2. **The Stochastic Integral Term:** Because the increments of the Wiener process ($dW_s$) are entirely independent of the starting value $X_0$, the conditioning on $X_0$ drops out. Since it's a standard Itô integral, its unconditional expectation is zero ($\because E[dW_s] = 0$):

    $$E\left[ \int_0^t \sigma e^{-\theta(t-s)} dW_s \mid X_0 \right] = E\left[ \int_0^t \sigma e^{-\theta(t-s)} dW_s \right] = 0$$

Combining these, we get:

$$E[X_t \mid X_0] = X_0 e^{-\theta t}$$

### B. Conditional Variance: $\operatorname{Var}(X_t \mid X_0)$

To calculate how much $X_t$ varies around its expected path, we set up the conditional variance:

$$\operatorname{Var}(X_t \mid X_0) = \operatorname{Var}\left( X_0 e^{-\theta t} + \int_0^t \sigma e^{-\theta(t-s)} dW_s \mid X_0 \right)$$

#### Key Simplification Rule

As the note at the bottom points out, because $X_0 e^{-\theta t}$ is a constant when we condition on $X_0$, **its variance is zero**. Adding a constant to a random variable shifts its position but does not alter its volatility or variance ($\operatorname{Var}(Y + c) = \operatorname{Var}(Y)$).

Therefore, the first term disappears, leaving us to calculate only the variance of the stochastic integral:

$$\operatorname{Var}(X_t \mid X_0) = \operatorname{Var}\left( \int_0^t \sigma e^{-\theta(t-s)} dW_s \mid X_0 \right)$$

### Step 1: Apply Itô Isometry

Just like in the previous example with $\int t dW_t$, we use **Itô Isometry** to convert this stochastic variance into a deterministic Riemann integral. We square the inside function and change $dW_s$ to $ds$:

$$\operatorname{Var}(X_t \mid X_0) = \int_0^t \left( \sigma e^{-\theta(t-s)} \right)^2 ds$$

### Step 2: Expand and Simplify the Integrand

Square the terms inside the integral:

$$\operatorname{Var}(X_t \mid X_0) = \int_0^t \sigma^2 e^{-2\theta(t-s)} ds$$

Now, separate the exponent $e^{-2\theta(t-s)}$ into two parts: $e^{-2\theta t} \cdot e^{2\theta s}$.

Since we are integrating with respect to $s$, the $e^{-2\theta t}$ part acts as a constant, and we can pull both $\sigma^2$ and $e^{-2\theta t}$ completely out to the front of the integral:

$$\operatorname{Var}(X_t \mid X_0) = \sigma^2 e^{-2\theta t} \int_0^t e^{2\theta s} ds$$

### Step 3: Evaluate the Deterministic Integral

Now we just have a standard, first-year calculus integral to solve. The anti-derivative of $e^{2\theta s}$ is $\frac{1}{2\theta}e^{2\theta s}$:

$$\operatorname{Var}(X_t \mid X_0) = \sigma^2 e^{-2\theta t} \left[ \frac{1}{2\theta} e^{2\theta s} \right]_0^t$$

Evaluate it at the upper bound $t$ and lower bound $0$:

$$\operatorname{Var}(X_t \mid X_0) = \sigma^2 e^{-2\theta t} \left( \frac{e^{2\theta t}}{2\theta} - \frac{e^0}{2\theta} \right)$$

Since $e^0 = 1$, we can factor out the $\frac{1}{2\theta}$ to the front:

$$\operatorname{Var}(X_t \mid X_0) = \frac{\sigma^2 e^{-2\theta t}}{2\theta} \left( e^{2\theta t} - 1 \right)$$

### Step 4: The Final Clean-Up

Finally, distribute that $e^{-2\theta t}$ back inside the parentheses to cancel out the exponents ($e^{-2\theta t} \cdot e^{2\theta t} = e^0 = 1$):

$$\operatorname{Var}(X_t \mid X_0) = \frac{\sigma^2}{2\theta} \left( 1 - e^{-2\theta t} \right)$$

### Conclusion of the O-U Distribution

You now have the two parameters that completely define the conditional distribution of the Ornstein-Uhlenbeck process! Because it is driven by a Brownian motion, it is normally distributed:

$$X_t \mid X_0 \sim N\left( X_0 e^{-\theta t}, \frac{\sigma^2}{2\theta} \left( 1 - e^{-2\theta t} \right) \right)$$

This successfully builds the theoretical foundation for mean-reverting financial models (like the Vasicek interest rate model).

---

**See also:** [[22.1 Properties of Ito Integral And the O-U Process.]] · [[24 Long-Term Statistical Distribution Of the O-U Process]] · [[25 Vasicek Model]]
