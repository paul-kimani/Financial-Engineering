# 03 — Duality of Linear Programming

> Every LP problem (the **primal**) has a paired LP problem (the **dual**)
> built from the *same data* — $A$, $b$, $c$ — but with the roles of
> variables and constraints swapped. Solving one tells you a lot about the
> other, including how much each resource is actually "worth." See
> [[01 - Optimization and Linear Programming Fundamentals]] for the decision
> variable / objective / constraint vocabulary this chapter assumes.

---

## 1. Why duality matters

Duality isn't just an algebra trick. It gives you three things:

- **Economic interpretation** — dual variables = **shadow prices**, the marginal value of each resource in the primal.
- **Computational flexibility** — sometimes the dual is much easier to solve than the primal, and solving one solves the other.
- **Theoretical bounds** — the dual's objective value always bounds the primal's, which is what eventually proves optimality (see §4, Weak Duality).

**Note.** The primal and dual are two views of one underlying resource-allocation problem — the primal asks "how do I use my resources optimally?", the dual asks "what is each unit of resource actually worth?"

---

## 2. Primal vs. dual — basic vocabulary

| Term | Definition |
|---|---|
| **Primal problem** | The original LP you're trying to solve |
| **Dual problem** | The related LP obtained by transforming the primal — swaps the *roles* of variables and constraints |

**Shadow prices.** The dual variables ($y_i$) represent the shadow price of each primal constraint (resource). If $y_1^* = 10$, it means: *"if I had one more unit of resource 1, my optimal profit would rise by 10."*

**Core relationships.**

- At the optimum, primal objective value **=** dual objective value (Strong Duality, §5).
- Primal **unbounded** $\Rightarrow$ dual **infeasible**.
- Primal **infeasible** $\Rightarrow$ dual is either infeasible or unbounded.

---

## 3. Building the dual via normalization

### 3.1 Normalize the primal first

| Form | Rule |
|---|---|
| **Normal Max problem** | ALL constraints are $\le$ |
| **Normal Min problem** | ALL constraints are $\ge$ |

Both also require $x \ge 0$.

$$
\text{Normal Max:} \quad \text{Max } z = cx \;\; \text{s.t. } Ax \le b,\; x \ge 0
$$

$$
\text{Normal Min:} \quad \text{Min } z = cx \;\; \text{s.t. } Ax \ge b,\; x \ge 0
$$

**Getting to normal form.**

- A $\ge$ constraint in a Max problem (or a $\le$ in a Min problem) is flipped by **multiplying by $-1$**.
- An **equality** constraint ($=$) splits into two inequalities, $Ax \le b$ and $Ax \ge b$, then flip whichever one doesn't match the required direction.

### 3.2 Swap using the conversion table

| Primal LP | Dual LP |
|---|---|
| Normal **Max** problem | Normal **Min** problem |
| Matrix $A$ | $A^T$ |
| Objective coefficients ($c$) | become RHS of dual |
| RHS ($b$) | become objective coefficients of dual |
| # decision variables ($n$) | = # constraints in dual |
| # constraints ($m$) | = # decision variables in dual ($y \in \mathbb{R}^m$) |

**The two canonical pairs.**

$$
\text{Max } z = cx,\; Ax \le b,\; x\ge0
\quad\Longleftrightarrow\quad
\text{Min } w = b^Ty,\; A^Ty \ge c,\; y\ge0
$$

$$
\text{Min } z = cx,\; Ax \ge b,\; x\ge0
\quad\Longleftrightarrow\quad
\text{Max } w = b^Ty,\; A^Ty \le c,\; y\ge0
$$

**Mnemonic.** Max and Min always swap. RHS and objective coefficients trade places. $A$ transposes. Memorize the two pairs above rather than re-deriving them each time.

### 3.3 Worked example — the clean case

**Problem.** Write the dual of

$$
\text{Max } 3x_1+4x_2 \quad \text{s.t.}\quad \tfrac12x_1+2x_2\le30,\;\; 3x_1+x_2\le25,\;\; x_1,x_2\ge0
$$

$$A=\begin{pmatrix}\tfrac12 & 2\\ 3 & 1\end{pmatrix} \;\Rightarrow\; A^T=\begin{pmatrix}\tfrac12 & 3\\ 2 & 1\end{pmatrix}$$

**Dual.**

$$
\text{Min } 30y_1+25y_2 \quad \text{s.t.}\quad \tfrac12y_1+3y_2\ge3,\;\; 2y_1+y_2\ge4,\;\; y_1,y_2\ge0
$$

### 3.4 Worked example — mixed constraints

**Problem.** A Min problem with one $\le$ and one $\ge$ constraint must be normalized first:

$$
\text{Min } 3x_1+2x_2+2x_3 \;\; \text{s.t.}\;\; x_1+3x_2+x_3\le51,\;\; 4x_1+8x_2\ge24
$$

Flip the first by multiplying by $-1$: $-x_1-3x_2-x_3 \ge -51$. Now both are $\ge$, so it's a valid Normal Min problem, and the dual comes out as a Normal Max with the flipped RHS ($-51$) as one of the dual's objective coefficients.

### 3.5 Worked example — equality constraint

