# 03 — Itô's Lemma: Statement and Heuristic Derivation

> The stochastic chain rule. Stated, then derived via a second‑order Taylor
> expansion combined with the box‑calculus multiplication rules. This is the
> level of detail expected in most applied finance exams; for the rigorous
> $L^2$ proof see [[04 - Proof of Itô's Lemma]].

---

## 1. Statement

Let $X_t$ be a stochastic process governed by the SDE

$$
dX_t \;=\; \mu(X_t, t)\,dt + \sigma(X_t, t)\,dW_t.
$$

Let $f(t, x)$ be a function that is once continuously differentiable in $t$
and twice continuously differentiable in $x$ (i.e. $f \in C^{1,2}$). Then
$Y_t = f(t, X_t)$ is itself an Itô process and

$$
\boxed{\;df(t, X_t) \;=\; \left(\frac{\partial f}{\partial t} + \mu\,\frac{\partial f}{\partial x} + \tfrac{1}{2}\sigma^2\,\frac{\partial^2 f}{\partial x^2}\right) dt \;+\; \sigma\,\frac{\partial f}{\partial x}\,dW_t.\;}
$$

The extra term $\tfrac{1}{2}\sigma^2\,\partial_x^2 f$ is the **Itô
correction**. It is the entire reason stochastic calculus differs from
ordinary calculus.

---

## 2. Heuristic derivation (Taylor + box calculus)

### Step 1 — Taylor expansion in two variables

For a smooth function $f(t, X)$ the change $\Delta f$ when both $t$ and $X$
move is

$$
\Delta f \;=\; \frac{\partial f}{\partial t}\,\Delta t + \frac{\partial f}{\partial X}\,\Delta X + \tfrac{1}{2}\frac{\partial^2 f}{\partial X^2}\,(\Delta X)^2 + \tfrac{1}{2}\frac{\partial^2 f}{\partial t^2}\,(\Delta t)^2 + \frac{\partial^2 f}{\partial X\,\partial t}\,\Delta X\,\Delta t + \cdots
$$

As $\Delta t \to 0$ we pass to differentials. Higher‑order terms
$(\Delta t)^2$ and $\Delta X\,\Delta t$ are negligible compared to $\Delta t$
— they drop out.

### Step 2 — Analyse the $(\Delta X)^2$ term

Recall the SDE in increment form, $\Delta X = \mu\,dt + \sigma\,dW_t$. Then

$$
(\Delta X)^2 \;=\; (\mu\,dt + \sigma\,dW_t)^2 \;=\; \mu^2\,(dt)^2 + 2\mu\sigma\,(dt)(dW_t) + \sigma^2\,(dW_t)^2.
$$

### Step 3 — Apply the box calculus

The multiplication rules for infinitesimal increments (see §3 below) are
$(dt)^2 = 0$, $(dt)(dW_t) = 0$, and $(dW_t)^2 = dt$. Therefore

$$
(dX_t)^2 \;=\; \sigma^2\,dt.
$$

### Step 4 — Substitute back

$$
df \;=\; \frac{\partial f}{\partial t}\,dt + \frac{\partial f}{\partial X}\,dX_t + \tfrac{1}{2}\frac{\partial^2 f}{\partial X^2}\,(dX_t)^2.
$$

Plugging $dX_t = \mu\,dt + \sigma\,dW_t$ and $(dX_t)^2 = \sigma^2\,dt$:

$$
df \;=\; \frac{\partial f}{\partial t}\,dt + \frac{\partial f}{\partial X}\bigl[\mu\,dt + \sigma\,dW_t\bigr] + \tfrac{1}{2}\frac{\partial^2 f}{\partial X^2}\,\sigma^2\,dt.
$$

### Step 5 — Group $dt$ and $dW_t$ terms

$$
df \;=\; \left(\frac{\partial f}{\partial t} + \mu\,\frac{\partial f}{\partial X} + \tfrac{1}{2}\sigma^2\,\frac{\partial^2 f}{\partial X^2}\right) dt + \sigma\,\frac{\partial f}{\partial X}\,dW_t. \quad \blacksquare
$$

---

## 3. The box calculus (multiplication table)

In stochastic calculus $dt$ is an infinitesimal so small that higher‑order
powers $(dt)^2, (dt)^3, \dots$ can be treated as $0$. The Wiener increment
$dW_t$ has variance $dt$, which forces a non‑standard cross term.

| ×       | $dt$ | $dW_t$ |
| :------ | :--: | :----: |
| $dt$    |  0   |   0    |
| $dW_t$  |  0   | $dt$   |

In words:

- $(dt)^2 \to 0$ (negligible).
- $dt \cdot dW_t \to 0$ (negligible).
- $(dW_t)^2 = dt$ ← **the only non‑zero second‑order term**.

The third entry is the reason for the Itô correction.

---

## 4. Why does the correction term appear?

A one‑line answer:

> Because $(dW_t)^2$ is *order $dt$*, not $0$. In ordinary calculus $(dx)^2$
> is order $(dt)^2$ and vanishes.

The contrast in tabular form:

| Standard calculus            | Stochastic calculus                                |
| :--------------------------- | :------------------------------------------------- |
| $(dt)^2 = 0$                 | $(dt)^2 = 0$                                       |
| $(dx)^2 \approx 0$           | $(dW_t)^2 = dt$                                    |
| Taylor truncated at 1st order | Taylor truncated at **2nd** order (because of $dt$) |

---

## 5. Multi‑dimensional generalisation

For a function of time and two stochastic variables $f(X^1, X^2, t)$ the
differential picks up cross‑derivative terms:

$$
df(X^1, X^2, t) \;=\; \frac{\partial f}{\partial t}\,dt + \frac{\partial f}{\partial X^1}\,dX^1 + \frac{\partial f}{\partial X^2}\,dX^2 + \tfrac{1}{2}\frac{\partial^2 f}{\partial (X^1)^2}\,(dX^1)^2 + \tfrac{1}{2}\frac{\partial^2 f}{\partial (X^2)^2}\,(dX^2)^2 + \frac{\partial^2 f}{\partial X^1\,\partial X^2}\,(dX^1)(dX^2).
$$

The cross term $(dX^1)(dX^2)$ is governed by the *correlation* between the
two driving Brownian motions, $\rho\,dt$.

---

## Cross‑references

- [[02 - The Itô Integral]] — the integral against which Itô's lemma is
  stated.
- [[04 - Proof of Itô's Lemma]] — rigorous derivation via $L^2$ limits.
- [[06 - Itô's Lemma Worked Examples]] — forward and inverse contracts.
- [[07 - Solving SDEs with Itô's Lemma]] — using Itô's lemma to find closed
  forms for $\int W_s\,dW_s$ and the GBM solution.
- [[08 - Itô Integral Exercises and Moments]] — computing $\mathbb{E}[W_t^n]$
  by combining Itô's lemma with the integral‑expectation trick.
