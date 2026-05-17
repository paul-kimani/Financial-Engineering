# 05 — Diffusion Processes Catalogue

> The canonical SDEs used in financial engineering — Geometric Brownian
> Motion, Ornstein–Uhlenbeck, Vasicek, Cox–Ingersoll–Ross — with each SDE,
> its closed‑form solution where one exists, and a note on its strengths and
> drawbacks.

A **diffusion process** is a continuous‑time Markov process with continuous
sample paths, defined as the solution to an SDE of the form

$$
dX_t \;=\; \mu(t, X_t)\,dt + \sigma(t, X_t)\,dW_t.
$$

The drift $\mu$ controls the deterministic trend; the diffusion coefficient
$\sigma$ controls how the noise scales.

---

## 1. Geometric Brownian Motion (GBM)

**SDE**

$$
dS_t \;=\; \mu\,S_t\,dt + \sigma\,S_t\,dW_t.
$$

**Closed‑form solution**

$$
\boxed{\; S_t = S_0 \exp\left[ \left(\mu - \tfrac{1}{2}\sigma^2\right) t + \sigma W_t \right] \;}
$$

**Distribution**

$$
\ln S_t \;\sim\; \mathcal{N}\!\left(\ln S_0 + \bigl(\mu - \tfrac{1}{2}\sigma^2\bigr)t,\;\sigma^2 t\right).
$$

**Why it dominates equity modelling**

- $S_t > 0$ for all $t$ — log‑normal distribution.
- Both drift and volatility scale with the current level, matching the
  empirical observation that stock returns (not prices) are roughly
  stationary.
- Tractable: closed‑form moments and option prices (Black–Scholes).

> Full derivation and applications: [[10 - Geometric Brownian Motion]] and
> [[11 - GBM Probability Calculations]].

---

## 2. Ornstein–Uhlenbeck (OU) process

**SDE** (zero‑mean form)

$$
dX_t \;=\; -\theta\,X_t\,dt + \sigma\,dW_t, \quad \theta > 0.
$$

**Alternative mean‑reverting form** (towards a level $\mu$)

$$
dX_t \;=\; \theta(\mu - X_t)\,dt + \sigma\,dW_t.
$$

**Closed‑form solution**

$$
X_t \;=\; X_0\,e^{-\theta t} + \mu\bigl(1 - e^{-\theta t}\bigr) + \sigma\int_0^t e^{-\theta(t-s)}\,dW_s.
$$

**Properties**

- **Mean reverting:** $X_t$ is pulled back to the long‑run mean $\mu$ at
  speed $\theta$.
- Gaussian process — $X_t$ is normally distributed for all $t$.
- Stationary distribution: $\mathcal{N}\!\bigl(\mu,\;\sigma^2/(2\theta)\bigr)$.

**Uses**

Commodity prices; spread modelling; building block for the Vasicek rate
model.

---

## 3. Vasicek model (OU applied to interest rates)

**SDE**

$$
dr_t \;=\; a\,(b - r_t)\,dt + \sigma\,dW_t.
$$

The short rate $r_t$ mean‑reverts to a long‑term level $b$ at speed $a$.

**Closed‑form solution**

$$
r_t \;=\; r_0\,e^{-a t} + b\bigl(1 - e^{-a t}\bigr) + \sigma\int_0^t e^{-a(t-s)}\,dW_s.
$$

**Drawback.** Since the diffusion term is Gaussian and constant, the
Vasicek model theoretically allows **negative interest rates** with
non‑zero probability. (In some markets this is now realistic; historically
it was viewed as a flaw and motivated the CIR model.)

---

## 4. Cox–Ingersoll–Ross (CIR) model

An extension of Vasicek designed to **enforce non‑negative interest rates**
by making the volatility proportional to $\sqrt{r_t}$.

**SDE**

$$
dr_t \;=\; a\,(b - r_t)\,dt + \sigma\,\sqrt{r_t}\,dW_t.
$$

**Implicit (integrated) form**

$$
r_t \;=\; r_0\,e^{-a t} + b\bigl(1 - e^{-a t}\bigr) + \sigma\int_0^t e^{-a(t-s)}\,\sqrt{r_s}\,dW_s.
$$

This is *not* a closed‑form because $\sqrt{r_s}$ still depends on the
unknown process $r_s$ inside the integrand.

**Distribution.** Because of the square‑root diffusion, $r_t$ follows a
**non‑central chi‑squared** distribution.

**Feller condition.** As long as $2 a b \ge \sigma^2$, the rate $r_t$
remains strictly positive almost surely. If the condition fails, $r_t$ can
hit zero (though it cannot cross into negatives).

**Uses**

- Short‑rate modelling where positivity matters.
- Heston model of stochastic volatility (the variance follows a CIR).

---

## 5. Brownian bridge

A Brownian motion **conditioned to end at a specific value** $b$ at a
future time $T$:

**SDE**

$$
dX_t \;=\; \frac{b - X_t}{T - t}\,dt + dW_t, \quad 0 \le t < T.
$$

**Properties**

- $X_T = b$ almost surely (the path is *pinned* at $T$).
- Marginal distribution is Gaussian with mean and variance interpolating
  between the start and end conditions.

**Uses**

Bond prices that must converge to par at maturity; interpolation between
two observed values in path simulation.

---

## 6. At‑a‑glance comparison

| Process              | Drift                                  | Diffusion       | Sign of $X_t$        | Distribution            |
| :------------------- | :------------------------------------- | :-------------- | :------------------- | :---------------------- |
| Arithmetic BM        | $\mu$                                  | $\sigma$        | can be negative      | Normal                  |
| **GBM**              | $\mu\,S_t$                             | $\sigma\,S_t$   | strictly positive    | Log‑normal              |
| **OU / Vasicek**     | $\theta(\mu - X_t)$                    | $\sigma$        | can be negative      | Normal                  |
| **CIR**              | $a(b - r_t)$                           | $\sigma\sqrt{r_t}$ | non‑negative      | non‑central $\chi^2$    |
| **Brownian bridge**  | $(b - X_t)/(T - t)$                    | $1$             | can be negative      | Normal, pinned at $T$   |

---

## Cross‑references

- [[06 - Itô's Lemma Worked Examples]] — derive derivative SDEs from a GBM
  underlying.
- [[07 - Solving SDEs with Itô's Lemma]] — full derivation of the GBM
  closed‑form solution.
- [[10 - Geometric Brownian Motion]] — dedicated chapter on GBM.
- [[../Fixed Income Securities/]] — Vasicek and CIR in the context of term
  structure models.
