# 18 — Previsible Process and Martingale Representation Theorem

> The machinery that turns "a measure exists" into "a hedge exists". A
> **previsible** (predictable) process $\phi_t$ is one you can commit to using
> only past information — no look-ahead. The **Martingale Representation
> Theorem** then says any $\mathbb{Q}$-martingale can be written as
> $dE_t = \phi_t\,dD_t$, i.e. as a running integral against the discounted
> stock — which is exactly a self-financing trading strategy.

---

We introduce some of the most elegant and powerful machinery in mathematical finance: **Previsible Processes**, the **Martingale Representation Theorem (MRT)**, and how they combine to justify the creation of **Self-Financing Replicating Portfolios**.

Here is a step-by-step breakdown of the concepts, equations, and financial intuition.

---

## 1. Previsible Processes & The Martingale Representation Theorem (MRT)

This defines how we are mathematically allowed to trade and introduces the theorem that guarantees we can replicate any derivative payoff.

### Previsible (Predictable) Process

We can define a previsible process $\phi_t$ as one that is $\mathcal{F}_{t-1}$-measurable.

- **The Intuition:** In discrete time, $\phi_t$ represents the number of shares of stock you decide to hold in your portfolio _during_ the time step from $t-1$ to $t$.

- **Why is this necessary?** You must decide your portfolio allocation at the _start_ of the trading day ($t-1$) using only the historical data available up to that point ($\mathcal{F}_{t-1}$). You cannot use future information (hindsight) to decide how many shares to hold today.

> **N.B. :** This condition prevents "insider trading" or look-ahead bias in our models. It ensures that our mathematical trading strategies are physically and practically implementable in the real world.

### The Martingale Representation Theorem (MRT)

The MRT is a deep result in stochastic analysis. We can say:

If $X_t$ and $Y_t$ are both martingales under the same probability measure (such as the risk-neutral measure $\mathbb{Q}$), then there exists a unique previsible process $\phi_t$ such that:

$$dY_t = \phi_t dX_t$$

- **What this means physically:** It tells us that any random martingale process $Y_t$ can be perfectly written as the running integral of another martingale $X_t$, scaled by some dynamic trading strategy $\phi_t$.

### Financial Application of the MRT

In quantitative finance:

- Let $D_t = S_t e^{-rt}$ be the **discounted stock price** (which we proved is a martingale under $\mathbb{Q}$).

- Let $E_t = V_t e^{-rt}$ be the **discounted value of our trading portfolio** (which must also be a martingale under $\mathbb{Q}$ if we want to avoid arbitrage).

According to the MRT, because both $D_t$ and $E_t$ are $\mathbb{Q}$-martingales, there _must_ exist a unique previsible trading strategy $\phi_t$ (representing the number of shares held in the stock) such that:

$$dE_t = \phi_t dD_t$$

---

## 2. Self-Financing Portfolios

This page connects the abstract mathematics of the MRT to the concrete reality of pricing options.

### The Role of MRT in Derivative Valuation

> _"The role of MRT in derivative valuation is that it allows us to construct a self-financing portfolio."_

If you buy an option, the option's value changes randomly every second. To price and hedge this option, an investment bank constructs a **replicating portfolio** containing $\phi_t$ shares of the underlying stock and some cash in a money market account.

A portfolio is **self-financing** if no money is added to or withdrawn from it after the initial setup at time $0$. Any change in the value of the portfolio is driven _solely_ by the capital gains or losses of the underlying assets.

### How Girsanov + MRT work together to price an option:

1. **Girsanov's Theorem** guarantees we can find a risk-neutral measure $\mathbb{Q}$ where discounted stock prices are martingales.

2. The **Martingale Representation Theorem (MRT)** guarantees that because everything is a martingale under $\mathbb{Q}$, there exists a mathematically valid, previsible trading strategy ($\phi_t$) that can dynamically adjust our stock and cash holdings to perfectly match the payoff of any European option at maturity.

3. Since the portfolio is **self-financing** and matches the option exactly at maturity, the option's fair price today _must_ equal the cost of setting up this replicating portfolio.

---

## 3. Market Completeness — the Deeper Payoff

The MRT is really a statement about **market completeness**. In the single-stock Black–Scholes model there is exactly *one* source of randomness ($W_t^\mathbb{Q}$), and the MRT guarantees that a single traded asset is enough to represent — and therefore hedge — any $\mathcal{F}_T$-measurable payoff. This is what makes the risk-neutral measure **unique** and the price **unambiguous**.

When randomness outnumbers traded assets (e.g. stochastic volatility, or more risk factors than instruments), the market is **incomplete**: perfect replication fails, the equivalent martingale measure is no longer unique, and prices are only bounded rather than pinned down. That gap is exactly where the clean theory of these chapters stops and model risk begins — the practical lesson of the [[../Derivatives/Lehman Brothers 2007]] case study.

---

## Connections

**Within Stochastics**
- [[09 - Martingales]] — both $D_t$ and $E_t$ must be $\mathbb{Q}$-martingales for the MRT to apply.
- [[02 - The Itô Integral]] — the representation $dY_t = \phi_t\,dX_t$ is an Itô integral against $X_t$.
- [[16 - Equivalent Probability Measures and Girsanov's Theorem]] — supplies the measure $\mathbb{Q}$; MRT then supplies the strategy.
- [[19 - The self financing Portfolio]] · [[20 - Proof of Replicating Portfolio]] — the strategy $\phi_t$ realised as a self-financing portfolio.

**Across the programme**
- [[../Derivatives/Lehman Brothers 2007]] — what happens when the neat replication story meets incomplete, illiquid markets.
- [[../Financial Theory/13 - Efficient Market Hypothesis]] — no-arbitrage and information as the economic backdrop to completeness.
