# 29 — Derivation of the Black-Scholes-Merton Formula

> The direct integration route: under $\mathbb{Q}$, $S_T$ is log-normal, so the
> discounted expected call payoff $e^{-rT}\,\mathbb{E}^\mathbb{Q}[(S_T-K)^+]$
> splits into two truncated log-normal integrals. The first collapses to
> $S_0\Phi(d_1)$, the second to $Ke^{-rT}\Phi(d_2)$, assembling the celebrated
> $C_0 = S_0\Phi(d_1) - Ke^{-rT}\Phi(d_2)$. A worked numerical example closes the note.

---

## 1. Setup & Definitions

Under the risk-neutral measure $\mathbb{Q}$, the stock price at maturity $T$ follows Geometric Brownian Motion:

$$S_T = S_0 \exp\left( \left(r - \frac{1}{2}\sigma^2\right)T + \sigma\sqrt{T}Z \right), \quad Z \sim \mathcal{N}(0, 1) \text{ under } \mathbb{Q}$$

### Key Variables:

- $S_0$ (or $S_t$): Current share price

- $K$: Strike / exercise price

- $T$: Time to maturity

- $r$: Risk-free interest rate (continuously compounded)

- $\sigma$: Annual volatility

- $\Phi(\cdot)$: Cumulative distribution function (CDF) of standard normal distribution $\mathcal{N}(0,1)$

---

## 2. Risk-Neutral Valuation Formula

The price of a European call option $C_0$ at time $t=0$ is the discounted expected payoff under $\mathbb{Q}$:

$$C_0 = e^{-rT} \mathbb{E}^{\mathbb{Q}}\left[ \max(S_T - K, 0) \right]$$

Expressing this expectation as an integral using the probability density function $f(s)$ of $S_T$:

$$\mathbb{E}^{\mathbb{Q}}\left[ \max(S_T - K, 0) \right] = \int_K^\infty (s - K) f(s) \, ds$$

Split into two separate integrals:

$$\mathbb{E}^{\mathbb{Q}}\left[ \max(S_T - K, 0) \right] = \underbrace{\int_K^\infty s f(s) \, ds}_{\text{1st Integral}} - \underbrace{K \int_K^\infty f(s) \, ds}_{\text{2nd Integral}}$$

---

## 3. Log-Normal Parameters Definition

Since $S_T$ is log-normally distributed, let $X = \ln(S_T) \sim \mathcal{N}(m, v^2)$:

- Mean: $m = \ln(S_0) + \left(r - \frac{1}{2}\sigma^2\right)T$
- Variance: $v^2 = \sigma^2 T \implies v = \sigma\sqrt{T}$

---

## 4. Evaluating the First Integral

Using the **truncated log-normal moment formula** given in actuarial tables:

$$\int_K^\infty s f(s) \, ds = e^{m + \frac{1}{2}v^2} \cdot \Phi\left( \frac{m + v^2 - \ln K}{v} \right)$$

### Step A: Simplify $e^{m + \frac{1}{2}v^2}$

$$e^{m + \frac{1}{2}v^2} = \exp\left( \ln(S_0) + \left(r - \frac{1}{2}\sigma^2\right)T + \frac{1}{2}\sigma^2 T \right) = e^{\ln(S_0) + rT} = S_0 e^{rT}$$

### Step B: Evaluate the argument inside $\Phi(\cdot)$

$$\frac{m + v^2 - \ln K}{v} = \frac{\ln(S_0) + \left(r - \frac{1}{2}\sigma^2\right)T + \sigma^2 T - \ln K}{\sigma\sqrt{T}}$$

$$\frac{m + v^2 - \ln K}{v} = \frac{\ln\left(\frac{S_0}{K}\right) + \left(r + \frac{1}{2}\sigma^2\right)T}{\sigma\sqrt{T}} \equiv d_1$$

Therefore, the first integral becomes:

$$\int_K^\infty s f(s) \, ds = S_0 e^{rT} \Phi(d_1)$$

---

## 5. Evaluating the Second Integral

The second integral represents the probability that the option finishes in-the-money under $\mathbb{Q}$:

$$K \int_K^\infty f(s) \, ds = K \cdot \mathbb{P}^{\mathbb{Q}}(S_T > K) = K \cdot \mathbb{P}^{\mathbb{Q}}(\ln S_T > \ln K)$$

Since $\ln(S_T) \sim \mathcal{N}(m, v^2)$:

$$\mathbb{P}^{\mathbb{Q}}(\ln S_T > \ln K) = \Phi\left( \frac{m - \ln K}{v} \right)$$

Substituting $m$ and $v$:

$$\frac{m - \ln K}{v} = \frac{\ln(S_0) + \left(r - \frac{1}{2}\sigma^2\right)T - \ln K}{\sigma\sqrt{T}} = \frac{\ln\left(\frac{S_0}{K}\right) + \left(r - \frac{1}{2}\sigma^2\right)T}{\sigma\sqrt{T}} \equiv d_2$$

Thus, the second integral is:

$$K \int_K^\infty f(s) \, ds = K \Phi(d_2)$$

> Note that $d_2 = d_1 - \sigma\sqrt{T}$.

---

## 6. Assembling the Final Result

Substitute both evaluated integrals back into the discounted expected payoff formula:

$$C_0 = e^{-rT} \left[ S_0 e^{rT} \Phi(d_1) - K \Phi(d_2) \right]$$

Distribute $e^{-rT}$:

$$C_0 = S_0 e^{-rT} e^{rT} \Phi(d_1) - K e^{-rT} \Phi(d_2)$$

$$\mathbf{C_0 = S_0 \Phi(d_1) - K e^{-rT} \Phi(d_2)}$$

---

## 7. Solution to the Example Problem

**Given values:**

- Current stock price ($S_0$) = $\$65$

- Volatility ($\sigma$) = $0.25$

- Risk-free rate ($r$) = $0.02$

- Strike price ($K$) = $\$55$

- Time to expiry ($T$) = $6 \text{ months} = 0.5 \text{ years}$

### Step 1: Calculate $d_1$ and $d_2$

$$d_1 = \frac{\ln(65/55) + (0.02 + 0.5 \times 0.25^2)(0.5)}{0.25\sqrt{0.5}} = \frac{0.167054 + 0.025625}{0.176777} = \frac{0.192679}{0.176777} \approx \mathbf{1.09}$$

$$d_2 = d_1 - \sigma\sqrt{T} = 1.09 - 0.1768 \approx \mathbf{0.91}$$

### Step 2: Standard Normal Cumulative Probabilities $\Phi(\cdot)$

Using normal distribution tables:

- $\Phi(1.09) \approx 0.8621$

- $\Phi(0.91) \approx 0.8186$

### Step 3: Part (i) - Price of Call Option ($C_0$)

$$C_0 = 65(0.8621) - 55 e^{-(0.02)(0.5)} (0.8186)$$

$$C_0 = 56.0365 - 55(0.99005)(0.8186) = 56.0365 - 44.5752 = \mathbf{\$11.46}$$

### Step 4: Part (ii) - Price of Put Option ($P_0$) via Put-Call Parity

$$P_0 = C_0 + K e^{-rT} - S_0$$

$$P_0 = 11.46 + 55(0.99005) - 65 = 11.46 + 54.45 - 65 = \mathbf{\$0.91}$$

---

**See also:** [[28 Derivation of the Black Scholes PDE .]] · [[27 Stochastic Models of Derivative Prices]] · [[30 Put-Call Parity]]
