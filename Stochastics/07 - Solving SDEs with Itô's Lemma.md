# 07 — Solving SDEs with Itô's Lemma

> Itô's lemma is more than a chain rule — it is also the standard tool for
> turning an unsolvable SDE into one we *can* integrate. The trick: pick a
> clever function $f(t, X_t)$ so that $df$ has an explicit, integrable form.
> We use it here to evaluate $\int_0^t W_s\,dW_s$ in closed form and to
> derive the canonical Geometric Brownian Motion solution.

---

## 1. The strategy

Given an SDE $dX_t = \mu_t\,dt + \sigma_t\,dW_t$ that we cannot integrate
directly, we look for a function $f(t, X_t)$ such that

$$
df(t, X_t) \;=\; \underbrace{(\text{drift})\,dt}_{\text{easy to integrate}} \;+\; \underbrace{(\text{volatility})\,dW_t}_{\text{leaves an Itô integral, hopefully simple}}
$$

simplifies to something we can integrate term‑by‑term. We then **integrate
both sides from $0$ to $t$** and make the unknown the subject.

The recipe:

1. Choose the candidate function $f$ (the *Itô trick* — usually $\ln$, a
   power, or $\exp$).
2. Compute the three derivatives $\partial_t f$, $\partial_x f$,
   $\partial_x^2 f$.
3. Plug into Itô's lemma, simplify using the multiplication rules.
4. Integrate both sides from $0$ to $t$.
5. Use a known property (e.g. $W_0 = 0$) to evaluate boundary terms.
6. Rearrange to solve for the original integral or process.

---

## 2. Example 1 — Evaluating $\displaystyle\int_0^t W_s\,dW_s$

The integrand $W_s$ is itself random, so a naïve "$x\,dx$" treatment is
wrong. The Itô isometry tells us this integral has mean zero and variance
$\int_0^t s\,ds = t^2/2$, but Itô's lemma gives the **exact closed form**.

### Step 1 — choose the function

Pretend for a moment $W_s$ is a deterministic variable $x$, so we'd write
$\int x\,dx = x^2/2$. That suggests the candidate

$$
f(t, x) \;=\; \frac{x^2}{2}, \qquad \text{so that} \qquad f(t, W_t) \;=\; \frac{W_t^2}{2}.
$$

### Step 2 — partial derivatives

$$
\frac{\partial f}{\partial t} = 0, \qquad \frac{\partial f}{\partial x} = x, \qquad \frac{\partial^2 f}{\partial x^2} = 1.
$$

### Step 3 — apply Itô's lemma

$$
d\!\left(\frac{W_t^2}{2}\right) \;=\; 0\cdot dt + W_t\,dW_t + \tfrac{1}{2}\cdot 1 \cdot (dW_t)^2.
$$

Use $(dW_t)^2 = dt$:

$$
d\!\left(\frac{W_t^2}{2}\right) \;=\; W_t\,dW_t + \tfrac{1}{2}\,dt.
$$

### Step 4 — integrate both sides from $0$ to $t$

$$
\int_0^t d\!\left(\frac{W_s^2}{2}\right) \;=\; \int_0^t W_s\,dW_s + \int_0^t \tfrac{1}{2}\,ds.
$$

The left side telescopes; the right‑hand $ds$ integral is elementary.

$$
\left[\frac{W_s^2}{2}\right]_0^t \;=\; \int_0^t W_s\,dW_s + \tfrac{1}{2}\,t.
$$

### Step 5 — apply $W_0 = 0$ and rearrange

$$
\frac{W_t^2}{2} - 0 \;=\; \int_0^t W_s\,dW_s + \tfrac{1}{2}\,t.
$$

Hence

$$
\boxed{\;\int_0^t W_s\,dW_s \;=\; \tfrac{1}{2}\bigl(W_t^2 - t\bigr).\;}
$$

### Sanity check

- **Mean:** $\mathbb{E}\bigl[\tfrac{1}{2}(W_t^2 - t)\bigr] = \tfrac{1}{2}(t - t) = 0.$ ✓
- **Variance:** $\operatorname{Var}\!\bigl[\tfrac{1}{2}(W_t^2 - t)\bigr] = \tfrac{1}{4}\operatorname{Var}(W_t^2) = \tfrac{1}{4}\cdot 2t^2 = t^2/2$ ✓
  (matches the Itô isometry $\int_0^t s\,ds = t^2/2$).

Note the **$-t/2$ correction** compared with the naïve $W_t^2/2$. This is
the integrated cousin of Itô's correction term.

---

## 3. Example 2 — Closed‑form solution of GBM

**Problem.** Given the SDE of GBM,

