# 04 — Indifference Curves, MRS and Utility

> The *want-to* side of the consumption/savings decision. An
> **indifference curve** is the set of $(C_0, C_1)$ bundles delivering
> the same total utility. Its slope is the **Marginal Rate of
> Substitution (MRS)** — the investor's subjective time-preference rate.
> Diminishing marginal utility forces the curves to be convex.

---

## 1. Indifference curves

An **indifference curve** plots all bundles of current and future
consumption $(C_0, C_1)$ that give the investor the **same total
utility**. The investor is, by construction, *indifferent* between any
two points on the same curve.

The full **indifference map** is a family of nested curves
$\{I_1, I_2, I_3, \dots\}$. By the Non-Satiation axiom of
[[03 - Axioms of Choice under Certainty]], curves further up-and-right
represent higher utility:

$$
I_3 \succ I_2 \succ I_1.
$$

By Transitivity, no two curves cross — so the family is a clean
foliation of the $(C_0, C_1)$ plane.

---

## 2. The Marginal Rate of Substitution (MRS)

Pick a point $B$ on an indifference curve. The slope of the curve at
$B$ — equivalently, the slope of the straight tangent line to the
curve at $B$ — is the **Marginal Rate of Substitution** between
current and future consumption:

$$
\text{MRS} \;=\; -\,\frac{dC_1}{dC_0}\bigg|_{U=\text{const}}.
$$

**Plain reading.** MRS is the investor's *personal exchange rate* for
time. It answers:

> "How many extra units of tomorrow-money must I receive to willingly
> give up one unit of today-money, *and still feel equally happy*?"

It is the **subjective rate of time preference**, often denoted
$r_1$ in lecture notes. The investor will only trade away current
consumption for a return that at least matches their personal MRS.

> **Note — MRS vs MRT.** Don't conflate the MRS (this chapter) with
> the **MRT** of chapter [[05 - Opportunity Set and MRT]]. MRT is the
> *objective* market or technology trade-off. MRS is the *subjective*
> personal trade-off. The optimal consumption decision is found where
> the two coincide: $\text{MRS}^\ast = \text{MRT}^\ast$.

---

## 3. Why the MRS changes along the curve

The Convexity axiom forces indifference curves to be **convex toward
the origin**. Geometrically that means the curve is *steep* when $C_0$
is small and *flat* when $C_0$ is large.

- **Small $C_0$ (left of the curve).** You have very little today. To
  give up even a small amount of today-money, you demand a *lot* of
  tomorrow-money. MRS is large — your rate of time preference is high.
- **Large $C_0$ (right of the curve).** You have plenty today. You
  will give up some of today-money for relatively modest amounts
  tomorrow. MRS is small — your rate of time preference is low.

In short: **MRS declines as $C_0$ increases** along the curve.

---

## 4. Diminishing marginal utility

The Convexity-driven shape of indifference curves is the geometric
twin of a deeper economic principle:

> **Diminishing marginal utility.** Other things equal, the more one
> has of an asset, the *less* each successive unit adds to total
> utility.

In symbols, the utility function $U(\cdot)$ is strictly increasing but
concave:

$$
U'(C) > 0, \qquad U''(C) < 0.
$$

A typical example is **logarithmic utility** $U(C) = \ln C$, which
satisfies $U'(C) = 1/C > 0$ and $U''(C) = -1/C^2 < 0$. Log utility is
also the utility implicit in the
[[../QUANTFRAME/Concepts/BackGround to Kelly Criterion|Kelly Criterion]]
growth optimisation.

### Tracing diminishing marginal utility along an indifference curve

Move along a single indifference curve from a bundle $A_1$ (low $C_0$,
high $C_1$) to $A_3$ (high $C_0$, low $C_1$). At each step you give up
the *same* amount of $C_1$ but require an *increasingly large* amount
of $C_0$ to remain on the curve. Each additional unit of $C_0$ adds
less utility (diminishing marginal utility of $C_0$), so more of it is
needed to compensate for the lost $C_1$.

---

## 5. From indifference curves to the choice problem

Indifference curves are exactly half the apparatus. The other half is
the **opportunity set** (chapter [[05 - Opportunity Set and MRT]]),
which says what the market or technology actually permits. Solving the
consumption/savings decision means picking the bundle $(C_0^\ast, C_1^\ast)$
where:

- $(C_0^\ast, C_1^\ast)$ lies on the opportunity set (feasibility), and
- the indifference curve through $(C_0^\ast, C_1^\ast)$ is the **highest
  attainable** subject to that constraint.

Graphically that point is the unique tangency. Algebraically it is
the condition

$$
\boxed{\;\text{MRS}^\ast \;=\; \text{MRT}^\ast.\;}
$$

The next two chapters develop the MRT half of this equation, then
chapter [[06 - Real Investment Decision]] solves the first tangency
explicitly.

---

## Cross‑references

- [[03 - Axioms of Choice under Certainty]] — the four axioms that
  pin down the indifference-curve shape.
- [[02 - Consumption and Savings Decision]] — the choice problem these
  curves help solve.
- [[05 - Opportunity Set and MRT]] — the objective trade-off (MRT)
  that pairs with the subjective MRS here.
- [[06 - Real Investment Decision]] — the first tangency
  $\text{MRS}^\ast = \text{MRT}^\ast$ solved.
- [[10 - VNM Axioms and Utility Function]] — the utility-function
  analogue used under uncertainty.
- [[../QUANTFRAME/Concepts/BackGround to Kelly Criterion]] — log
  utility in a repeated-betting setting.
