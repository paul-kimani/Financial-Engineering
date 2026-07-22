# 15 — Let's Understand the Brownian Motion under $\mathbb{Q}$

> A $\mathbb{Q}$-Brownian motion $W_t^\mathbb{Q}$ and a physical Brownian motion
> $W_t^\mathbb{P}$ are *both* standard Brownian motions — the difference is only
> in the probabilities assigned to their paths. Girsanov's theorem supplies the
> exact shift, $dW_t^\mathbb{Q} = dW_t^\mathbb{P} + \theta\,dt$ with market price
> of risk $\theta = \tfrac{\mu-r}{\sigma}$, which is what converts a $\mu$-drift
> into the risk-free $r$-drift.

---

To understand what a **$\mathbb{Q}$-Brownian motion** (often written as $W_t^\mathbb{Q}$) is and how it differs from a "normal" (or physical) Brownian motion ($W_t^\mathbb{P}$), we have to look at how we adjust for risk in financial markets.

Mathematically, **they are both standard Brownian motions** within their respective universes. The difference lies entirely in the **probabilities** we assign to their paths.

---

## 1. The Core Definition

- **The "Normal" Brownian Motion ($W_t^\mathbb{P}$):** This is the driver of randomness in the **real world** (under the physical probability measure $\mathbb{P}$). It is what we actually observe when we look at historical asset charts.

- **The $\mathbb{Q}$-Brownian Motion ($W_t^\mathbb{Q}$):** This is the driver of randomness in the **risk-neutral world** (under the pricing probability measure $\mathbb{Q}$). It is an artificial construct we use to price derivatives.

If you look at a single, isolated path of a Brownian motion without any context, $W_t^\mathbb{P}$ and $W_t^\mathbb{Q}$ look identical. They both start at 0, have continuous paths, and have independent, normally distributed increments:

$$(W_t - W_s) \sim N(0, t-s)$$

The difference only appears when we look at the **drift of the asset** they are driving.

---

## 2. The Difference: Girsanov's Theorem

The relationship between $W_t^\mathbb{P}$ and $W_t^\mathbb{Q}$ is governed by **Girsanov's Theorem**, which is the mathematical bridge used to change probability measures (developed fully in [[16 Equivalent Probability Measures and Girsanov's Theorem]]).

In the real world ($\mathbb{P}$), a stock price $S_t$ has a high drift $\mu$ because investors demand a risk premium:

$$dS_t = \mu S_t dt + \sigma S_t dW_t^\mathbb{P}$$

In the risk-neutral world ($\mathbb{Q}$), we want the same stock to drift at the risk-free rate $r$. To force this change mathematically _without altering the stock's volatility ($\sigma$)_, we must shift the Brownian motion. Girsanov's Theorem defines this shift as:

$$dW_t^\mathbb{Q} = dW_t^\mathbb{P} + \left( \frac{\mu - r}{\sigma} \right) dt$$

The term $\theta = \frac{\mu - r}{\sigma}$ is known as the **Market Price of Risk** (or the Sharpe ratio of the asset).

If we rearrange this equation:

$$dW_t^\mathbb{P} = dW_t^\mathbb{Q} - \left( \frac{\mu - r}{\sigma} \right) dt$$

If we substitute this $dW_t^\mathbb{P}$ back into our real-world SDE, we get:

$$dS_t = \mu S_t dt + \sigma S_t \left( dW_t^\mathbb{Q} - \left( \frac{\mu - r}{\sigma} \right) dt \right)$$

$$dS_t = \mu S_t dt + \sigma S_t dW_t^\mathbb{Q} - (\mu - r) S_t dt$$

$$dS_t = r S_t dt + \sigma S_t dW_t^\mathbb{Q}$$

By switching our reference frame to the $\mathbb{Q}$-Brownian motion, the subjective drift $\mu$ has completely vanished, replaced by the risk-free rate $r$.

---

## 3. How to Visualize the Difference

Think of changing from $\mathbb{P}$ to $\mathbb{Q}$ like **walking on a moving walkway** at an airport:

- **The Real-World ($\mathbb{P}$):** You are walking forward at your normal pace ($W_t^\mathbb{P}$), but the walkway is also moving forward quickly under your feet (the high drift $\mu$). To an observer standing off the walkway, you are flying forward.
- **The Risk-Neutral World ($\mathbb{Q}$):** The walkway slows down significantly to a slow crawl (the risk-free rate $r$). You are still walking with the exact same step-to-step variability and style ($W_t^\mathbb{Q}$), but because the ground beneath you has slowed down, your overall forward progress is much slower.

---

## Summary Comparison

| **Feature**                 | **Physical/Normal Brownian Motion ($W_t^\mathbb{P}$)** | **Risk-Neutral Brownian Motion ($W_t^\mathbb{Q}$)**                              |
| --------------------------- | -------------------------------------------------- | -------------------------------------------------------------------------------- |
| **Probability Measure**     | Real-world ($\mathbb{P}$)                          | Risk-neutral ($\mathbb{Q}$)                                                      |
| **Associated Asset Drift**  | $\mu$ (includes risk premium)                      | $r$ (risk-free rate)                                                             |
| **Path Probability**        | Upward-trending paths are highly likely.           | Downward-pointing or flatter paths are given more weight than in the real world. |
| **Used For**                | Risk management, forecasting, historical analysis. | Pricing options, constructing replicating portfolios.                            |
| **Mathematical properties** | Standard Brownian motion under $\mathbb{P}$        | Standard Brownian motion under $\mathbb{Q}$                                      |

---

## 4. Why Only the Drift Changes

The key fact that makes all of this work: **the change of measure shifts the drift but leaves the volatility $\sigma$ untouched.** Mathematically, $W_t^\mathbb{Q}$ and $W_t^\mathbb{P}$ have the *same* quadratic variation, $[W]_t = t$, and by Lévy's characterisation any continuous martingale with that quadratic variation is a standard Brownian motion. Girsanov's theorem only re-weights *which paths are likely*, so the "size" of the random shocks — the diffusion coefficient — is invariant. This is why volatility can be estimated from real-world ($\mathbb{P}$) data (see [[12 - GBM Parameter Estimation]]) and then used directly for $\mathbb{Q}$-pricing, whereas the drift cannot.

---

## Connections

**Within Stochastics**
- [[01 - Introduction to Stochastic Processes]] — the defining properties of the Wiener process shared by $W_t^\mathbb{P}$ and $W_t^\mathbb{Q}$.
- [[16 Equivalent Probability Measures and Girsanov's Theorem]] — the theorem that makes the shift $dW_t^\mathbb{Q} = dW_t^\mathbb{P} + \theta\,dt$ rigorous.
- [[17 Example application of Girsanov's Theorem]] — the shift applied end-to-end to a stock SDE.
- [[12 - GBM Parameter Estimation]] — why $\sigma$ (but not $\mu$) carries over from $\mathbb{P}$ to $\mathbb{Q}$.

**Across the programme**
- [[../Financial Theory/11 - Risk Aversion - Certainty Equivalent and Risk Premium]] — the market price of risk $\theta$ as the compensation risk-averse investors demand.
