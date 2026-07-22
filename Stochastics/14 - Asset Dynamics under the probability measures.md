# 14 — Asset Dynamics under the Probability Measures

> We fix the two traded assets of the standard market model — a risky
> **stock** $S_t$ and a risk-free **cash account** $B_t = e^{rt}$ — and confirm
> that, once discounted, both are **martingales** under the risk-neutral
> measure $\mathbb{Q}$. This martingale property, $E^\mathbb{Q}[S_T e^{-rT}\mid\mathcal{F}_t] = S_t e^{-rt}$,
> is the engine of risk-neutral valuation.

---

We formally define the two primary traded assets in a standard market model—a risky stock and a risk-free cash account (bond)—and we can confirm their martingale properties under the risk-neutral measure $\mathbb{Q}$ after discounting.

Let's break this down.

---

## 1. Probability Measures: Real-world ($\mathbb{P}$) vs. Risk-Neutral ($\mathbb{Q}$)

We begin by introducing the two probability measures (developed in [[13 - Probability Measures]]):

- **$\mathbb{P}$ (Real-world Probability Measure):** Represents the actual physical probabilities of market moves, where risky assets have a subjective growth rate (drift) $\mu$ that includes a risk premium.

- **$\mathbb{Q}$ (Risk-Neutral Probability Measure):** An artificial probability measure where investors are assumed to be indifferent to risk. Under $\mathbb{Q}$, all assets grow at the risk-free rate $r$.

---

## 2. Discounted Value of an Underlying Asset

This section introduces the discounted asset process, which we denote as $D_t$ (or $\hat{S}_t$ as we did in our previous proof).

### The Formula

$$D_t = S_t e^{-rt} = \frac{S_t}{B_t}$$

- $S_t$ is the stock price at time $t$.

- $B_t = e^{rt}$ is the cash account (or bond) value at time $t$.

- Multiplying by $e^{-rt}$ (or dividing by $B_t$) "discounts" the future value back to time $0$ to adjust for the time value of money.

### The Martingale Property

We need to state the mathematical condition for $D_t$ to be a martingale under the risk-neutral filtration $\mathcal{F}_t$:

$$E^\mathbb{Q}\left[ \frac{S_T}{B_T} \;\middle|\; \mathcal{F}_t \right] = \frac{S_t}{B_t}$$

Rewriting this using the discount factor $e^{-rt}$:

$$E^\mathbb{Q}\left[ S_T e^{-rT} \;\middle|\; \mathcal{F}_t \right] = S_t e^{-rt}$$

This states that **the best estimate of the future discounted stock price at time $T$, given everything we know up to time $t$, is simply its discounted price today.** This is the core condition for risk-neutral valuation.

---

## 3. Discounted Value of a Cash Account (Bond)

Here, We can apply the same discounting logic to the risk-free cash account $B_t$ itself.

### The Formulation

The value of a cash account starting with $\$1$ at time $0$ and growing at a continuous risk-free rate $r$ is:

$$B_t = e^{rt}$$

If we discount this cash account by multiplying it by $e^{-rt}$ (which is equivalent to dividing it by itself), we get a constant:

$$B_t e^{-rt} = e^{rt} \cdot e^{-rt} = e^0 = 1$$

### The Martingale Property

Since the discounted cash account is always exactly $1$ at any point in time, its future value is entirely deterministic (non-random). Therefore, taking its conditional expectation is trivial:

$$E^\mathbb{Q}\left[ B_T e^{-rT} \;\middle|\; \mathcal{F}_t \right] = B_t e^{-rt} = 1$$

Because $1 = 1$, the discounted cash account is trivially a martingale (specifically, a constant martingale).

---

## 4. Asset Dynamics under $\mathbb{Q}$

We can proceed to define how the price of the stock ($S_t$) and the risk-free bond ($B_t$) evolve over time using Stochastic Differential Equations (SDEs) under the risk-neutral measure $\mathbb{Q}$.

### A. The Stock Price ($S_t$)

$$dS_t = r S_t dt + \sigma S_t dW_t^\mathbb{Q}$$

- **$r S_t dt$ (Drift):** Because we are under the risk-neutral measure $\mathbb{Q}$, the stock's expected rate of return is exactly the risk-free rate $r$. There is no subjective risk premium ($\mu$) here.

- **$\sigma S_t dW_t^\mathbb{Q}$ (Diffusion):** Represents the random, volatile fluctuations of the stock price, driven by the $\mathbb{Q}$-Brownian motion ($W_t^\mathbb{Q}$ — see [[15 - Lets understand the brownian motion under Q]]).

### B. The Bond / Cash Account ($B_t$)

$$dB_t = r B_t dt$$

- This is a standard deterministic ordinary differential equation (ODE). There is no $dW_t$ term because the cash account is risk-free; it has no volatility ($\sigma = 0$).
- Solving this differential equation by separating variables ($\frac{dB_t}{B_t} = r dt$) and integrating yields the continuous compounding formula we saw above: $B_t = B_0 e^{rt}$.

---

## 5. The Cash Account as Numéraire

Dividing by $B_t = e^{rt}$ is more than a discounting convenience — $B_t$ is acting as the **numéraire**, the unit in which all other prices are quoted. The martingale statement

$$\frac{S_t}{B_t} = E^\mathbb{Q}\!\left[ \frac{S_T}{B_T} \mid \mathcal{F}_t \right]$$

says that *prices measured in units of the money-market account have no drift under $\mathbb{Q}$*. This is the "fair-game" property of a [[09 - Martingales|martingale]]: the best forecast of tomorrow's normalised price is today's. Different choices of numéraire (a bond, a foreign currency) give different equivalent martingale measures — the idea that underpins currency-swap and quanto pricing in [[../Derivatives/Swaps/04 Currency Swaps - Introduction]].

---

## Connections

**Within Stochastics**
- [[09 - Martingales]] — the formal definition of the martingale property invoked here.
- [[13 - Probability Measures]] — where $\mathbb{P}$ and $\mathbb{Q}$ are introduced.
- [[15 - Lets understand the brownian motion under Q]] — the $\mathbb{Q}$-Brownian motion driving $dS_t$.
- [[19 - The self financing Portfolio]] · [[21 - Martingale Pricing Of European Contingent Claims]] — these dynamics fed into a replicating portfolio.

**Across the programme**
- [[../Fixed Income Securities/22 April 2026]] — the bond/cash-account $B_t = e^{rt}$ as the discounting engine for fixed-income cash flows.
- [[../Derivatives/Swaps/02 Value of an Interest rate swap]] — valuing a stream of cash flows as discounted expectations.
