# 07 — Choice with Capital Markets

> Add a frictionless capital market with a constant rate $r$ to the
> real-investment world of chapter [[06 - Real Investment Decision]].
> Investors can now lend (consume less than income) or borrow (consume
> more), and the opportunity set is augmented by a **trading line** of
> slope $-(1+r)$. The optimal bundle sits where the highest attainable
> indifference curve touches that line.

---

## 1. The capital-market rate

Assume the borrowing and lending rates are equal and constant:

$$
r_b \;=\; r_l \;=\; r.
$$

For every KES 1 deferred today, the consumer receives KES $1(1 + r)$
tomorrow. For every KES 1 borrowed today, KES $1(1 + r)$ must be
repaid tomorrow.

This single $r$ does the work in everything that follows.

---

## 2. The opportunity set expands

Without a capital market the opportunity set is the concave frontier
$F$ alone (chapter [[06 - Real Investment Decision]]). With a capital
market the consumer can move *off* the frontier in two new ways:

- **Lend** the unspent portion of current wealth at rate $r$.
- **Borrow** against future wealth at rate $r$.

If the consumer first chooses an investment point $x' = (x_0', x_1')$
on $F$ and then lends or borrows, future consumption $C_1$ depends on
$C_0$ linearly.

### 2.1 Lending

If $C_0 < x_0'$ the consumer **lends** the difference $x_0' - C_0$ at
rate $r$. Future consumption is the future endowment $x_1'$ plus
principal-plus-interest on the loan:

$$
C_1 \;=\; x_1' \;+\; (1 + r)\,(x_0' - C_0).
$$

Rearranging:

$$
C_1 \;=\; (1 + r)\,x_0' \;+\; x_1' \;-\; (1 + r)\,C_0.
$$

### 2.2 Borrowing

If $C_0 > x_0'$ the consumer **borrows** $C_0 - x_0'$ and repays
principal plus interest from $x_1'$:

$$
C_1 \;=\; x_1' \;-\; (1 + r)\,(C_0 - x_0').
$$

Which rearranges to **exactly the same** linear equation:

$$
\boxed{\;C_1 \;=\; (1 + r)\,x_0' \;+\; x_1' \;-\; (1 + r)\,C_0.\;}
$$

---

## 3. The trading line

The equation in §2 is a **straight line** in $(C_0, C_1)$-space
passing through the chosen investment point $x' = (x_0', x_1')$ and
having constant slope

$$
\frac{dC_1}{dC_0} \;=\; -(1 + r).
$$

We call this line the **trading line** (or **capital-market line**).
Every $(C_0, C_1)$ on it is feasible: pick a point on the trading
line and the corresponding amount lent (if $C_0 < x_0'$) or borrowed
(if $C_0 > x_0'$) makes it reachable.

Interpretation of the slope: **reducing current consumption by KES 1
increases future consumption by KES $1(1+r)$** — the objective
market rate of time-substitution.

---

## 4. The optimal savings/consumption bundle

The optimal $(C_0^\ast, C_1^\ast)$ is the point where the **highest
attainable indifference curve** is tangent to the **trading line**.

At that tangency the slopes match:

$$
\boxed{\;\text{MRS}^\ast \;=\; 1 + r.\;}
$$

That is: the subjective rate of time preference equals the objective
market rate.

### Why points off the tangency are sub-optimal

Suppose $C_0'$ lies on the trading line but not at the tangency, and
the indifference curve through $C'$ has $\text{MRS} > (1+r)$ — the
consumer values present consumption more highly than the market does.
Lending some of $C_0'$ at rate $r$ moves them to a higher indifference
curve. The symmetric argument rules out the case
$\text{MRS} < (1+r)$. Only the tangency survives.

---

## 5. Combining with real investment

Two opportunity sets now coexist:

- the **concave frontier $F$** from real investments, and
- the **trading line** from the capital market.

The optimal *investment* point $x^\ast = (x_0^\ast, x_1^\ast)$ on $F$
and the optimal *consumption* point $C^\ast = (C_0^\ast, C_1^\ast)$ on
the trading line through $x^\ast$ are in general **different** points.
Real investment sets the *intercept* of the trading line; capital-market
borrowing or lending lets the consumer slide along the line to their
preferred consumption mix.

The remarkable fact — proved in the next chapter — is that the
investment decision $x^\ast$ does **not depend on the consumer's
preferences**. It depends only on the frontier and the market rate
$r$. That decoupling is the content of
[[08 - Fisher's Separation Theorem]].

---

## 6. What capital markets buy you

Without a capital market the consumer is forced to pin consumption to
their savings/investment choice: $C_0' = x_0'$ and $C_1' = x_1'$ —
exactly the single point on $F$ chosen.

With a capital market the consumer can:

- **Consume more than current endowment** by borrowing against future
  income; and
- **Build future wealth above the frontier-attainable level** by
  lending current income at rate $r$.

The result is a *higher attainable indifference curve* than $F$ alone
allows. The consumer is unambiguously better off — see also the
"Benefits of Capital Markets" discussion in
[[08 - Fisher's Separation Theorem]].

---

## Cross‑references

- [[06 - Real Investment Decision]] — the no-capital-market base case.
- [[08 - Fisher's Separation Theorem]] — the decoupling that the
  trading line enables, and the transaction-cost case that breaks it.
- [[05 - Opportunity Set and MRT]] — MRT on a flat capital-market line
  is exactly $1 + r$.
- [[04 - Indifference Curves, MRS and Utility]] — the subjective MRS
  paired against the market slope.
- [[../Fixed Income Securities/22 April 2026]] — bond present value
  uses exactly the same $(1+r)$ discounting.
- [[../Fixed Income Securities/15 April online Excercise]] — repo
  haircut and repurchase price built on $r$ over a short term.
