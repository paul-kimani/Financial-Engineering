# 16 — Equivalent Probability Measures and Girsanov's Theorem

> The two pillars that make risk-neutral pricing legal. **Equivalence** guarantees
> that changing measure only re-weights paths, never invents or destroys them
> (same null sets). **Girsanov's theorem** (Cameron–Martin–Girsanov) is the
> machine that performs the shift, absorbing the drift $\mu$ into the risk-free
> rate $r$ via the market price of risk $\tfrac{\mu-r}{\sigma}$.

---

Here, we will layout the foundational mathematical theory that makes risk-neutral pricing possible: **Equivalent Probability Measures** and **Girsanov's Theorem** (specifically the Cameron-Martin-Girsanov version).

---

## 1. Equivalent Probability Measures

Before changing from the real-world measure $\mathbb{P}$ to the risk-neutral measure $\mathbb{Q}$, we must ensure we don't accidentally break the laws of probability. This is where **equivalence** comes in.

### The Definition

Two probability measures, $\mathbb{P}$ and $\mathbb{Q}$, defined on the same sample space and event structure (the $\sigma$-algebra $\mathcal{F}$), are **equivalent** if and only if they agree on what is impossible and what is possible.

Mathematically, for any event $E$:

1. $\mathbb{P}(E) > 0 \iff \mathbb{Q}(E) > 0$ _(If it can happen in one world, it can happen in the other.)_

2. $\mathbb{P}(E) = 0 \iff \mathbb{Q}(E) = 0$ _(If it is impossible in one world, it is impossible in the other.)_

### Why is this crucial?

If we didn't have equivalence, we could change measures and suddenly make an impossible event (like a stock price dropping below $0) possible. Equivalence ensures that **we only shift the probabilities (the weights) of the paths, not the paths themselves.**

---

## 2. Girsanov's Theorem

Let's introduce the mathematical machine that actually performs the shift from $\mathbb{P}$ to $\mathbb{Q}$: **Girsanov's Theorem**.

### Stating the Cameron-Martin-Girsanov Theorem

Its states:

- Let $Z_t$ be a **Standard Brownian Motion (SBM)** under the real-world measure $\mathbb{P}$.

- Let $u_s$ (at times written as $\gamma_s$ ) be a **predictable process** (meaning its value at time $s$ is known from the history up to that point).

Girsanov's Theorem states that there exists an equivalent probability measure $\mathbb{Q}$ such that the shifted process $\tilde{Z}_t$, defined by:

$$\tilde{Z}_t = Z_t + \int_0^t u_s ds$$

is a **Standard Brownian Motion under $\mathbb{Q}$**.

### The Financial Application

In finance, the predictable process we use is the constant **Market Price of Risk** (or Sharpe ratio):

$$u_t = \frac{\mu - r}{\sigma}$$

This measures how much excess return ($\mu - r$) investors demand per unit of volatility ($\sigma$). By integrating this constant over time, the relationship becomes:

$$\tilde{Z}_t = Z_t + \left(\frac{\mu - r}{\sigma}\right)t$$

---

## 3. Application in Finance & Summary

Let's summarizes how we apply this shift to option pricing and interest rate modeling.

### Key Takeaway

By changing the measure from $\mathbb{P}$ to $\mathbb{Q}$, Girsanov's Theorem allows us to **completely absorb and eliminate the drift ($\mu$)** of a stock price process, replacing it with the risk-free rate $r$. Once the drift is adjusted to $r$, any discounted asset price becomes a **martingale**, allowing us to easily compute fair option prices.

### Summary of Girsanov Results

The table below is a a cheat sheet for translating between the two worlds:

|**Framework**|**Probability Measure**|**Standard Brownian Motion (SBM)**|**Brownian Increment**|**Relationship**|
|---|---|---|---|---|
|**Real-World**|$\mathbb{P}$|$Z_t$|$dZ_t$|$\tilde{Z}_t = Z_t + \left(\frac{\mu - r}{\sigma}\right)t$|
|**Risk-Neutral World**|$\mathbb{Q}$|$\tilde{Z}_t$|$d\tilde{Z}_t$|$d\tilde{Z}_t = dZ_t + \left(\frac{\mu - r}{\sigma}\right)dt$|

### What this means in practice:

If you are working in the real world ($\mathbb{P}$) and want to switch to the risk-neutral world ($\mathbb{Q}$), you simply substitute $dZ_t$ with:

$$dZ_t = d\tilde{Z}_t - \left(\frac{\mu - r}{\sigma}\right)dt$$

When you plug this into the real-world stock SDE ($dS_t = \mu S_t dt + \sigma S_t dZ_t$), the subjective drift $\mu$ cancels out perfectly, leaving you with the risk-neutral dynamics:

$$dS_t = r S_t dt + \sigma S_t d\tilde{Z}_t$$

---

**See also:** [[15 Lets understand the brownian motion under Q]] · [[17 Example application of Girsanov's Theorem]] · [[18 Previsible Process and Martingale Representation Theorem]]
