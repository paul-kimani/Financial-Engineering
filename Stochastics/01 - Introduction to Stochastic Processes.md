# 01 — Introduction to Stochastic Processes

> The first formal pass through the objects we will study: stochastic
> processes, the contrast between ODEs and SDEs, the Wiener process, and a
> preview of the Itô integral and Itô's lemma.

---

## 1. Definitions

- **Definition 1.** A *stochastic process* is a collection of random
  variables indexed by time, representing systems that evolve under
  uncertainty.
- **Definition 2.** Denoted $\{X_t\}_{t \ge 0}$, it models the evolution of
  a system with inherent randomness. Unlike a deterministic model, you
  cannot predict the exact future state — only the **probability** of future
  states.

---

## 2. Ordinary vs. stochastic calculus

### 2.1 Ordinary calculus (deterministic)

Ordinary calculus treats deterministic functions — systems with no
randomness — whose evolution is captured by **Ordinary Differential
Equations (ODEs)**.

- **The derivative.** For a function $f(x)$,

$$
\frac{df}{dx} \;=\; \lim_{\Delta x \to 0} \frac{f(x + \Delta x) - f(x)}{\Delta x}.
$$

- **The ODE.** An ODE has only one term — the deterministic flow:

$$
dX_t \;=\; a(X_t, t)\,dt.
$$

### 2.2 Stochastic calculus (randomness introduced)

Financial markets are noisy and unpredictable, so we replace ODEs with
**Stochastic Differential Equations (SDEs)** that carry an extra "noise"
term:

$$
dX_t \;=\; \underbrace{\mu(X_t, t)\,dt}_{\text{drift / deterministic}}
\;+\; \underbrace{\sigma(X_t, t)\,dW_t}_{\text{volatility / randomness}}.
$$

See [[00 - Foundations Review]] for the side‑by‑side comparison table.

---

## 3. The Wiener process & Brownian motion

The $W_t$ in the SDE is the **Wiener process**, the mathematical foundation
of standard Brownian motion. This is the engine that drives the randomness.

### 3.1 Characteristics of a Wiener process $W_t$

1. **Starts at zero:** $W_0 = 0$.
2. **Independent increments:** future movements do not depend on past
   movements (consistent with the Efficient Market Hypothesis).
3. **Normally distributed increments:** for $s < t$,
   $W_t - W_s \sim \mathcal{N}(0, t - s)$.
4. **Continuous path:** the path is continuous everywhere but
   *differentiable nowhere* — infinitely jagged.
5. **No drift:** pure standard Brownian motion has no upward or downward
   trend.

### 3.2 Brownian motion with drift

A *generalised* Brownian motion adds a drift term:

$$
B_t \;=\; \underbrace{\mu t}_{\text{drift}} \;+\; \underbrace{\sigma W_t}_{\text{volatility}}.
$$

In quant trading $\mu$ is the expected return of an asset over time, and
$\sigma$ is the market volatility that scales the random shocks $W_t$.

> ⚠️ Despite the drift, $B_t$ can still go **negative** — making it a poor
> model for stock prices. Compare with Geometric Brownian Motion in
> [[10 - Geometric Brownian Motion]].

---

## 4. Stochastic integrals and Itô's lemma (preview)

Because $W_t$ is "infinitely jagged" (not differentiable in the standard
sense), classical Riemann integration fails on $\int X_s\,dW_s$.

### 4.1 The Itô integral

The Itô integral of a stochastic process $X_t$ against a Brownian motion is
defined as

$$
\int_0^t X_s\,dW_s,
$$

constructed as a mean‑square limit of left‑endpoint Riemann sums. Full
treatment in [[02 - The Itô Integral]].

### 4.2 Itô's lemma — the stochastic chain rule

This is the single most important theorem in financial mathematics.

> **Itô's lemma** is the stochastic counterpart to the chain rule in
> ordinary calculus.

If $X_t$ follows the general SDE

$$
dX_t \;=\; \mu(X_t, t)\,dt + \sigma(X_t, t)\,dW_t,
$$

and you want the differential of a new function $f(X_t, t)$ — for example,
pricing a derivative $f$ on an underlying $X_t$ — then

$$
df(X_t, t) \;=\; \Bigl(\tfrac{\partial f}{\partial t} + \mu\,\tfrac{\partial f}{\partial X_t} + \tfrac{1}{2}\sigma^2\,\tfrac{\partial^2 f}{\partial X_t^2}\Bigr) dt \;+\; \sigma\,\tfrac{\partial f}{\partial X_t}\,dW_t.
$$

### Why is this different from ordinary calculus?

Notice the **Itô correction term** $\tfrac{1}{2}\sigma^2\,\partial^2_x f$.
In ordinary calculus second‑order terms vanish in the limit. In stochastic
calculus the squared differential of a Wiener process is non‑zero:
$(dW_t)^2 = dt$. Because variance accumulates linearly with time, this
second‑derivative term *must* be included.

The derivation lives in [[03 - Itô's Lemma - Statement and Derivation]];
the rigorous proof lives in [[04 - Proof of Itô's Lemma]].

---

## Cross‑references

- [[00 - Foundations Review]] — vocabulary, ODE vs SDE, taxonomy.
- [[02 - The Itô Integral]] — building $\int X_s\,dW_s$ from the ground up.
- [[03 - Itô's Lemma - Statement and Derivation]] — derivation via Taylor +
  box calculus.
- [[05 - Diffusion Processes Catalogue]] — canonical SDEs used in finance.
