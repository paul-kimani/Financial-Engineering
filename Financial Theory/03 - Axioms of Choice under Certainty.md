# 03 — Axioms of Choice under Certainty

> Before we can map preferences to a utility function, we have to agree
> on what "rational" preferences look like. Four axioms — Completeness,
> Non-Satiation, Transitivity, Convexity — give the minimum conditions
> for consistent rational behaviour and constrain the *shape* of the
> indifference curves we draw in chapter
> [[04 - Indifference Curves, MRS and Utility]].

---

## 1. Why axioms?

For the mathematical theory of choice to work, we have to assume the
investor is rational. **Axioms of choice** are assumptions about the
way choices are made that are accepted without verification — they are
the *minimum set of conditions* for consistent rational behaviour.

Each axiom has a graphical consequence on the indifference curves
defined in chapter [[04 - Indifference Curves, MRS and Utility]]. Take
the axioms away and the indifference-curve picture collapses.

The four axioms in this chapter apply under **certainty** — all
payoffs are known. The expanded six-axiom system used under
uncertainty lives in chapter
[[10 - VNM Axioms and Utility Function]].

---

## 2. Axiom 1 — Completeness (or Comparability)

The decision maker, considering the entire set of available alternatives,
can compare any two of them. There is no "I don't know."

For any two alternatives $a_1, a_2$ the investor must be able to state
exactly one of:

1. $a_1 \succ a_2$ — alternative $a_1$ is preferred to $a_2$.
2. $a_2 \succ a_1$ — alternative $a_2$ is preferred to $a_1$.
3. $a_1 \sim a_2$ — the investor is indifferent.

The entire space of choice can be mapped — every pair admits a verdict.

---

## 3. Axiom 2 — Non-Satiation ("more is better")

Other things equal, the individual always prefers **more** of any
given asset to less. Applied to the two-period consumption problem,
this means the investor desires more current consumption *and* more
future consumption.

**Graphical impact.** Indifference curves further up and to the right
on a graph represent **higher** utility:

$$
I_3 \succ I_2 \succ I_1,
$$

because moving up-and-right means more $C_0$ *and* more $C_1$.

---

## 4. Axiom 3 — Transitivity (Consistency)

Preferences must be logically consistent.

- If $a_1 \succ a_2$ and $a_2 \succ a_3$, then $a_1 \succ a_3$.
- If $a_1 \sim a_2$ and $a_2 \sim a_3$, then $a_1 \sim a_3$.

**Graphical consequence — indifference curves never intersect.**
Suppose two curves crossed at point $B$. Pick $A$ on the *higher*
curve and $C$ on the lower one. By construction $A \sim B$ and
$B \sim C$, so transitivity forces $A \sim C$. But $A$ has more of
both goods than $C$, so by Non-Satiation $A \succ C$. Contradiction.

So **Transitivity + Non-Satiation $\Rightarrow$ indifference curves
cannot cross**.

---

## 5. Axiom 4 — Convexity (Diversification / Love of Variety)

If $x$ and $y$ are two bundles such that $U(x) = U(y)$, and if $z$ is a
weighted combination

$$
z = \alpha\,x + (1 - \alpha)\,y, \qquad 0 \le \alpha \le 1,
$$

then

$$
U(z) \;\ge\; U(x) = U(y).
$$

**Plain reading.** Take two bundles between which the investor is
indifferent and build a new bundle whose components are commodity-by-
commodity weighted averages of $x$ and $y$. The investor *never*
prefers either pure bundle to the combination — agents **love
variety**.

**Graphical consequence — indifference curves are convex toward the
origin.** This is exactly the shape that gives a *diminishing* MRS as
$C_0$ increases (chapter [[04 - Indifference Curves, MRS and Utility]]).

---

## 6. Summary — what the axioms buy you

| Axiom             | Plain meaning                                            | Graphical consequence                                     |
| :---------------- | :------------------------------------------------------- | :-------------------------------------------------------- |
| **Completeness**  | Every pair of alternatives admits a verdict.             | Every point in the plane sits on some indifference curve. |
| **Non-Satiation** | More is better.                                          | Curves further up-and-right give higher utility.          |
| **Transitivity**  | Preferences are consistent across chains of comparisons. | Indifference curves never intersect.                      |
| **Convexity**     | A mixture is at least as good as either ingredient.      | Curves are convex toward the origin (diminishing MRS).    |

With these four in place, the indifference curves drawn in
[[04 - Indifference Curves, MRS and Utility]] are well-defined,
non-crossing, downward-sloping, and convex — exactly the shapes used to
solve the tangency conditions in chapters
[[06 - Real Investment Decision]] and
[[07 - Choice with Capital Markets]].

---

## Cross‑references

- [[04 - Indifference Curves, MRS and Utility]] — the shapes these
  axioms force.
- [[02 - Consumption and Savings Decision]] — the choice problem the
  axioms underwrite.
- [[06 - Real Investment Decision]] — first use of the convex
  indifference curves to find a tangency.
- [[10 - VNM Axioms and Utility Function]] — the six-axiom analogue
  used under uncertainty.
- [[../QUANTFRAME/Concepts/BackGround to Kelly Criterion]] — log
  utility is the canonical utility consistent with these axioms in a
  repeated-betting setting.
