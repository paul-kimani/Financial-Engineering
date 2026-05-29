# 05 — Opportunity Set and MRT

> The *can-do* side of the consumption/savings decision. The
> **opportunity set** lists every $(C_0, C_1)$ bundle the individual
> can afford. The **budget constraint** is its boundary. Its slope —
> the **Marginal Rate of Transformation (MRT)** — is the objective
> market trade-off between today-money and tomorrow-money. Under a
> flat capital-market rate $r$ the boundary is a straight line of
> slope $-(1+r)$.

---

## 1. The consumption opportunity set

Before we add time, recall the static microeconomic primitives.

- **Opportunity set.** The complete set of consumption bundles that
  the individual can afford, given current income and market prices.
- **Budget constraint.** The boundary of the opportunity set — the
  exact limit of what is affordable if all resources are spent. It
  encodes the real-world **trade-off**: to get more of Good A, you
  must give up some of Good B.
- **Marginal Rate of Transformation (MRT).** The slope of the budget
  constraint. It is the rate at which one good must be sacrificed to
  "transform" purchasing power into another good.

Under certainty MRT is **objective** — it comes from market prices or
technology, not from the investor's preferences. That's the contrast
with **MRS** in chapter [[04 - Indifference Curves, MRS and Utility]].

---

## 2. Adding time — current vs future consumption

Financial economics replaces two physical goods with two time periods.

- **Current consumption $C_0$** (sometimes written $x_0$) — what you
  spend today.
- **Future consumption $C_1$** (sometimes written $x_1$) — what you
  spend tomorrow, funded by what you save today.

If you save and earn nothing, $C_1 = W_0 - C_0$. If you save and earn
a return, the opportunity set lets you get **more than one unit** of
future consumption per unit of current consumption deferred.

---

## 3. The capital-market rate

Suppose a frictionless capital market exists with a **constant**
borrowing-and-lending rate $r$. The terms of trade are simple:

$$
\text{Defer KES 1 today} \;\longrightarrow\; \text{receive KES } 1(1+r) \text{ tomorrow}.
$$

In this setting the MRT is the same for every bundle on the boundary
— the trade-off does not depend on how much you have already
deferred. The opportunity set is therefore bounded by a **straight
line** of slope

$$
\boxed{\;\text{MRT} \;=\; -\,(1 + r).\;}
$$

This line, often called the **capital-market line** or **trading
line**, is developed in detail in chapter
[[07 - Choice with Capital Markets]].

---

## 4. MRT in real-investment settings

When the only way to grow wealth is via **real investments** —
machinery, infrastructure, productive projects — the trade-off varies
with how much has already been invested.

The frontier becomes **concave** (diminishing returns to scale). At
any point on it the MRT is the local slope:

$$
\text{MRT} \;=\; -\,\frac{dC_1}{dC_0}\bigg|_{\text{frontier}}.
$$

We can express the MRT as $(1 + i)$ where $i$ is the **real rate of
return** on the *very last* shilling invested (the marginal
investment):

$$
\text{MRT} \;=\; 1 + i.
$$

Because of diminishing returns, $i$ **declines** as the level of real
investment increases. The MRT shrinks accordingly, which is why the
real-investment frontier flattens out near the top. Worked through in
chapter [[06 - Real Investment Decision]].

---

## 5. MRS vs MRT — the optimal-choice condition

Two distinct quantities now share the same units (slopes in
$(C_0, C_1)$-space):

| Quantity                  | Source                                              | Subjective or objective? |
| :------------------------ | :-------------------------------------------------- | :----------------------- |
| **MRS**                   | Indifference curves (chapter 04)                    | Subjective preference    |
| **MRT** (capital market)  | Constant rate $r$ — straight boundary               | Objective market         |
| **MRT** (real investment) | Concave frontier — declining in investment level    | Objective technology     |

The **optimal consumption bundle** is the one where

$$
\boxed{\;\text{MRS}^\ast \;=\; \text{MRT}^\ast.\;}
$$

Graphically: the unique tangency between the highest attainable
indifference curve and the opportunity set. Chapter
[[06 - Real Investment Decision]] solves this tangency in the
no-capital-market case; chapter
[[07 - Choice with Capital Markets]] solves it once borrowing and
lending are available.

---

## Cross‑references

- [[04 - Indifference Curves, MRS and Utility]] — the MRS paired
  against MRT here.
- [[02 - Consumption and Savings Decision]] — the choice problem this
  trade-off describes.
- [[06 - Real Investment Decision]] — concave-frontier case worked
  through; first tangency solved.
- [[07 - Choice with Capital Markets]] — straight-line trading line at
  slope $-(1+r)$.
- [[08 - Fisher's Separation Theorem]] — when both opportunity sets
  coexist.
- [[../Fixed Income Securities/22 April 2026]] — discounting at rate
  $r$ in bond valuation is the same $(1+r)$ machinery.
- [[../Fixed Income Securities/22 April online excercise]] — forward
  rates discount cash flows using the same logic.
