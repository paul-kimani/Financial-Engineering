# 08 — Fisher's Separation Theorem

> When a frictionless capital market exists, the investment decision
> (where to sit on the real-investment frontier) and the consumption
> decision (where to sit on the resulting trading line) **decouple**.
> The investor first maximises wealth by investing until the marginal
> rate of return equals the market rate, then adjusts consumption to
> taste by borrowing or lending. Transaction costs break the theorem.

---

## 1. The two-step decision

Combining the concave real-investment frontier $F$ of
[[06 - Real Investment Decision]] with the capital-market trading
line of [[07 - Choice with Capital Markets]] gives a remarkable
decomposition. Under a frictionless capital market the rational
consumer's problem splits into:

1. **Investment decision.** Choose the real-investment level so that
   the marginal rate of return on investment equals the objective
   market rate of interest:

   $$
   \boxed{\;\text{MRT}^\ast \;=\; 1 + i^\ast \;=\; 1 + r.\;}
   $$

   This pins down a unique investment point $x^\ast = (x_0^\ast,
   x_1^\ast)$ on the frontier, *independent of the investor's
   preferences*.

2. **Consumption decision.** Slide along the trading line through
   $x^\ast$ — borrowing or lending at rate $r$ — until the
   subjective time-preference rate matches the market rate:

   $$
   \text{MRS}^\ast \;=\; 1 + r.
   $$

   This gives the optimal consumption bundle $C^\ast = (C_0^\ast,
   C_1^\ast)$, which *does* depend on preferences.

---

## 2. The separation theorem

> **Fisher's Separation Theorem.** When a frictionless capital market
> exists with a common rate $r$ for borrowing and lending, every
> investor — regardless of their personal preferences for current vs
> future consumption — chooses the **same** real-investment level
> $x^\ast$, namely the one satisfying $\text{MRT}^\ast = 1 + r$.

The theorem has a powerful corporate-finance consequence:

> **The firm should maximise NPV.** Investment choices made by a firm
> on behalf of its owners are *independent* of the owners' individual
> consumption preferences. The firm needs only to maximise the present
> wealth of its owners — i.e. maximise **net present value** at the
> objective market rate $r$.

In particular, owners with very different time preferences need not
fight about the firm's investment plan: every owner agrees on the
NPV-maximising point $B$ on $F$. They then go to the capital market to
implement their personal consumption pattern.

---

## 3. NPV maximisation — the geometric statement

The trading line through any real-investment point $x'$ has the
equation

$$
C_1 \;=\; (1 + r)\,x_0' \;+\; x_1' \;-\; (1 + r)\,C_0.
$$

Its $C_0$-intercept (where $C_1 = 0$) is

$$
W_0^\ast \;=\; x_0' \;+\; \frac{x_1'}{1 + r}
\;=\; \text{present value of the investment bundle.}
$$

Maximising $W_0^\ast$ over admissible $x'$ on $F$ means **pushing the
trading line as far up-and-right as possible**. The optimum is the
tangency $x^\ast$ where the trading line of slope $-(1+r)$ just
touches $F$ — exactly the condition $\text{MRT}^\ast = 1 + r$.

This is the geometric content of "maximise NPV". Different consumers
then choose different points along that single tangent line according
to their indifference curves.

---

## 4. Benefits of capital markets

Capital markets generally **raise the budget constraint**.

- **With** capital markets: investors lend or borrow along the trading
  line, reach a higher indifference curve, and **separate** their
  earning and consumption patterns.
- **Without** capital markets: savings *equal* investment ($C_0' = x_0'$
  and $C_1' = x_1'$). The consumer is stuck on the frontier $F$ at a
  strictly lower indifference curve.

To a considerable extent, capital markets let individuals **separate
income-earning decisions from consumption-pattern decisions**. People
can make career and earning choices on other grounds and rely on the
capital market to smooth their living standard over time.

---

## 5. Where the theorem breaks — transaction costs

If borrowing and lending rates differ — typically because of
**transaction costs** charged by financial intermediaries — Fisher's
theorem **fails**.

Let $r_B$ and $r_L$ denote the borrowing and lending rates with
$r_B > r_L$. The trading line through $x^\ast$ is now a *kinked*
piecewise line:

- slope $-(1 + r_B)$ for $C_0 > x_0^\ast$ (borrowing side, steeper),
- slope $-(1 + r_L)$ for $C_0 < x_0^\ast$ (lending side, flatter).

### Two consumer types

- A **lender** $L$ — who prefers more future consumption — sees the
  flatter $-(1 + r_L)$ slope as their effective trade-off. The
  tangency condition $\text{MRT} = 1 + r_L$ pins their optimal
  investment at $C_L$.
- A **borrower** $B$ — who prefers more current consumption — sees
  the steeper $-(1 + r_B)$ slope. Tangency requires $\text{MRT} =
  1 + r_B$, which is at a different point $C_B$ on $F$.

Different consumers therefore choose **different** real-investment
levels. The firm's manager can no longer pick a single
preference-independent NPV-maximising point: the investment decision
depends on whether the marginal owner is a borrower or a lender.

Hence under transaction costs the investment decision is **contingent
on consumer preferences** — the separation theorem breaks down.

---

## 6. Recap

| Setting                              | Investment decision                                   | Consumption decision           | Separation? |
| :----------------------------------- | :---------------------------------------------------- | :----------------------------- | :---------- |
| Real investment only                 | Tangency $\text{MRS}^\ast = \text{MRT}^\ast$ on $F$.  | Same point — $C^\ast = x^\ast$. | No          |
| Real investment + frictionless market | $\text{MRT}^\ast = 1 + r$, preference-independent.    | Along trading line; $\text{MRS}^\ast = 1+r$. | **Yes**     |
| Two rates $r_B \neq r_L$             | Depends on whether consumer borrows or lends.         | Depends on personal MRS.       | No          |

---

## Cross‑references

- [[07 - Choice with Capital Markets]] — the trading line whose
  tangency with $F$ produces the preference-independent investment
  point.
- [[06 - Real Investment Decision]] — the no-capital-market case where
  separation does not happen.
- [[02 - Consumption and Savings Decision]] — the original problem
  this theorem decomposes.
- [[../Fixed Income Securities/22 April 2026]] — NPV / discounting at
  rate $r$ is the same machinery as maximising $W_0^\ast$ here.
- [[../Fixed Income Securities/15 April online Excercise]] — the repo
  haircut illustrates a transaction-cost wedge in a different form.
- [[09 - Expected Utility Theory]] — under uncertainty, the elegance
  of separation is partially preserved (e.g. two-fund separation in
  mean-variance) and partially lost.
- [[08.1 - Numerical Example on Maximising Wealth]] — a fully worked numerical application of this theorem.

