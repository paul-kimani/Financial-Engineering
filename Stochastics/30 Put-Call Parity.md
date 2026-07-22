# 30 — Put-Call Parity

> A model-free identity: for European options sharing strike $K$, maturity $T$,
> and underlying $S$, $P_t + S_t = C_t + K e^{-r(T-t)}$. It follows purely from
> the **Law of One Price** — a protective put and a fiduciary call both pay
> $\max(S_T, K)$ at maturity, so they must cost the same today. No distributional
> assumptions required.

---

## 1. Definition

**Put-Call Parity** defines a fundamental static relationship between the price of a European call option and a European put option, provided they both have the **same strike price ($K$)**, **same expiration date ($T$)**, and the **same underlying asset ($S$)**.

---

## 2. The Formula

$$P_t + S_t = C_t + K e^{-r(T-t)}$$

### Variable Breakdown:

- **$P_t$**: Price of the European put option at time $t$

- **$S_t$**: Current price of the underlying share at time $t$

- **$C_t$**: Price of the European call option at time $t$

- **$K$**: Exercise (strike) price of the option

- **$r$**: Continuously compounded risk-free interest rate

- **$T - t$**: Time remaining until option maturity

---

## 3. Financial Intuition (No-Arbitrage Proof)

The formula is built on the **Law of One Price** (see [[31.1 Law of One Price]]): if two portfolios yield the exact same payout at maturity $T$, they must cost the same today at time $t$.

Construct two portfolios today at time $t$:

- **Portfolio A:** Buy $1$ Put option ($P_t$) $+$ Buy $1$ Share of stock ($S_t$)

- **Portfolio B:** Buy $1$ Call option ($C_t$) $+$ Deposit $K e^{-r(T-t)}$ cash into a risk-free bank account

### What happens at maturity $T$?

At expiry $T$, compare the terminal values for both scenarios:

|**Scenario at Expiry**|**Value of Portfolio A ($P_T + S_T$)**|**Value of Portfolio B ($C_T + K$)**|
|---|---|---|
|**Stock ends above strike ($S_T \ge K$)**|Put expires worthless ($0$).<br><br>You hold stock = **$S_T$**.|Call is exercised ($\max(S_T - K, 0) = S_T - K$).<br><br>Cash grows to $K$.<br><br>Total = $(S_T - K) + K =$ **$S_T$**.|
|**Stock ends below strike ($S_T < K$)**|Put is exercised ($\max(K - S_T, 0) = K - S_T$).<br><br>You hold stock = $S_T$.<br><br>Total = $(K - S_T) + S_T =$ **$K$**.|Call expires worthless ($0$).<br><br>Cash grows to $K$.<br><br>Total = **$K$**.|

### Conclusion

In every scenario at maturity $T$:

$$\text{Value of Portfolio A} = \max(S_T, K) = \text{Value of Portfolio B}$$

Because both portfolios guarantee the exact same payoff at time $T$, their costs at time $t$ **must be equal** to prevent riskless arbitrage opportunities:

$$P_t + S_t = C_t + K e^{-r(T-t)}$$

---

**See also:** [[31 Proof of Put-Call Parity]] · [[31.1 Law of One Price]] · [[29 Derivation of The Black Scholes merton formula.]]
