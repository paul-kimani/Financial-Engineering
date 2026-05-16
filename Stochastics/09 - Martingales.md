# 09 — Martingales

> A martingale is the formal way of saying "fair game" — the best estimate
> of the future is the present. In finance the martingale property is the
> mathematical content of *no‑arbitrage*: under a suitable measure, the
> discounted price of any tradable asset is a martingale.

---

## 1. Definition

A stochastic process $\{M_t\}_{t \ge 0}$ adapted to a filtration
$\{\mathcal{F}_t\}$ is a **martingale** if it satisfies the following three
properties.

1. **Integrability.** $\mathbb{E}\bigl[|M_t|\bigr] < \infty$ for all
   $t \in T$.
2. **Adaptedness.** $M_t$ is $\mathcal{F}_t$‑measurable — the value of
   $M_t$ is known given the information up to time $t$.
3. **Martingale property.**
   - *Continuous time:* $\mathbb{E}[M_t \mid \mathcal{F}_s] = M_s$ for all
     $s \le t$.
   - *Discrete time:* $\mathbb{E}[M_{n+1} \mid \mathcal{F}_n] = M_n$.

> **Interpretation.** Conditional on everything we know today, the best
> estimate of the future value is today's value. There is no statistically
> exploitable trend.

### Related notions

| Concept              | Defining inequality                                          | Plain meaning                       |
| :------------------- | :----------------------------------------------------------- | :---------------------------------- |
| **Martingale**       | $\mathbb{E}[X_t \mid \mathcal{F}_s] = X_s$                   | a fair game                         |
| **Submartingale**    | $\mathbb{E}[X_t \mid \mathcal{F}_s] \ge X_s$                 | tendency to drift *up*              |
| **Supermartingale**  | $\mathbb{E}[X_t \mid \mathcal{F}_s] \le X_s$                 | tendency to drift *down*            |

---

## 2. Manipulating conditional expectations

Five rules that get used over and over when proving the martingale
property.

