# 19 — The Self-Financing Portfolio

> A **self-financing portfolio** is a closed system: after the initial capital
> goes in at $t=0$, you may rebalance freely but never add or withdraw cash. The
> defining condition drops the rebalancing terms, leaving
> $dV_t = \phi_t\,dS_t + \psi_t\,dB_t$ — the wealth moves only with asset prices.
> Under $\mathbb{P}$ this expands to a drift set by your allocation and a
> volatility driven entirely by your stock exposure.

---

This page provides the formal mathematical framework for a **Self-Financing Portfolio**, focusing on its value definition, its differential dynamics, and how it behaves under the real-world probability measure $\mathbb{P}$.

Here is the section-by-section explanation.

---

## 1. Conceptual & Static Definition

This page explains what a self-financing portfolio means economically and sets up its baseline algebraic equation.

### The Economic Intuition

A **self-financing portfolio** is a closed financial system. Once you inject your initial capital at time $t=0$, you lock the vault.

- You are completely allowed to rebalance your holdings—for instance, selling some bonds to buy more stock.

- However, any transaction must be funded entirely from within the portfolio. You **cannot inject fresh cash** from the outside (no external additions), and you **cannot withdraw funds** to buy a coffee (no external withdrawals).

### The Portfolio Value Equation

Mathematically, at any given moment $t$, your portfolio's total wealth $V_t$ is the sum of your asset holdings:

$$V_t = \phi_t S_t + \psi_t B_t$$

Where:

- $S_t$: The price of one share of stock (Risky Asset).

- $\phi_t$: The number of shares of stock you hold.

- $B_t$: The price of one unit of the cash account/bond (Risk-free Asset).

- $\psi_t$: The number of bond units you hold.

---

## 2. Differential Dynamics & The Real-World ($\mathbb{P}$) Expansion

This page addresses what happens when time moves forward by an infinitesimal step $dt$.

### The Self-Financing Condition (The Key Equation)

When you differentiate the value equation, a normal calculus application (the product rule) would yield four terms: $dV_t = (\phi_t dS_t + \psi_t dB_t) + (S_t d\phi_t + B_t d\psi_t)$.

However, for a portfolio to be **self-financing**, it must satisfy:

$$dV_t = \phi_t dS_t + \psi_t dB_t$$

- **Why?** The missing part, $(S_t d\phi_t + B_t d\psi_t)$, represents the net cost of buying and selling shares during rebalancing. For a self-financing portfolio, that rebalancing cost must equal exactly **zero**.

- This means the variation in your total wealth ($dV_t$) is driven **solely by the market price movements of the assets themselves** ($dS_t$ and $dB_t$), not by you changing your position size.

### Substituting Real-World Dynamics ($\mathbb{P}$)

Your notes then substitute how the stock and bond actually evolve in the real world under measure $\mathbb{P}$:

- **Stock Price Dynamics:** $dS_t = \mu S_t dt + \sigma S_t dW_t$ _(Note: There is a small typo in your notes' list where it says $\sigma S_t dt$ instead of $\sigma S_t dW_t$, but it is corrected right below in the actual derivation)._

- **Bond Dynamics:** $dB_t = r B_t dt$

Plugging these two SDEs into the self-financing condition:

$$dV_t = \phi_t \left( \mu S_t dt + \sigma S_t dW_t \right) + \psi_t \left( r B_t dt \right)$$

Grouping the deterministic time terms ($dt$) together and isolating the random Brownian motion term ($dW_t$) yields the final equation at the bottom of the page:

$$dV_t = \left( \phi_t \mu S_t + \psi_t r B_t \right) dt + \phi_t \sigma S_t dW_t$$

### What This SDE Tells Us:

1. **The Drift:** The expected growth rate of your portfolio wealth depends on your asset allocations. You get a return of $\mu$ on your stock allocation ($\phi_t S_t$) and a return of $r$ on your bond allocation ($\psi_t B_t$).

2. **The Risk:** The volatility of your entire portfolio is driven completely by your stock exposure ($\phi_t \sigma S_t dW_t$). The bond adds absolutely zero randomness because it has no $dW_t$ component.

The prompt concludes with a note to check the assumptions of the **Black-Scholes-Merton (BSM)** and **Binomial Pricing Models**, which explicitly rely on this continuous-time self-financing replication to eliminate risk completely!

---

## 3. The Discrete-Time Intuition

The continuous condition $dV_t = \phi_t\,dS_t + \psi_t\,dB_t$ is the limit of a discrete book-keeping identity. Over a step $[t, t+\Delta t]$, "self-financing" means the cost of the *new* position equals the value of the *old* one:

$$\phi_{t+\Delta t} S_t + \psi_{t+\Delta t} B_t = \phi_t S_t + \psi_t B_t$$

i.e. any extra shares bought are paid for *entirely* by selling bonds (or vice-versa) — no cash is injected or removed. Rearranging and taking $\Delta t \to 0$ kills the $(S_t\,d\phi_t + B_t\,d\psi_t)$ term, which is why only the market-move terms survive. This is the exact continuous-time analogue of the rebalancing rule in the discrete [[27 Stochastic Models of Derivative Prices|binomial model]].

---

## Connections

**Within Stochastics**
- [[05 - Diffusion Processes Catalogue]] — the GBM/bond SDEs substituted into $dV_t$.
- [[18 Previsible Process and Martingale Representation Theorem]] — supplies the previsible strategy $\phi_t$ that this portfolio implements.
- [[20 Proof of Replicating Portfolio]] — proves self-financing $\iff$ discounted value is a $\mathbb{Q}$-martingale.
- [[28 Derivation of the Black Scholes PDE .]] — the delta-hedged portfolio is a self-financing portfolio in disguise.

**Across the programme**
- [[../Financial Theory/06 - Real Investment Decision]] — the budget-constraint logic (no external funds) mirrored in real investment.
- [[../Derivatives/Swaps/03 Interest Rate Swap Terminology & Risk Profiles]] — dynamic hedging of a rate book as self-financing rebalancing.