$$
dS_t \;=\; \mu\,S_t\,dt + \sigma\,S_t\,dW_t,
$$

solve for $S_t$ explicitly in terms of $W_t$.

**Why not integrate directly?** The unknown $S_t$ appears on both sides
("the unknown process is on both sides of the equation" — the hallmark of a
GBM). Naïve integration is not possible. We use Itô's lemma to transform
the SDE into one we *can* integrate.

### Step 1 — choose the function

The right transformation is the natural logarithm:

$$
f(t, S_t) \;=\; \ln S_t.
$$

This is motivated by the observation that *multiplicative* dynamics become
*additive* under $\ln$.

### Step 2 — partial derivatives

$$
\frac{\partial f}{\partial t} = 0, \qquad \frac{\partial f}{\partial S_t} = \frac{1}{S_t}, \qquad \frac{\partial^2 f}{\partial S_t^2} = -\frac{1}{S_t^2}.
$$

### Step 3 — apply Itô's lemma

$$
df(t, S_t) \;=\; 0\cdot dt + \frac{1}{S_t}\,dS_t + \tfrac{1}{2}\!\left(-\frac{1}{S_t^2}\right)(dS_t)^2.
$$

Substitute $dS_t = \mu S_t\,dt + \sigma S_t\,dW_t$ and
$(dS_t)^2 = \sigma^2 S_t^2\,dt$:

$$
d(\ln S_t) \;=\; \frac{1}{S_t}\bigl(\mu S_t\,dt + \sigma S_t\,dW_t\bigr) - \frac{1}{2 S_t^2}\bigl(\sigma^2 S_t^2\,dt\bigr).
$$

The $S_t$'s cancel and we are left with a *deterministic* drift plus a
*constant‑coefficient* diffusion:

$$
\boxed{\;d(\ln S_t) \;=\; \bigl(\mu - \tfrac{1}{2}\sigma^2\bigr)\,dt + \sigma\,dW_t.\;}
$$

### Step 4 — integrate

$$
\int_0^t d(\ln S_s) \;=\; \int_0^t \bigl(\mu - \tfrac{1}{2}\sigma^2\bigr)\,ds + \int_0^t \sigma\,dW_s,
$$

$$
\ln S_t - \ln S_0 \;=\; \bigl(\mu - \tfrac{1}{2}\sigma^2\bigr)\,t + \sigma\,W_t.
$$

### Step 5 — rearrange

$$
\ln\!\left(\frac{S_t}{S_0}\right) \;=\; \bigl(\mu - \tfrac{1}{2}\sigma^2\bigr)\,t + \sigma\,W_t,
$$

$$
\boxed{\; S_t = S_0 \exp\left[ \left(\mu - \tfrac{1}{2}\sigma^2\right) t + \sigma W_t \right]. \;}
$$

### Why the $-\tfrac{1}{2}\sigma^2$?

The Itô correction. Volatility *reduces* the expected log return, even
though $\mathbb{E}[\sigma W_t] = 0$. This is the source of the famous
"volatility drag" in finance.

A full discussion of the resulting distribution, moments, and applications
lives in [[10 - Geometric Brownian Motion]].

---

## 4. Exercises (no solutions — use the same recipe)

Use Itô's lemma to find closed‑form expressions for the following Itô
integrals. The two‑step plan: (i) spot the "antiderivative" $f$ via the
ordinary‑calculus analogue; (ii) apply Itô's lemma, integrate, and
rearrange.

1. $\displaystyle\int_0^t W_s^2\,dW_s$ &nbsp; *(hint: try $f(x) = x^3/3$)*
2. $\displaystyle\int_0^t e^{k W_s}\,dW_s$ &nbsp; *(hint: try $f(x) = \tfrac{1}{k}e^{kx}$)*
3. $\displaystyle\int_0^t \cos W_s\,dW_s$ &nbsp; *(see worked example in [[08 - Itô Integral Exercises and Moments]])*
4. Show that

   $$
   \int_0^t \sin W_s\,dW_s \;=\; 1 - \cos W_t - \tfrac{1}{2}\int_0^t \cos W_s\,ds.
   $$

---

## Cross‑references

- [[02 - The Itô Integral]] — definition, mean, variance of $\int X\,dW$.
- [[03 - Itô's Lemma - Statement and Derivation]] — the lemma itself.
- [[06 - Itô's Lemma Worked Examples]] — the *forward* direction: take a
  known function of $S_t$ and find its SDE.
- [[08 - Itô Integral Exercises and Moments]] — using the same trick to
  compute moments $\mathbb{E}[W_t^n]$.
- [[10 - Geometric Brownian Motion]] — continues the GBM story with
  distributions and expectations.
