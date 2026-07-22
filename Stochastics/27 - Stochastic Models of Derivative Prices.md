# 27 — Stochastic Models of Derivative Prices

> The bridge from the martingale machinery to the models everyone actually
> quotes. Two frameworks dominate: the discrete-time **Binomial (BOPM)** and the
> continuous-time **Black–Scholes (BSM)**. This chapter lists the BSM
> assumptions — GBM prices, constant $\sigma$ and $r$, no dividends, frictionless
> no-arbitrage markets — and states the call/put formulae with their $d_1, d_2$.

---

## 1. Overview of Stochastic Models

The notes categorize derivative pricing into two foundational frameworks:

- **Binomial Option Pricing Model (BOPM):** A discrete-time model where price movements occur in steps.

- **Black-Scholes Model (BSM):** A continuous-time model where price dynamics evolve smoothly and continuously over time using stochastic differential equations (SDEs).

---

## 2. Assumptions of the Black-Scholes Model

The notes divide the classical Black-Scholes assumptions into **Asset Assumptions** and **Market Assumptions**.

### Assumptions Made on the Assets (Risky & Riskless Assets)

1. **Lognormal Distribution / Geometric Brownian Motion (GBM):** Stock returns are assumed to be normally distributed, meaning asset prices $S_t$ follow a lognormal distribution and satisfy:

$$dS_t = \mu S_t dt + \sigma S_t dW_t$$

2. **Constant Volatility ($\sigma$):** The volatility of asset returns is assumed to be deterministic, constant, and fully known over the option's lifespan.

3. **Constant Risk-Free Rate ($r$):** The risk-free rate is constant and known over the entire term, corresponding to a continuously compounded risk-free bond.

4. **No Dividends:** The underlying asset pays no continuous or discrete dividends during the option's life.

5. **Continuous Price Movement (No Jumps):** Price dynamics contain no jump processes or sudden gaps (pure diffusion process).

6. **Equal Borrowing and Lending Rates:** Investors can borrow and lend cash at the same risk-free rate $r$.

### Assumptions Made on the Market

1. **Frictionless Market:** No transaction costs, brokerage fees, or taxes.

2. **No Arbitrage Opportunities:** The market is assumed to be efficient, ruling out risk-free profit higher than $r$.

3. **Perfect Liquidity:** Assets are infinitely divisible, and trading occurs continuously without market impact.

4. **Unrestricted Short Selling:** Short selling is permitted with full use of short-sale proceeds.

5. **Strictly European Options:** Options can only be exercised at maturity $T$ (no early exercise prior to expiry).

---

## 3. Black-Scholes Formulae

For an option with current spot price $S_t$, strike price $K$, risk-free rate $r$, time-to-maturity $T-t$, and standard normal cumulative distribution function $N(\cdot)$:

### European Call Option Price ($C_t$)

$$C_t = S_t N(d_1) - K e^{-r(T-t)} N(d_2)$$

### European Put Option Price ($P_t$)

$$P_t = K e^{-r(T-t)} N(-d_2) - S_t N(-d_1)$$

where:

- **$d_1$** measures the risk-adjusted probability of the option finishing in-the-money plus a delta-hedging multiplier:

    $$d_1 = \frac{\ln\left(\frac{S_t}{K}\right) + \left(r + \frac{\sigma^2}{2}\right)(T-t)}{\sigma \sqrt{T-t}}$$

- **$d_2$** is related to $d_1$ by subtracting the total volatility drift over time:

    $$d_2 = d_1 - \sigma \sqrt{T-t}$$

---

## Summary of Key Terms

|**Variable**|**Definition**|
|---|---|
|**$S_t$**|Current price of the underlying asset|
|**$K$**|Exercise / Strike price|
|**$r$**|Continuously compounded risk-free rate|
|**$T - t$**|Time remaining until option expiration|
|**$\sigma$**|Volatility of the underlying asset returns|
|**$N(d)$**|Cumulative distribution function (CDF) of standard normal $\mathcal{N}(0,1)$|

---

## 4. Two Models, One Limit

The discrete BOPM and the continuous BSM are not rivals — they are the **same model at two resolutions**. As the number of binomial steps $n \to \infty$ (with up/down factors $u = e^{\sigma\sqrt{\Delta t}}$, $d = 1/u$ and risk-neutral probability $q = \frac{e^{r\Delta t}-d}{u-d}$), the binomial price converges to the Black–Scholes price. The $N(d_1), N(d_2)$ terms are the Gaussian limit of the binomial's cumulative distribution, by the Central Limit Theorem. This is why the [[19 - The self financing Portfolio|self-financing rebalancing]] argument works identically in both settings, and why practitioners reach for the tractable BSM for European options but keep the lattice for early-exercise (American) features.

---

## Connections

**Within Stochastics**
- [[10 - Geometric Brownian Motion]] — the GBM assumption underpinning both models.
- [[05 - Diffusion Processes Catalogue]] — GBM's assumptions in the context of other diffusions.
- [[28 - Derivation of the Black Scholes PDE]] · [[29 - Derivation of The Black Scholes merton formula]] — the two derivations of the BSM formulae stated here.
- [[19 - The self financing Portfolio]] — the replication argument shared by both models.

**Across the programme**
- [[../Derivatives/7 May 2026 - Personal Notes 1]] — the option contracts these models price.
- [[../Derivatives/Lehman Brothers 2007]] — what the "frictionless, no-jumps" assumptions cost when they fail.
- [[../Financial Theory/13 - Efficient Market Hypothesis]] — the no-arbitrage/efficiency assumptions listed here, from the theory side.
