# 31 — Proof of Put-Call Parity

> The formal replication argument. Build **Portfolio A** (long call + a
> zero-coupon bond paying $K$) and **Portfolio B** (long put + one share). Case
> analysis on $S_T \lessgtr K$ shows both portfolios pay $\max(S_T, K)$ in every
> state; by the Law of One Price their time-$t$ costs must match, giving
> $P_t + S_t = C_t + K e^{-r(T-t)}$.

---

Consider two distinct portfolios constructed at time $t$:

### Portfolio A: Long Call + Risk-free Bond

- **Long European Call Option:** Strike price $K$, maturity $T$, priced at $C_t$.

- **Risk-free Zero-Coupon Bond:** Pays $K$ guaranteed at time $T$. Its present value at time $t$ is $K e^{-r(T-t)}$.

- **Initial Cost at time $t$:** $C_t + K e^{-r(T-t)}$

### Portfolio B: Long Put + Underlying Asset

- **Long European Put Option:** Strike price $K$, maturity $T$, priced at $P_t$.

- **Underlying Stock:** One share currently priced at $S_t$.

- **Initial Cost at time $t$:** $P_t + S_t$

---

## Payoff Analysis at Maturity ($t = T$)

At maturity $T$, we evaluate the payoffs under two possible market conditions:

### Case 1: Stock Price finishes below Strike ($S_T < K$)

- **Portfolio A:**

    - The Call option expires worthless ($\max(S_T - K, 0) = 0$).

    - The zero-coupon bond pays out $K$.

    - **Total Payoff:** $0 + K = \mathbf{K}$

- **Portfolio B:**

    - The Put option is exercised ($\max(K - S_T, 0) = K - S_T$).

    - You hold the share of stock worth $S_T$.

    - **Total Payoff:** $(K - S_T) + S_T = \mathbf{K}$

### Case 2: Stock Price finishes above Strike ($S_T \ge K$)

- **Portfolio A:**

    - The Call option is exercised ($\max(S_T - K, 0) = S_T - K$).

    - The zero-coupon bond pays out $K$.

    - **Total Payoff:** $(S_T - K) + K = \mathbf{S_T}$

- **Portfolio B:**

    - The Put option expires worthless ($\max(K - S_T, 0) = 0$).

    - You hold the share of stock worth $S_T$.

    - **Total Payoff:** $0 + S_T = \mathbf{S_T}$

---

## Summary Table

|**Portfolio**|**Cost at time $t$**|**Payoff at $T$ ($S_T < K$)**|**Payoff at $T$ ($S_T \ge K$)**|
|---|---|---|---|
|**A**|$C_t + K e^{-r(T-t)}$|$0 + K = \mathbf{K}$|$(S_T - K) + K = \mathbf{S_T}$|
|**B**|$P_t + S_t$|$(K - S_T) + S_T = \mathbf{K}$|$0 + S_T = \mathbf{S_T}$|

---

## Conclusion

Because **Portfolio A** and **Portfolio B** generate identical payoffs in all possible future states at maturity $T$, the **Law of One Price** states that under no-arbitrage conditions, their initial costs at time $t$ must be identical:

$$P_t + S_t = C_t + K e^{-r(T-t)} \quad \blacksquare$$

---

## The Arbitrage If Parity Breaks

The proof is worth completing with the *enforcement* argument. Suppose $C_t + Ke^{-r(T-t)} < P_t + S_t$ (Portfolio A is cheaper). A trader would **buy A** and **short B** today, pocketing the positive difference. At maturity both portfolios are worth $\max(S_T, K)$, so the short exactly funds the long and the closing cash flow is zero — a *riskless* profit banked upfront. Traders piling into this trade bid A up and push B down until the gap closes. The same argument runs symmetrically if B is cheaper. This is the [[31.1 - Law of One Price|Law of One Price]] doing active work, and it is why parity holds as a near-equality in liquid markets (up to transaction costs and borrow fees).

---

## Connections

**Within Stochastics**
- [[30 - Put-Call Parity]] — the statement and the synthetic-position reading.
- [[31.1 - Law of One Price]] — the no-arbitrage principle enforcing it.
- [[22 - General Formula]] — parity as a consequence of the linear pricing formula.

**Across the programme**
- [[../Fixed Income Securities/04 Repurchase Agreements (REPOs) Rates, Applications, and Risks]] — the repo/borrow mechanics that make the short leg of this arbitrage possible (and set its true cost).
- [[../Derivatives/7 May 2026 - Personal Notes 1]] — the option and forward payoffs used in the two portfolios.
