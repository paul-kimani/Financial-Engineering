# 06 — Real Investment Decision

> Strip out the capital markets. The only way to convert today-money
> into tomorrow-money is via **real investments** — machinery,
> infrastructure, productive projects. The opportunity set becomes a
> concave frontier exhibiting diminishing returns, and the consumption
> decision is solved by tangency: $\text{MRS}^\ast = \text{MRT}^\ast$.

---

## 1. The setting

Assume no capital market — no borrowing, no lending at any rate $r$.
Investors transform current consumption into future consumption
*only* by undertaking real investments (machinery, infrastructure,
funding a startup, planting crops).

Under this restriction the opportunity set is described by a
**concave frontier** $F$ in $(C_0, C_1)$-space, exhibiting **diminishing
returns to scale** as the level of real investment increases.

---

## 2. Reading the concave frontier

Starting from initial wealth $W_0$:

- **No investment.** Consume everything today: $C_0 = W_0$,
  $C_1 = 0$. This is the rightmost point on the frontier.
- **Making an investment.** Reduce current consumption to $C_0 < W_0$.
  The amount invested is $W_0 - C_0$. Moving left on the $C_0$-axis
  pushes the bundle *up* on the $C_1$-axis — the invested funds grow.

The frontier therefore connects $(W_0, 0)$ on the right to some upper
intercept on the $C_1$-axis on the left, curving concavely between
them.

### Why concave? — diminishing returns to scale

Unlike a flat bank interest rate, real investments exhibit **diminishing
returns**. The first investment (your first machine) yields huge
returns. As you keep investing (your tenth machine), the marginal
addition to future output gets smaller.

In symbols: the marginal product is decreasing — each extra unit of
real capital adds less future consumption than the previous one.

---

## 3. MRT on the concave frontier

The MRT at any point on the frontier is the absolute slope:

$$
\text{MRT} \;=\; -\,\frac{dC_1}{dC_0}.
$$

We can express it as $(1 + i)$ where $i$ is the **real rate of return**
on the *marginal* investment — the last shilling deferred:

$$
\text{MRT} \;=\; 1 + i.
$$

### The golden rule of the frontier

Because of diminishing returns:

> As the level of real investment **increases** (moving *left* on the
> $C_0$-axis and *up* on the $C_1$-axis), the marginal rate of return
> $i$ **decreases**. The MRT therefore declines as investment
> increases, and the slope of $F$ flattens near the top.

This is exactly the curvature condition that makes the tangency in §4
well-defined.

---

## 4. The optimal investment decision

The investor solves the consumption/savings problem by combining the
frontier $F$ (the *can-do*) with their indifference curves $I_1, I_2,
\dots$ (the *want-to*, chapter
[[04 - Indifference Curves, MRS and Utility]]).

The optimal bundle $(C_0^\ast, C_1^\ast)$ sits at the **tangency** of
the **highest attainable** indifference curve with the frontier.

At the tangency, the slopes match:

$$
\boxed{\;\text{MRS}^\ast \;=\; \text{MRT}^\ast.\;}
$$

### Why interior points are sub-optimal

Take an interior point $x'$ on the frontier where the indifference
curve through $x'$ is *flatter* than the frontier (i.e. $\text{MRS}_{x'}
< \text{MRT}_{x'}$). Move slightly to the *left* along $F$ — you invest
a tiny bit more, gaining future consumption at the rate $\text{MRT}_{x'}$,
while you would have been willing to give up current consumption at
only the (lower) rate $\text{MRS}_{x'}$. So the move is welfare-improving
and $x'$ cannot be optimal. The symmetric argument rules out points
where the indifference curve is steeper than the frontier. Only the
tangency survives.

---

## 5. Summary — first tangency solved

| Object                  | Role                                                        |
| :---------------------- | :---------------------------------------------------------- |
| **Frontier $F$**        | Opportunity set under real investment only (concave).       |
| **Indifference curves** | Subjective preferences (convex).                            |
| **MRT**                 | $1 + i$; declines with investment level.                    |
| **MRS**                 | Subjective time-preference rate; declines with $C_0$.       |
| **Optimum**             | Tangency: $\text{MRS}^\ast = \text{MRT}^\ast$.              |

The next chapter, [[07 - Choice with Capital Markets]], reintroduces
borrowing and lending at a constant rate $r$. The opportunity set
expands beyond the concave frontier alone and the optimal decision
splits into two cleanly separable steps — the content of
[[08 - Fisher's Separation Theorem]].

---

## Cross‑references

- [[05 - Opportunity Set and MRT]] — MRT defined, capital-market line
  contrast.
- [[04 - Indifference Curves, MRS and Utility]] — the indifference
  curves whose tangency with $F$ defines the optimum.
- [[03 - Axioms of Choice under Certainty]] — the axioms that
  guarantee the indifference curves have the right convex shape for
  this tangency to be unique.
- [[07 - Choice with Capital Markets]] — what changes when borrowing
  and lending are available.
- [[08 - Fisher's Separation Theorem]] — the decoupling that follows.
- [[../QUANTFRAME/Concepts/BackGround to Kelly Criterion]] — the
  optimal *bet fraction* is the discrete-time analogue of the marginal
  real investment here.
