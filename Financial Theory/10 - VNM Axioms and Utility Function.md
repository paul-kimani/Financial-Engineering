# 10 — VNM Axioms and Utility Function

> Six axioms — extending the four axioms of chapter
> [[03 - Axioms of Choice under Certainty]] to gambles — pin down a
> measurable **expected-utility** representation of preferences. The
> resulting object is the **Von Neumann–Morgenstern (VNM) utility
> function**. Its values are still real numbers, but it is defined over
> *uncertain* payoffs.

---

## 1. The setup — gambles

A **gamble** (or lottery) is a random payoff. We write

$$
G(x, y, p) \;\equiv\; \text{pays } x \text{ with probability } p,\; y \text{ with probability } 1 - p.
$$

By construction $G(x, y, p) = G(y, x, 1 - p)$.

The six axioms below tell us when preferences over such gambles admit
a well-behaved expected-utility representation.

---

## 2. The six VNM axioms

### Axiom 1 — Comparability (Completeness)

For the entire set of uncertain alternatives, the investor can
*always* say:

- $x \succ y$, or
- $y \succ x$, or
- $x \sim y$ (indifference).

No "I can't decide."

### Axiom 2 — Transitivity

If $x \succ y$ and $y \succ z$, then $x \succ z$. If $x \sim y$ and
$y \sim z$, then $x \sim z$.

### Axiom 3 — Strong Independence

Take three alternatives $a_1, a_2, a_3$. Compare two lotteries that
share a common "tail" outcome $a_3$:

- Lottery I: gives $a_1$ with probability $p$, $a_3$ with probability
  $1 - p$.
- Lottery II: gives $a_2$ with probability $p$, $a_3$ with probability
  $1 - p$.

Then:

- If $a_1 \sim a_2$, the investor is indifferent between Lottery I
  and Lottery II.
- If $a_1 \succ a_2$, the investor prefers Lottery I.

**Plain reading.** Mixing both candidates with the same third gamble
doesn't change the ordering. The common tail "factors out."

### Axiom 4 — Continuity / Measurability

Take $a_1 \succ a_2 \succ a_3$. Continuity says there exists a
probability $p \in (0, 1)$ such that the investor is **indifferent**
between $a_2$ for certain and the gamble $G(a_1, a_3, p)$:

$$
p\,U(a_1) + (1 - p)\,U(a_3) \;=\; U(a_2).
$$

The certain alternative $a_2$ here is the **certainty equivalent** of
the gamble — the topic of chapter
[[11 - Risk Aversion - Certainty Equivalent and Risk Premium]].

### Axiom 5 — Unequal Probability / Ranking

Take two lotteries over the same outcomes $a_1 \succ a_2$:

- Lottery I gives $a_1$ with probability $p_1$, $a_2$ with $1 - p_1$.
- Lottery II gives $a_1$ with probability $p_2$, $a_2$ with $1 - p_2$.

Then:

- Lottery I is preferred to Lottery II iff $p_1 > p_2$.
- The investor is indifferent iff $p_1 = p_2$.

Holding outcomes fixed, **higher probability of the better outcome
strictly increases preference**.

### Axiom 6 — Non-Satiation

Individuals prefer more to less — the same axiom as under certainty
(chapter [[03 - Axioms of Choice under Certainty]]).

---

## 3. The Von Neumann–Morgenstern utility function

If a preference relation over gambles satisfies the six axioms, there
exists a real-valued function $U(\cdot)$ — the **VNM utility
function** — and an expected-utility function $\text{EU}(\cdot)$ such
that for any gamble $G(x, z, p)$:

$$
\boxed{\;\text{EU}(x, z, p) \;=\; p\,U(x) \;+\; (1 - p)\,U(z).\;}
$$

The investor's preferences over gambles are exactly those induced by
expected utility:

$$
G_1 \succ G_2 \;\Longleftrightarrow\; \text{EU}(G_1) > \text{EU}(G_2).
$$

The utility function $U(\cdot)$ here is the *usual* utility on certain
monetary payoffs — it assigns a real number to each *certain* amount
of money. $\text{EU}(\cdot)$ extends this to lotteries linearly in
probabilities.

> **Important domain distinction.** $U(\cdot)$ is defined over
> *certain* monetary payments. $\text{EU}(\cdot)$ is defined over
> *uncertain* asset payoffs. They share a symbol in many texts but
> live in different spaces.

---

## 4. The certainty equivalent

The Continuity axiom (§2) gives a powerful object — the **certainty
equivalent** of a gamble.

> **Definition.** The certainty equivalent of $G(x, z, p)$ is the
> certain amount $y$ such that
> $U(y) = \text{EU}(x, z, p)$,
> i.e. the investor is indifferent between receiving $y$ for certain
> and accepting the gamble.

The certainty equivalent is the **maximum price** the investor is
willing to pay to obtain the gamble — equivalently, the gamble's
"cash-in-hand" value to that investor. The gap between expected payoff
and certainty equivalent is the **risk premium**, developed in chapter
[[11 - Risk Aversion - Certainty Equivalent and Risk Premium]].

---

## 5. Why the six VNM axioms, not the four of chapter 03?

The four certainty axioms of [[03 - Axioms of Choice under Certainty]]
are not enough once payoffs become uncertain. We need extra structure
to handle probabilities:

| New axiom              | What it adds                                                                                                                |
| :--------------------- | :-------------------------------------------------------------------------------------------------------------------------- |
| **Strong Independence** | Allows the expected-utility *linear-in-probabilities* representation. Without it, $\text{EU}$ need not split as a weighted sum. |
| **Continuity**          | Guarantees a certainty equivalent exists for every gamble (no "lexicographic" preferences that refuse any trade-off).        |
| **Ranking**             | Pins down how preferences move with the probability of the better outcome — the building block of dominance arguments.       |

Completeness, Transitivity, Non-Satiation are inherited from the
certainty axioms. Convexity does *not* re-appear here — its role under
uncertainty is taken over by the **shape of $U(\cdot)$** (concave for
risk aversion), see chapter
[[11 - Risk Aversion - Certainty Equivalent and Risk Premium]].

---

## Cross‑references

- [[03 - Axioms of Choice under Certainty]] — the four-axiom analogue
  under certainty.
- [[09 - Expected Utility Theory]] — why expected utility is the right
  criterion in the first place.
- [[11 - Risk Aversion - Certainty Equivalent and Risk Premium]] —
  uses the certainty equivalent defined here.
- [[12 - Pratt-Arrow Risk Aversion]] — local measures of risk
  aversion built on $U(\cdot)$.
- [[04 - Indifference Curves, MRS and Utility]] — the certainty-side
  $U(\cdot)$.
- [[../QUANTFRAME/Concepts/BackGround to Kelly Criterion]] — log
  utility is the canonical VNM utility for repeated betting.
