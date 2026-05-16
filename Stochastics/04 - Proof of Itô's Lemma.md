# 04 — Proof of Itô's Lemma

> The approach depends on the level of rigour expected. Three standard
> tiers:
>
> 1. **Heuristic "finance‑exam" proof** — Taylor expansion + box algebra.
> 2. **Rigorous proof** — telescoping sum, $L^2$ convergence, quadratic
>    variation.
> 3. **Statement only** — when exam time is short.
>
> Use this page as a reference; the heuristic derivation is also written up
> standalone in [[03 - Itô's Lemma - Statement and Derivation]].

---

## Scenario 1 — Standard finance‑exam heuristic proof

**Context.** You are asked to show *why* the extra term
$\tfrac{1}{2}f''(X_t)\sigma^2\,dt$ appears.
**Approach.** Second‑order Taylor expansion + box algebra for $dW^2$.

### Part 1 — Statement of Itô's lemma

Let $X_t$ be an Itô process satisfying

$$
dX_t \;=\; \mu_t\,dt + \sigma_t\,dW_t,
$$

and let $f(t, x)$ be twice continuously differentiable. Then $Y_t = f(t, X_t)$
is an Itô process with

$$
df(t, X_t) \;=\; \left(\frac{\partial f}{\partial t} + \mu_t\,\frac{\partial f}{\partial x} + \tfrac{1}{2}\sigma_t^2\,\frac{\partial^2 f}{\partial x^2}\right) dt + \sigma_t\,\frac{\partial f}{\partial x}\,dW_t.
$$

### Part 2 — Heuristic derivation

1. **Taylor expansion.**

   $$
   df \;=\; \frac{\partial f}{\partial t}\,dt + \frac{\partial f}{\partial x}\,dX + \tfrac{1}{2}\frac{\partial^2 f}{\partial x^2}\,(dX)^2 + \dots
   $$

   Higher‑order terms $(dt)^2$ and $dt\,dX$ go to zero faster than $dt$.

2. **Substitute $dX$ into the $(dX)^2$ term.**

   $$
   (dX)^2 \;=\; (\mu\,dt + \sigma\,dW)^2 \;=\; \mu^2\,(dt)^2 + 2\mu\sigma\,(dt)(dW) + \sigma^2\,(dW)^2.
   $$

3. **Apply the rules of thumb (quadratic variation).**

   - $(dt)^2 \approx 0$
   - $dt \cdot dW \approx 0$
   - $(dW)^2 = dt$ ← the *only* non‑zero second‑order term.

4. **Substitute back:** $(dX)^2 = \sigma^2\,dt$.

5. **Plug into the Taylor expansion:**

   $$
   df \;=\; \frac{\partial f}{\partial t}\,dt + \frac{\partial f}{\partial x}(\mu\,dt + \sigma\,dW) + \tfrac{1}{2}\frac{\partial^2 f}{\partial x^2}\,\sigma^2\,dt.
   $$

6. **Group $dt$ and $dW$ terms:**

   $$
   df \;=\; \left(\frac{\partial f}{\partial t} + \mu\,\frac{\partial f}{\partial x} + \tfrac{1}{2}\sigma^2\,\frac{\partial^2 f}{\partial x^2}\right) dt + \sigma\,\frac{\partial f}{\partial x}\,dW. \quad \square
   $$

---

## Scenario 2 — Rigorous proof

A deeper dive into the foundations. The heuristic proof is enough for most
applied finance exams; a rigorous proof requires building the result from
the ground up using measure theory and $L^2$ limits.

We will prove **Itô's lemma for Brownian motion** (the hardest part), then
state the extension to general Itô processes.

### Theorem (one‑dimensional Itô's lemma)

Let $f \in C^2(\mathbb{R})$ (twice continuously differentiable with bounded
derivatives) and let $B_t$ be a standard Brownian motion. Then for any
$t \ge 0$, almost surely

$$
f(B_t) \;=\; f(0) + \int_0^t f'(B_s)\,dB_s + \tfrac{1}{2}\int_0^t f''(B_s)\,ds.
$$

Or in differential form,

$$
df(B_t) \;=\; f'(B_t)\,dB_t + \tfrac{1}{2}f''(B_t)\,dt.
$$

### Part 1 — Setup (approximation by step functions)

Let $\Pi_n = \{0 = t_0 < t_1 < \dots < t_n = t\}$ be a sequence of
partitions with mesh $\|\Pi_n\| = \max_k |t_{k+1} - t_k| \to 0$. Use the
**telescoping sum**

$$
f(B_t) - f(0) \;=\; \sum_{k=0}^{n-1}\bigl[f(B_{t_{k+1}}) - f(B_{t_k})\bigr].
$$

### Part 2 — Taylor's theorem (rigorous version)

Since $f \in C^2$, apply the exact Taylor expansion with Lagrange remainder.
For each increment $\Delta B_k = B_{t_{k+1}} - B_{t_k}$:

$$
f(B_{t_{k+1}}) - f(B_{t_k}) \;=\; f'(B_{t_k})\,\Delta B_k + \tfrac{1}{2}f''(B_{t_k})\,(\Delta B_k)^2 + R_k,
$$

with remainder $|R_k| \le \varepsilon(|\Delta B_k|)\,(\Delta B_k)^2$ and
$\varepsilon(h) \to 0$ as $h \to 0$ (uniform continuity of $f''$ on
compacts, plus boundedness of $B_t$ in probability).

Summing,

$$
f(B_t) - f(0) \;=\; \underbrace{\sum_{k=0}^{n-1} f'(B_{t_k})\,\Delta B_k}_{(A)} + \tfrac{1}{2}\underbrace{\sum_{k=0}^{n-1} f''(B_{t_k})\,(\Delta B_k)^2}_{(B)} + \underbrace{\sum_{k=0}^{n-1} R_k}_{(C)}.
$$

We now examine the limits of $(A)$, $(B)$, $(C)$.

### Part 3 — Convergence of $(A)$: the Itô integral

By construction of the Itô integral, $\sum f'(B_{t_k})\,\Delta B_k$ is the
integral of a simple process (left‑endpoint evaluation). Since $f'$ is
continuous and $B_t$ has continuous paths, $f'(B_t)$ is adapted with finite
second moment, so

$$
\sum_{k=0}^{n-1} f'(B_{t_k})\,\Delta B_k \;\xrightarrow{L^2}\; \int_0^t f'(B_s)\,dB_s.
$$

### Part 4 — Convergence of $(B)$: the quadratic variation trick

This is the heart of the rigorous proof.

**Step 4.1 — Quadratic variation theorem.** For Brownian motion,

$$
\lim_{n \to \infty} \sum_{k=0}^{n-1} (\Delta B_k)^2 \;=\; t \quad (\text{in probability, and in } L^2).
$$

**Step 4.2 — Localisation.** We want to replace $(\Delta B_k)^2$ with
$\Delta t_k = t_{k+1} - t_k$. Define

$$
D_n \;=\; \sum_{k=0}^{n-1} f''(B_{t_k})\bigl[(\Delta B_k)^2 - \Delta t_k\bigr]
$$

and show $\mathbb{E}[D_n^2] \to 0$.

Expanding the square,

$$
\mathbb{E}[D_n^2] \;=\; \mathbb{E}\!\left[\sum_{k,j} f''(B_{t_k}) f''(B_{t_j})\bigl((\Delta B_k)^2 - \Delta t_k\bigr)\bigl((\Delta B_j)^2 - \Delta t_j\bigr)\right].
$$

**Key observation (independence of increments):** if $k \ne j$, WLOG
$k < j$. Then $\Delta B_j$ is independent of $\mathcal{F}_{t_j}$, which
contains $B_{t_k}$, $\Delta B_k$, $f''(B_{t_k})$. Since
$\mathbb{E}\bigl[(\Delta B_j)^2 - \Delta t_j \mid \mathcal{F}_{t_j}\bigr] = 0$,
the cross terms vanish in expectation. Only the diagonal survives.

**Step 4.3 — Bound the diagonal.**

$$
\mathbb{E}[D_n^2] \;=\; \sum_{k=0}^{n-1} \mathbb{E}\!\left[\bigl(f''(B_{t_k})\bigr)^2\bigl((\Delta B_k)^2 - \Delta t_k\bigr)^2\right].
$$

Since $f''$ is bounded (say by $M$),

$$
\mathbb{E}[D_n^2] \;\le\; M^2 \sum_{k=0}^{n-1} \mathbb{E}\!\left[\bigl((\Delta B_k)^2 - \Delta t_k\bigr)^2\right].
$$

For $Z \sim \mathcal{N}(0, \sigma^2)$ one has
$\mathbb{E}[(Z^2 - \sigma^2)^2] = 2\sigma^4$. Apply this with
$\Delta B_k \sim \mathcal{N}(0, \Delta t_k)$:

$$
\mathbb{E}\!\left[\bigl((\Delta B_k)^2 - \Delta t_k\bigr)^2\right] \;=\; 2\,(\Delta t_k)^2,
$$

so $\mathbb{E}[D_n^2] \le 2 M^2 \sum_k (\Delta t_k)^2$.

**Step 4.4 — Take the limit.** Because
$\sum_k (\Delta t_k)^2 \le \|\Pi_n\|\,\sum_k \Delta t_k = \|\Pi_n\|\,t \to 0$,
we get $\mathbb{E}[D_n^2] \to 0$, hence $L^2$ convergence:

$$
\sum f''(B_{t_k})\,(\Delta B_k)^2 \;\xrightarrow{L^2}\; \sum f''(B_{t_k})\,\Delta t_k \;\to\; \int_0^t f''(B_s)\,ds.
$$

### Part 5 — Convergence of $(C)$: the remainder

$|R_k| \le \varepsilon(|\Delta B_k|)\,(\Delta B_k)^2$. By
Cauchy–Schwarz,

$$
\left|\sum R_k\right| \;\le\; \sqrt{\sum \varepsilon(|\Delta B_k|)^2}\;\cdot\;\sqrt{\sum (\Delta B_k)^4}.
$$

- As $n \to \infty$, $\max_k |\Delta B_k| \to 0$ (uniform continuity of
  paths), so $\varepsilon(|\Delta B_k|) \to 0$.
- $\sum (\Delta B_k)^4 \to 0$ because
  $\mathbb{E}[\sum (\Delta B_k)^4] = 3\sum (\Delta t_k)^2 \to 0$.

So $\sum R_k \to 0$ in probability.

### Part 6 — Extension to general Itô processes

Once Itô's lemma is proven for $B_t$, the extension to
$dX_t = \mu_t\,dt + \sigma_t\,dB_t$ and $f(t, x)$ follows from the chain
rule and the definition of the integral against $X_t$. The time variable
adds the standard calculus term $\partial_t f\,dt$, and the drift term
$\mu_t\,dt$ produces zero cross‑variation with $dB_t$:

$$
f(t, X_t) - f(0, X_0) \;=\; \int_0^t \left(\frac{\partial f}{\partial s} + \mu_s\,\frac{\partial f}{\partial x} + \tfrac{1}{2}\sigma_s^2\,\frac{\partial^2 f}{\partial x^2}\right) ds + \int_0^t \sigma_s\,\frac{\partial f}{\partial x}\,dB_s. \quad \blacksquare
$$

### Summary of the rigorous proof structure

1. **Telescoping sum** — break the interval into small pieces.
2. **Taylor expansion** — apply exact calculus locally.
3. **Quadratic variation** — replace $(\Delta B)^2$ with $\Delta t$. (Only
   non‑standard step.)
4. **Isometry & independence** — variance of the error goes to zero.
5. **Limit argument** — $L^2$ convergence to the integrals.

---

## How to choose your approach in an exam

| If the question is…                                                                  | Your answer should be…                                                                                                                                          |
| :----------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **"State and *prove* Itô's lemma"**                                                  | Use **Scenario 1** (Taylor + box algebra). Show the expansion, highlight $(dW)^2 = dt$, group terms.                                                            |
| **"Derive the Black–Scholes PDE"**                                                   | You just **use** Itô's lemma. State it, apply it to $C(t, S_t)$, set $d\Pi = 0$. No need to re‑prove the lemma.                                                 |
| **"Give a heuristic argument for Itô's lemma"**                                      | Use **Scenario 1**.                                                                                                                                             |
| **"Why does the second derivative term appear in Itô but not standard calculus?"**    | Focus on Step 3 of Scenario 1: *because $(dW)^2$ behaves like $dt$, not $0$. In standard calculus $(dx)^2$ is order $(dt)^2$ and vanishes.*                     |

### Visual aid (good for an exam answer)

| Standard calculus            | Stochastic calculus                                |
| :--------------------------- | :------------------------------------------------- |
| $(dt)^2 = 0$                 | $(dt)^2 = 0$                                       |
| $(dx)^2 \approx 0$           | $(dW)^2 = dt$                                      |
| Taylor truncated at 1st order | Taylor truncated at 2nd order (because of $dt$)    |

---

## Cross‑references

- [[03 - Itô's Lemma - Statement and Derivation]] — same proof at lower
  detail, plus the multi‑dimensional extension.
- [[02 - The Itô Integral]] — definition of the Itô integral and the
  isometry used in Part 3.
- [[06 - Itô's Lemma Worked Examples]] — applies the lemma to forward and
  inverse contracts.
