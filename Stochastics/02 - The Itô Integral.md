# 02 — The Itô Integral

> Building the Itô integral $\int_0^T X_s\,dW_s$ from the easiest case
> (constants) to general adapted processes, via *simple processes* and a
> limiting procedure. The key payoff is the **Itô isometry** and the
> **martingale property** that powers no‑arbitrage pricing.

---

## 1. Why a new integral?

We want to compute things like

> *(Amount of stock I own) × (change in stock price).*

The "change in stock price" is Brownian motion $B_t$. But $B_t$ is **too
jagged** to use ordinary calculus (like trying to measure the length of a
lightning bolt with a ruler — its **total variation is infinite** on any
interval).

The fix is the classic "start easy, then take a limit" trick:

1. Define the integral for the simplest possible integrands (constants).
2. Extend to step functions ("simple processes").
3. Take a mean‑square limit to general adapted processes.

---

## 2. Simple processes — the staircase

Before integrating complex random functions, start with a "dummy" function
called a **simple process**, denoted $X(t)$.

- **Non‑random:** values are predetermined; no surprises.
- **Simple:** it is just a step function — a staircase.

Formally,

$$
X(t) \;=\; c_0\,\mathbb{1}_{\{0\}}(t) \;+\; \sum_{i=0}^{n-1} c_i\,\mathbb{1}_{(t_i,\,t_{i+1}]}(t).
$$

Translation: *between time $t_i$ and time $t_{i+1}$ the value is the
constant $c_i$.*

---

## 3. Defining the Itô integral

In ordinary calculus an integral is roughly *height × small time step*. In
Itô calculus the integral $\int_0^T X(t)\,dB(t)$ is the *height of the step
function* times the *small step in Brownian motion* $dB(t)$.

For our staircase,

$$
\int_0^T X(t)\,dB(t) \;=\; \sum_{i=0}^{n-1} c_i\,\bigl(B(t_{i+1}) - B(t_i)\bigr).
$$

- $c_i$ is the constant height of the step function during a specific
  time chunk.
- $\bigl(B(t_{i+1}) - B(t_i)\bigr)$ is the **increment** — how much
  Brownian motion moved up or down in that exact same time chunk.
- Multiply and sum.

---

## 4. Building up from base cases

### 4.1 Constant $X(t) = 1$

> *If you hold exactly one share from open to close*, your P&L is just
> $B(T) - B(0)$.

$$
\int_0^T 1 \cdot dB(t) \;=\; B(T) - B(0).
$$

### 4.2 Constant $X(t) = c$

If you hold $c$ shares all day, your P&L is $c\,(B(T) - B(0))$. This is
the only case where the Itô integral behaves exactly like a normal
high‑school integral.

### 4.3 Two‑step process

If $X(t) = c_1$ on $(0, a]$ and $c_2$ on $(a, T]$:

$$
\int_0^T X(t)\,dB(t) \;=\; c_1\,\bigl(B(a) - B(0)\bigr) + c_2\,\bigl(B(T) - B(a)\bigr).
$$

**Visual analogy:** $B(t)$ is a wiggly road going uphill, and $X(t)$ is the
gear you are in. Stay in gear 2 for the first mile, shift to gear 5 for the
second mile, add up the engine work.

### 4.4 Limiting procedure — staircases to slides

In real markets a trader rebalances continuously. The "staircase" $X(t)$
becomes a smooth curve:

1. Approximate the smooth curve by a staircase with $n$ tiny steps.
2. Compute the integral for that staircase (sum of $n$ tiny P&Ls).
3. Take $n \to \infty$ (mean‑square limit).

**The Itô choice — left endpoint:** on the step from $9{:}00{:}00$ to
$9{:}00{:}01$, the integrand is evaluated *at* $9{:}00{:}00$. You cannot
look into the future. Using the midpoint would be cheating (predicting the
future) and would give the **Stratonovich integral** instead — which is
*not* a martingale and breaks no‑arbitrage pricing.

---

## 5. The Itô integral as a random variable

Because $\int X\,dB$ adds up pieces of Brownian motion (which are random),
the result is itself a **random variable** — specifically, a Gaussian one.

### 5.1 Mean

Each Brownian increment has mean zero, so the integral itself has mean
zero:

$$
\mathbb{E}\!\left[\int_0^T X(t)\,dB(t)\right] \;=\; 0.
$$

### 5.2 Independence of increments

Increments of Brownian motion over **non‑overlapping** time intervals are
independent. What happens between $t_1$ and $t_2$ does not care what
happened between $t_0$ and $t_1$.

### 5.3 Variance — the Itô isometry

Because the increments are independent, the total variance is the sum of
the individual variances. For the simple process the variance of a single
piece $c_i\,\Delta B_i$ is $c_i^2 (t_{i+1} - t_i)$, giving

$$
\operatorname{Var}\!\left[\int_0^T X(t)\,dB(t)\right] \;=\; \sum_{i=0}^{n-1} c_i^2\,(t_{i+1} - t_i) \;=\; \int_0^T X(t)^2\,dt.
$$

In the limit, for a general adapted integrand $f$:

$$
\boxed{\;\mathbb{E}\!\left[\left(\int_0^T f(t,\omega)\,dW_t\right)^2\right] \;=\; \mathbb{E}\!\left[\int_0^T f(t,\omega)^2\,dt\right].\;}
$$

This is the **Itô isometry**. It links the messy world of stochastic
integrals back to ordinary, predictable Lebesgue integrals, and it is the
central tool used to extend the integral to all square‑integrable adapted
processes.

---

## 6. Summary — "so what?"

| Concept                | Math symbol                            | Plain meaning                                                  |
| :--------------------- | :------------------------------------- | :------------------------------------------------------------- |
| **Simple process**     | $X(t) = c_i$ on intervals              | a trading plan: "buy and hold for 1 hour, then sell"           |
| **Integral of simple** | $\sum c_i\,\Delta B_i$                 | mark‑to‑market P&L from each holding period                    |
| **Limiting procedure** | $n \to \infty$ steps                   | moving from once‑an‑hour to once‑a‑microsecond rebalancing     |
| **Result**             | $\int X\,dB$                           | total P&L from a dynamic strategy in a random market           |

> *We know how to measure the area under a straight line. We know how to
> measure the area under a staircase. By making the steps infinitely tiny,
> we can measure the area under any (well‑behaved) curve — even a chaotic
> jagged one.*

---

## Cross‑references

- [[01 - Introduction to Stochastic Processes]] — Wiener process definition.
- [[03 - Itô's Lemma - Statement and Derivation]] — uses this integral.
- [[04 - Proof of Itô's Lemma]] — Itô isometry appears in the convergence
  argument.
- [[07 - Solving SDEs with Itô's Lemma]] — closes the loop by evaluating
  $\int_0^t W_s\,dW_s$ in closed form.
- [[09 - Martingales]] — the martingale property of the Itô integral.
