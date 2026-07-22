# 09 — Expected Utility Theory

> Part II of the folder swaps certain payoffs for uncertain ones.
> Comparing investments by expected monetary value alone fails — the
> St. Petersburg paradox shows that a gamble with *infinite* expected
> value still finds no taker. Bernoulli's resolution: investors choose
> on **expected utility**, not expected money.

---

## 1. Why expected value alone fails

A first guess for choosing between uncertain investments is to pick
the one with the highest **expected value**. For random payoff $X$
taking outcomes $X_1, X_2, \dots$ with probabilities $p_1, p_2, \dots$:

$$
\mathbb{E}[X] \;=\; p_1 X_1 + p_2 X_2 + \cdots.
$$

If the expected-value criterion held universally, an individual would
be willing to **pay** an amount equal to the expected value of an
uncertain prize. That rule turns out to be wrong in practice — most
people refuse fair bets at any meaningful scale, and refuse some
bets even when the expected value is infinite (next section).

---

## 2. Fair games

A **fair game** (or fair bet / fair lottery) is a random game whose
expected payoff is **zero**:

$$
\mathbb{E}[X] \;=\; 0.
$$

**Example — coin flip for KES 1.** Heads pays $+1$, tails pays
$-1$, each with probability $\tfrac{1}{2}$:

$$
\mathbb{E}[X] \;=\; 0.5\cdot(+1) + 0.5\cdot(-1) \;=\; 0.
$$

A game with $\mathbb{E}[X] > 0$ is unfair *in the player's favour*; it
becomes "fair" again only after an entry fee equal to $\mathbb{E}[X]$.

Most people happily flip a fair coin for a few dollars but refuse the
same coin flip when the stake becomes $\pm \text{£}1$ million. The
expected-value criterion cannot explain that change of heart.

---

## 3. The St. Petersburg paradox

Bernoulli's diagnostic example. The setup:

- A fair coin is tossed until the **first head** appears, ending the
  game.
- If the first head appears on toss $s$, the casino pays $2^{s}$
  shillings.

How much would you pay to enter? Under the expected-value criterion
the entry fee should equal the expected payoff:

$$
\mathbb{E}[X] \;=\; \sum_{s=1}^{\infty} \tfrac{1}{2^{s}} \cdot 2^{s}
\;=\; \sum_{s=1}^{\infty} 1 \;=\; +\infty.
$$

The expected payoff is **infinite** — yet nobody is willing to pay
even, say, £1 billion to play. The paradox: a game with infinite
expected value finds no takers.

**Conclusion.** In situations involving uncertainty and risk,
individuals do **not** choose based on expected value of outcomes.

---

## 4. Bernoulli's resolution — expected utility

Bernoulli's insight: investors do not respond to monetary prizes
directly; they respond to the **utility** these prizes provide.
Twice the money is not twice as good — the *marginal* utility of
wealth **declines** as wealth increases (diminishing marginal utility,
chapter [[04 - Indifference Curves, MRS and Utility]]).

With a concave utility function $U(\cdot)$ the St. Petersburg sum
becomes

$$
\mathbb{E}[U(X)] \;=\; \sum_{s=1}^{\infty} \tfrac{1}{2^{s}} \cdot U(2^{s}),
$$

which **converges** (try $U(x) = \ln x$: each term is $s \ln 2 / 2^{s}$
and the sum is finite). Bernoulli called this finite quantity the
**moral value** of the game.

The paradox dissolves: a game's *moral* value can be small even when
its *monetary* expected value is infinite.

---

## 5. The expected utility hypothesis

> **Expected Utility Hypothesis.** Under uncertainty individuals act
> *as if* they choose to maximise the **expected utility** of
> outcomes, not the expected monetary value.

For two uncertain payoffs $X = (X_i, p_i)$ and $Y = (Y_i, q_i)$, the
investor prefers $X$ to $Y$ when

$$
\mathbb{E}[U(X)] \;>\; \mathbb{E}[U(Y)].
$$

The choice depends on the **shape** of the utility function $U(\cdot)$.

| Quantity                 | Formula                                            |
| :----------------------- | :------------------------------------------------- |
| **Expected payoff**      | $\mathbb{E}[X] = p_1 X_1 + p_2 X_2 + \cdots$       |
| **Expected utility**     | $\mathbb{E}[U(X)] = p_1 U(X_1) + p_2 U(X_2) + \cdots$ |

The next chapter sets out the six axioms that pin down $U(\cdot)$ —
the Von Neumann–Morgenstern construction.

---

## 6. Risk preferences

The *shape* of $U(\cdot)$ determines the investor's risk attitude.

- **Risk Neutral.** Cares only about expected payoff. $U$ is **linear**.
  Standard modelling assumption for firms (which are presumed to have
  large risk-bearing capacity).
- **Risk Averse.** Dislikes risk. $U$ is **strictly concave**
  ($U'' < 0$). Standard assumption for individual investors in
  financial theory.
- **Risk Loving.** Prefers more risk. $U$ is **strictly convex**
  ($U'' > 0$). The gambler's utility.

The mapping risk preference $\leftrightarrow$ curvature is made
quantitative in chapter
[[11 - Risk Aversion - Certainty Equivalent and Risk Premium]] and
sharpened by the Pratt–Arrow measures of chapter
[[12 - Pratt-Arrow Risk Aversion]].

### Illustration — log vs squared utility

Faced with a gamble offering either umbrellas or ice-cream, an investor
with $U(I) = \sqrt{I}$ (concave, risk-averse) prefers the safer
umbrella; an investor with $U(I) = I^2$ (convex, risk-loving) prefers
the riskier ice-cream. Same expected payoffs, different choices —
driven entirely by the utility function.

---

## 7. From single outcomes to portfolios

Under uncertainty the objects of choice are no longer consumption
bundles but **vectors of state-contingent (future) money payoffs**.
Each such vector is a **financial asset** that the investor can buy.

Individuals demand securities to **redistribute income across time and
states of nature** — the consumption-smoothing role of Part I extended
to uncertain states. The framework asks:

> How do consumers rank/order alternative uncertain investments
> according to their preferences?

Answer: by expected utility, using a $U(\cdot)$ pinned down by the
VNM axioms in chapter [[10 - VNM Axioms and Utility Function]].

---

## Cross‑references

- [[10 - VNM Axioms and Utility Function]] — the six axioms that
  underwrite the expected-utility function.
- [[11 - Risk Aversion - Certainty Equivalent and Risk Premium]] —
  pricing risk via utility curvature.
- [[12 - Pratt-Arrow Risk Aversion]] — the local measures of risk
  aversion (ARA, RRA).
- [[04 - Indifference Curves, MRS and Utility]] — the certainty
  counterpart of $U(\cdot)$ and the origin of diminishing marginal
  utility.
- [[01 - Models in Financial Economics]] — economic models in finance
  are built on top of expected utility.
- [[../QUANTFRAME/Concepts/BackGround to Kelly Criterion]] — log
  utility resolves the St. Petersburg paradox and underlies the Kelly
  growth optimum.
- [[../Stochastics/10 - Geometric Brownian Motion]] — the GBM
  log-return distribution interacts naturally with log utility.
- [[09.1 - Expected Utility theory Question]] — a worked expected-utility decision under two utility functions.

