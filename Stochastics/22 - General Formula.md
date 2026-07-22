# 22 — General Formula

> The risk-neutral pricing formula, distilled to a **three-step recipe**: write
> the payoff at $T$, take its $\mathbb{Q}$-expectation, and discount by
> $e^{-r(T-t)}$. This chapter closes the algebra of
> $V_t = E^\mathbb{Q}[V_T e^{-r(T-t)} \mid \mathcal{F}_t]$ and then plugs in the
> standard payoffs — call, put, forward, and the general claim.

---

## 1. Finishing the Pricing Formula and Its 3-Step Recipe

This page wraps up the algebraic simplification of the pricing formula and gives you an incredibly practical, high-level recipe for pricing any contingent claim.

### The Math Check

At the top, we equate the option price $V_t$ with our replicating portfolio $\Pi_t$:

$$V_t e^{-rt} = E^\mathbb{Q}\left[ V_T e^{-rT} \mid \mathcal{F}_t \right]$$

To isolate the current price $V_t$, we multiply both sides of the equation by $e^{rt}$:

$$V_t = e^{rt} E^\mathbb{Q}\left[ V_T e^{-rT} \mid \mathcal{F}_t \right]$$

Since $e^{rt}$ is deterministic (not random), we can pull it inside the expectation:

$$V_t = E^\mathbb{Q}\left[ V_T e^{-rT} \cdot e^{rt} \mid \mathcal{F}_t \right]$$

By combining the exponents (since $-rT + rt = -r(T-t)$), we arrive at the clean final formula:

$$\mathbf{V_t = E^\mathbb{Q}\left[ V_T e^{-r(T-t)} \mid \mathcal{F}_t \right]}$$

### The 3-Step Recipe

If you ever get lost in the stochastic calculus, just remember these three intuitive steps:

1. **Find the Payoff:** Write down what the option pays out at maturity $T$ (e.g., $(S_T - K)^+$).

2. **Find the Expected Payoff:** Calculate the average value of that payoff, but _strictly_ using the risk-neutral probabilities ($\mathbb{Q}$-measure).

3. **Discount it:** Bring that expected future money back to today (time $t$) by multiplying it by the discount factor $e^{-r(T-t)}$.

---

## 2. Application to Common Derivatives

Now let's look at how we plug specific contract payouts into our brand-new formula.

### 1. Call Option

A European call option gives you the right to buy a stock at a set strike price $K$.

- **The Payoff at $T$:** $\max(S_T - K, 0)$ (you only exercise it if the stock price $S_T$ is higher than the strike $K$).

- **The Pricing Formula:**

$$V_t = E^\mathbb{Q}\left[ \max(S_T - K, 0) \mid \mathcal{F}_t \right] e^{-r(T-t)}$$

    _(Solving this expectation analytically is exactly how we get the famous Black-Scholes call pricing formula — see [[29 - Derivation of The Black Scholes merton formula]]!)_

### 2. Put Option

A European put option gives you the right to sell a stock at a strike price $K$.

- **The Payoff at $T$:** $\max(K - S_T, 0)$ (you only exercise it if the stock price $S_T$ is lower than the strike $K$, letting you sell it for more than it's worth).

- **The Pricing Formula:**

$$V_t = E^\mathbb{Q}\left[ \max(K - S_T, 0) \mid \mathcal{F}_t \right] e^{-r(T-t)}$$

### 3. Forward Contract

A forward contract is an obligation to buy the stock at maturity $T$ for a fixed price $K$. Unlike an option, you _must_ go through with it even if you lose money.

- **The Payoff at $T$:** $S_T - K$ (this can be positive or negative).

- **The Pricing Formula:**

$$V_t = E^\mathbb{Q}\left[ S_T - K \mid \mathcal{F}_t \right] e^{-r(T-t)}$$

### 4. Any General Derivative

For any random derivative contract with an arbitrary payoff structure:

- **The Pricing Formula:**

$$V_t = E^\mathbb{Q}\left[ \text{Payoff} \mid \mathcal{F}_t \right] e^{-r(T-t)}$$

---

## 3. The Forward Price Falls Straight Out

The forward payoff $S_T - K$ is a useful sanity check on the machinery. Applying the recipe and using that the *discounted stock is a $\mathbb{Q}$-martingale* (so $E^\mathbb{Q}[S_T \mid \mathcal{F}_t] = S_t e^{r(T-t)}$):

$$V_t = e^{-r(T-t)}\left( S_t e^{r(T-t)} - K \right) = S_t - K e^{-r(T-t)}$$

Setting this to zero (a forward costs nothing to enter) gives the fair **forward price** $F = S_t e^{r(T-t)}$ — the cost-of-carry relation. Subtracting the put payoff from the call payoff, $\max(S_T-K,0) - \max(K-S_T,0) = S_T - K$, then reproduces **[[30 - Put-Call Parity|put–call parity]]** directly from linearity of the expectation. So parity is not a separate axiom; it is baked into this one formula.

---

## Connections

**Within Stochastics**
- [[21 - Martingale Pricing Of European Contingent Claims]] — the derivation of the formula this note applies.
- [[29 - Derivation of The Black Scholes merton formula]] — evaluating the call expectation analytically under GBM.
- [[30 - Put-Call Parity]] · [[31 - Proof of Put-Call Parity]] — the parity relation recovered above.

**Across the programme**
- [[../Derivatives/7 May 2026 - Personal Notes 1]] — calls, puts and forwards as traded instruments.
- [[../Derivatives/Swaps/02 Value of an Interest rate swap]] — a swap valued as a portfolio of forward-rate agreements, each priced by this recipe.