An equality like $2x_1+x_2=3$ becomes **two** constraints: $2x_1+x_2\ge3$ and $-2x_1-x_2\ge-3$, each contributing its own dual variable ($y_2$, $y_3$). This is why an equality constraint in the primal effectively produces a dual variable that is unrestricted in sign once you recombine $y_2 - y_3$.

**Practice.** Work through Example 2.1.3 from the unit yourself (3 constraints: $\le$, $=$, $\ge$) before checking the worked solution — it's the best single example for drilling the normalization step.

---

## 4. Weak Duality Theorem (WDT)

**Statement.** If both the primal (Max) and dual (Min) have feasible solutions, then for every feasible primal $x$ and every feasible dual $y$:

$$c^Tx \le b^Ty$$

(Reversed, $c^Tx \ge b^Ty$, if primal is Min and dual is Max.)

**Proof sketch.**

1. Primal feasibility: $Ax \le b \;\Rightarrow\; x^TA^T \le b^T \;\Rightarrow\; x^TA^Ty \le b^Ty$ (post-multiply by $y \ge 0$; multiplying a $\le$ by a non-negative vector preserves direction).
2. Dual feasibility: $A^Ty \ge c \;\Rightarrow\; y^TA \ge c^T \;\Rightarrow\; y^TAx \ge c^Tx$ (post-multiply by $x \ge 0$).
3. Since $y^TAx = x^TA^Ty$ (both are scalars — the same quantity, transposed), chain the two:

$$c^Tx \le y^TAx = x^TA^Ty \le b^Ty \;\;\Rightarrow\;\; c^Tx \le b^Ty$$

**The trick worth remembering.** $y^TAx$ is a $1\times1$ matrix (a scalar), so it equals its own transpose: $y^TAx = (y^TAx)^T = x^TA^Ty$. That identity is what connects the primal inequality chain to the dual inequality chain.

**Consequences.**

- **Optimality test:** if $x$ is primal-feasible, $y$ is dual-feasible, and $c^Tx = b^Ty$, then $x$ and $y$ are both optimal — necessary *and* sufficient.
- Primal unbounded $\Rightarrow$ dual infeasible.
- Dual unbounded $\Rightarrow$ primal infeasible.

**Duality gap.**

$$
\text{Duality gap} = c^Tx - b^Ty \quad(\text{equivalently } p^*-d^*\text{ at the optima})
$$

Always $\ge 0$. Zero $\iff$ Strong Duality holds; otherwise the gap is strictly positive and only weak duality holds.

---

## 5. Strong Duality Theorem (SDT)

**Statement.** If either the primal or the dual has an optimal solution, then so does the other, and their optimal objective values are equal:

$$c^Tx^* = b^Ty^*$$

This is stronger than WDT — WDT only gives an inequality between *any* feasible pair; SDT guarantees the gap closes to exactly zero at the optimum.

**Optimality conditions.** $x$ is optimal for $\text{Min } c^Tx \text{ s.t. } Ax\ge b, x\ge0$ if and only if:

1. $x$ is primal feasible: $Ax \ge b,\; x\ge0$
2. there exists $y$ that is dual feasible: $A^Ty \le c,\; y\ge0$
3. there is no duality gap: $c^Tx = b^Ty$

**Numeric check.** Primal optimal $(x_1,x_2)=(3, 2.8)$, $z^*=370$. Dual optimal $(y_1,y_2,y_3)=(0,10,10)$, $w^*=370$. $z^*=w^*=370$ confirms both are optimal via Strong Duality.

---

## 6. How solving the dual helps the primal

- **Insight into constraints** — dual values reveal which resources are binding (scarce/valuable) vs. slack.
- **Finding the primal solution** — the dual's optimal solution can sometimes directly recover the primal's optimal solution (complementary slackness — worth its own chapter later).
- **Sensitivity analysis** — dual variables show how the optimal objective value shifts as $b$ (resource availability) changes; this is the formal meaning of "shadow price."

---

## 7. Quick-reference cheat sheet

| Concept | One-line takeaway |
|---|---|
| Primal | The problem you actually care about |
| Dual | Same data, roles of variables/constraints swapped |
| Shadow price | Value of $y_i^*$ = marginal value of one more unit of resource $i$ |
| Normal Max | All constraints $\le$ |
| Normal Min | All constraints $\ge$ |
| Max $\leftrightarrow$ Min | Dual of a Max is always a Min, and vice versa |
| $A \to A^T$ | Technological coefficient matrix transposes |
| $c \leftrightarrow b$ | Objective coefficients and RHS swap roles |
| Weak Duality | $c^Tx \le b^Ty$ for *any* feasible pair (Max primal) |
| Strong Duality | $c^Tx^*=b^Ty^*$ at the *optimum*, whenever one side has an optimum |
| Duality gap | $c^Tx-b^Ty \ge 0$; zero $\iff$ strong duality |
| Primal unbounded | Dual infeasible |
| Dual unbounded | Primal infeasible |

---

## 8. Open questions for revision

- Practice deriving the dual for at least 2 primal problems with mixed constraint types, without looking at the table first.
- Work through complementary slackness — the unit hints at it ("solution of the dual can directly yield the solution of the primal") but doesn't formalize it yet; check if a later chapter covers it.
- Re-derive the WDT proof from memory — it's short, and it's the kind of thing that shows up as a "prove that..." exam question.

---
*Source: Topic 2 — Duality of Linear Programming (unit PDF, pp. 15–23).*