1. **Measurability (take out what's known).** If $X_n$ is
   $\mathcal{F}_n$‑measurable, then

   $$
   \mathbb{E}[X_n \mid \mathcal{F}_n] = X_n, \qquad \mathbb{E}[X_n\,Y \mid \mathcal{F}_n] = X_n\,\mathbb{E}[Y \mid \mathcal{F}_n].
   $$

2. **Independence.** If $X_{n+1}$ is independent of $\mathcal{F}_n$, then

   $$
   \mathbb{E}[X_{n+1} \mid \mathcal{F}_n] = \mathbb{E}[X_{n+1}].
   $$

3. **Tower property.** For $\mathcal{F}_n \subset \mathcal{F}_{n+1}$,

   $$
   \mathbb{E}\bigl[\mathbb{E}[X \mid \mathcal{F}_{n+1}] \mid \mathcal{F}_n\bigr] = \mathbb{E}[X \mid \mathcal{F}_n].
   $$

4. **Linearity.**

   $$
   \mathbb{E}[a Y + b X_{n+1} \mid \mathcal{F}_n] = a\,\mathbb{E}[Y \mid \mathcal{F}_n] + b\,\mathbb{E}[X_{n+1} \mid \mathcal{F}_n].
   $$

5. **Constants pull out.** $\mathbb{E}[c \mid \mathcal{F}_n] = c$ for any
   constant $c$.

Filtration chain: $\mathcal{F}_0 \subset \mathcal{F}_1 \subset \cdots \subset \mathcal{F}_n \subset \cdots$

---

## 3. Discrete‑time example — $M_n = S_n^2 - n$

**Claim.** Let $S_n = X_1 + X_2 + \dots + X_n$ where the $X_i$ are i.i.d.
with $\mathbb{E}[X_i] = 0$ and $\mathbb{E}[X_i^2] = 1$. Then
$M_n = S_n^2 - n$ is a martingale w.r.t. $\mathcal{F}_n = \sigma(X_1,\dots,X_n)$.

**Proof.**

$$
\mathbb{E}[M_{n+1} \mid \mathcal{F}_n] = \mathbb{E}\bigl[(S_{n+1})^2 - (n+1) \mid \mathcal{F}_n\bigr] = \mathbb{E}\bigl[(S_n + X_{n+1})^2 \mid \mathcal{F}_n\bigr] - (n+1).
$$

Expanding the square and using measurability of $S_n$ plus independence of
$X_{n+1}$:

$$
= S_n^2 + 2 S_n\,\mathbb{E}[X_{n+1}] + \mathbb{E}[X_{n+1}^2] - (n+1) = S_n^2 + 0 + 1 - (n+1) = S_n^2 - n = M_n. \quad \blacksquare
$$

---

## 4. Continuous‑time martingale tests (Itô formulation)

A second, very useful definition for *Itô processes*:

> **A stochastic process $X_t$ governed by**
> $\;dX_t = \mu(t, X_t)\,dt + \sigma(t, X_t)\,dB_t\;$
> **is a local martingale if and only if the drift coefficient is zero**:
> $\mu(t, X_t) \equiv 0$.

In plain words: an Itô process is a local martingale exactly when its drift
vanishes. Combined with an integrability condition (see below) this gives
you a *true* martingale.

### Key technical considerations

1. **Filtration.** The process must be adapted to the filtration
   $\mathcal{F}_t$ of the driving Brownian motion.
2. **Integrability.** For a *local* martingale to be a *true* martingale,
   it must satisfy an integrability condition. The standard sufficient
   condition is the **Novikov condition** — when it holds, expectations
   remain well defined.
3. **Risk‑neutral measure $\mathbb{Q}$.** In derivatives pricing we often
   change measure so that the *discounted* asset price $S_t\,e^{-r t}$
   becomes a martingale by killing the drift under $\mathbb{Q}$. This is
   the engine behind the Fundamental Theorem of Asset Pricing.

---

## 5. Exercise — which of these are martingales?

For each process, decide whether it is a martingale.

1. $\sqrt{t}$ — purely deterministic, increasing. **Not a martingale**
   (it is a submartingale).
2. $W_t^2 - t$ — *is* a martingale. See discrete analogue above; same
   logic in continuous time.
3. $\;e^{\sigma W_t - \tfrac{1}{2}\sigma^2 t}\;$ — *is* a martingale. This
   is the **exponential (or Doléans‑Dade) martingale** and is the engine of
   Girsanov's theorem.

A full proof of (3) follows.

---

## 6. Exponential martingale: $M_t = e^{\sigma W_t - \tfrac{1}{2}\sigma^2 t}$

We will prove $\mathbb{E}[M_t \mid \mathcal{F}_s] = M_s$ for $s \le t$ in
two ways.

### Method (i) — properties of expectation

Use the increment decomposition $W_t = W_s + (W_t - W_s)$:

$$
M_t = e^{\sigma W_s - \tfrac{1}{2}\sigma^2 s}\,\cdot\,e^{\sigma(W_t - W_s) - \tfrac{1}{2}\sigma^2 (t-s)} = M_s\,\cdot\,e^{\sigma(W_t - W_s) - \tfrac{1}{2}\sigma^2 (t-s)}.
$$

Take conditional expectation with respect to $\mathcal{F}_s$. Since $M_s$
is $\mathcal{F}_s$‑measurable (take out what's known), and
$W_t - W_s \perp \mathcal{F}_s$ (independent increments):

$$
\mathbb{E}[M_t \mid \mathcal{F}_s] = M_s \cdot e^{-\tfrac{1}{2}\sigma^2 (t-s)} \cdot \mathbb{E}\!\left[e^{\sigma(W_t - W_s)}\right].
$$

The increment $\sigma(W_t - W_s) \sim \mathcal{N}\!\bigl(0, \sigma^2(t-s)\bigr)$,
and the MGF of a Gaussian $\mathcal{N}(\mu, v)$ is $e^{\mu + v/2}$:

$$
\mathbb{E}\!\left[e^{\sigma(W_t - W_s)}\right] \;=\; e^{0 + \tfrac{1}{2}\sigma^2 (t-s)}.
$$

The two exponential factors cancel exactly:

$$
\mathbb{E}[M_t \mid \mathcal{F}_s] \;=\; M_s \cdot e^{-\tfrac{1}{2}\sigma^2(t-s)} \cdot e^{+\tfrac{1}{2}\sigma^2(t-s)} \;=\; M_s. \quad \blacksquare
$$

### Method (ii) — Itô's lemma (drift = 0 test)

Apply Itô's lemma to $F(t, W_t) = e^{\sigma W_t - \tfrac{1}{2}\sigma^2 t}$:

$$
\frac{\partial F}{\partial t} = -\tfrac{1}{2}\sigma^2 F, \quad \frac{\partial F}{\partial W_t} = \sigma F, \quad \frac{\partial^2 F}{\partial W_t^2} = \sigma^2 F.
$$

Then

$$
dF(t, W_t) = -\tfrac{1}{2}\sigma^2 F\,dt + \sigma F\,dW_t + \tfrac{1}{2}\sigma^2 F\,(dW_t)^2.
$$

Using $(dW_t)^2 = dt$:

$$
dF = -\tfrac{1}{2}\sigma^2 F\,dt + \sigma F\,dW_t + \tfrac{1}{2}\sigma^2 F\,dt = \sigma F\,dW_t.
$$

The drift is zero, so $F$ is a (local) martingale. With suitable
integrability (true here, since $\sigma$ is constant), it is a true
martingale. $\blacksquare$

> **Why this matters.** The exponential martingale is the
> Radon–Nikodým derivative used in Girsanov's theorem to switch from the
> physical measure $\mathbb{P}$ to the risk‑neutral measure $\mathbb{Q}$.

---

## 7. The discounted asset price is a martingale under $\mathbb{Q}$

A canonical application: in the Black–Scholes setup, the *discounted*
stock price $\tilde S_t = S_t\,e^{-r t}$ is a martingale under the
risk‑neutral measure $\mathbb{Q}$.

### Setup

Under the physical measure $\mathbb{P}$, the stock follows
$dS_t = \mu S_t\,dt + \sigma S_t\,dW_t$. Under $\mathbb{Q}$ the drift is
replaced by the risk‑free rate:

$$
dS_t \;=\; r\,S_t\,dt + \sigma\,S_t\,dW_t^{\mathbb{Q}}.
$$

The closed‑form solution (Itô + log trick — see
[[07 - Solving SDEs with Itô's Lemma]]) is

$$
S_t \;=\; S_s\,\exp\!\Bigl\{\bigl(r - \tfrac{1}{2}\sigma^2\bigr)(t - s) + \sigma\bigl(W_t^{\mathbb{Q}} - W_s^{\mathbb{Q}}\bigr)\Bigr\}.
$$

### Showing $\mathbb{E}_{\mathbb{Q}}[\tilde S_t \mid \mathcal{F}_s] = \tilde S_s$

The discounted price is $\tilde S_t = e^{-r t} S_t$. Substitute:

$$
\tilde S_t = e^{-r t}\,S_s\,\exp\!\Bigl\{\bigl(r - \tfrac{1}{2}\sigma^2\bigr)(t - s) + \sigma(W_t^{\mathbb{Q}} - W_s^{\mathbb{Q}})\Bigr\}.
$$

Compute the conditional expectation. The factor $S_s = e^{r s}\tilde S_s$
is $\mathcal{F}_s$‑measurable and pulls out. The increment
$W_t^{\mathbb{Q}} - W_s^{\mathbb{Q}}$ is independent of $\mathcal{F}_s$.
Use the Gaussian MGF
$\mathbb{E}\!\left[e^{\sigma(W_t^{\mathbb{Q}} - W_s^{\mathbb{Q}})}\right] = e^{\tfrac{1}{2}\sigma^2(t - s)}$:

$$
\mathbb{E}_{\mathbb{Q}}\!\bigl[\tilde S_t \mid \mathcal{F}_s\bigr]
\;=\; \tilde S_s \cdot e^{-r(t-s)} \cdot e^{(r - \tfrac{1}{2}\sigma^2)(t-s)} \cdot e^{\tfrac{1}{2}\sigma^2(t-s)}
\;=\; \tilde S_s.
$$

All four exponentials collapse. $\blacksquare$

Equivalently, the **drift test**: apply Itô's lemma to
$\tilde S_t = e^{-r t} S_t$ under $\mathbb{Q}$. The deterministic factor
$e^{-r t}$ cancels the drift $r S_t$, leaving
$d\tilde S_t = \sigma\,\tilde S_t\,dW_t^{\mathbb{Q}}$ — drift zero, hence
martingale.

---

## Cross‑references

- [[02 - The Itô Integral]] — the Itô integral is the prototypical
  martingale.
- [[03 - Itô's Lemma - Statement and Derivation]] — used in Method (ii)
  of the exponential martingale proof.
- [[07 - Solving SDEs with Itô's Lemma]] — derives the GBM closed form
  used here.
- [[10 - Geometric Brownian Motion]] — pre‑measure‑change dynamics of $S_t$.
- [[../Derivatives/]] — risk‑neutral pricing applications.
