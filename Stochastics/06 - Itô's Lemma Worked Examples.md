# 06 — Itô's Lemma: Worked Examples

> Two canonical applications of [[03 - Itô's Lemma - Statement and Derivation|Itô's lemma]]:
> deriving the SDE of a **forward contract** and of an **inverse payoff**
> when the underlying follows GBM. Each example follows the same four‑step
> recipe.

---

## 1. Setup — the recipe

The share price follows a Geometric Brownian Motion:

$$
dS_t \;=\; \mu\,S_t\,dt + \sigma\,S_t\,dW_t.
$$

To apply Itô's lemma to a function $f(t, S_t)$, the general formula is

$$
df(t, S_t) \;=\; \frac{\partial f}{\partial t}\,dt + \frac{\partial f}{\partial S_t}\,dS_t + \tfrac{1}{2}\frac{\partial^2 f}{\partial S_t^2}\,(dS_t)^2.
$$

### Multiplication rules (box calculus)

- $(dt)^2 = 0$
- $dW_t \cdot dt = 0$
- $(dW_t)^2 = dt$

Expanding $(dS_t)^2 = (\mu S_t\,dt + \sigma S_t\,dW_t)^2$, the $(dt)^2$ and
$dt\,dW_t$ terms vanish, leaving the key identity

$$
\boxed{\;(dS_t)^2 \;=\; \sigma^2\,S_t^2\,dt.\;}
$$

### The four‑step recipe

1. Compute the partial derivatives of $f$.
2. Substitute them into Itô's lemma.
3. Substitute the GBM for $dS_t$ and use $(dS_t)^2 = \sigma^2 S_t^2\,dt$.
4. Group $dt$ and $dW_t$ terms; recognise the original function on the
   right.

---

## 2. Example 1 — SDE of a forward contract

**Problem.** Find the SDE of a forward contract whose value at time $t$ is

$$
F_t \;=\; S_t\,e^{r(T - t)}.
$$

### Step 1 — partial derivatives

- $\dfrac{\partial f}{\partial t} = S_t \cdot e^{r(T-t)} \cdot (-r) = -r\,S_t\,e^{r(T-t)}.$
- $\dfrac{\partial f}{\partial S_t} = e^{r(T-t)}.$
- $\dfrac{\partial^2 f}{\partial S_t^2} = 0.$

### Step 2 — substitute into Itô's lemma

$$
df(t, S_t) \;=\; -r\,S_t\,e^{r(T-t)}\,dt + e^{r(T-t)}\,dS_t + 0.
$$

### Step 3 — substitute the GBM

Plug $dS_t = \mu S_t\,dt + \sigma S_t\,dW_t$:

$$
df(t, S_t) = -r S_t\, e^{r(T-t)}\, dt + e^{r(T-t)} \left( \mu S_t\, dt + \sigma S_t\, dW_t \right).
$$

Expanding,

$$
df(t, S_t) \;=\; -r\,S_t\,e^{r(T-t)}\,dt + \mu\,S_t\,e^{r(T-t)}\,dt + \sigma\,S_t\,e^{r(T-t)}\,dW_t.
$$

### Step 4 — group terms and recognise $F_t$

Recall $F_t = S_t\,e^{r(T-t)}$. Substituting back,

$$
df(t, S_t) \;=\; -r F_t\,dt + \mu F_t\,dt + \sigma F_t\,dW_t,
$$

so

$$
\boxed{\;dF_t \;=\; (\mu - r)\,F_t\,dt + \sigma\,F_t\,dW_t.\;}
$$

**Interpretation.** The forward inherits the *same volatility* as the
underlying ($\sigma$), but its drift is reduced by the risk‑free rate $r$.
Under the risk‑neutral measure ($\mu = r$) the drift vanishes, making the
discounted forward a martingale — exactly the no‑arbitrage condition.

---

## 3. Example 2 — SDE of an inverse contract

**Problem.** Given that the share follows GBM, find the SDE of the payoff

$$
Y_t \;=\; \frac{e^{r(T - t)}}{S_t}.
$$

### Step 1 — partial derivatives

Treating the function as $e^{r(T-t)}\,S_t^{-1}$:

- $\dfrac{\partial Y}{\partial t} = \dfrac{1}{S_t}\cdot\bigl(-r\,e^{r(T-t)}\bigr) = -r\,\dfrac{e^{r(T-t)}}{S_t}.$
- $\dfrac{\partial Y}{\partial S_t} = e^{r(T-t)}\cdot(-1)\,S_t^{-2} = -\dfrac{e^{r(T-t)}}{S_t^2}.$
- $\dfrac{\partial^2 Y}{\partial S_t^2} = -e^{r(T-t)}\cdot(-2)\,S_t^{-3} = \dfrac{2\,e^{r(T-t)}}{S_t^3}.$

### Step 2 — substitute into Itô's lemma

$$
dY(t, S_t) \;=\; -r\,\frac{e^{r(T-t)}}{S_t}\,dt - \frac{e^{r(T-t)}}{S_t^2}\,dS_t + \tfrac{1}{2}\,\frac{2\,e^{r(T-t)}}{S_t^3}\,(dS_t)^2.
$$

### Step 3 — expand using the multiplication rules

Use $(dS_t)^2 = \sigma^2\,S_t^2\,dt$ and $dS_t = \mu S_t\,dt + \sigma S_t\,dW_t$:

$$
dY(t, S_t) = -r\, \frac{e^{r(T-t)}}{S_t}\, dt \;-\; \frac{e^{r(T-t)}}{S_t^2} \left( \mu S_t\, dt + \sigma S_t\, dW_t \right) \;+\; \frac{e^{r(T-t)}}{S_t^3} \left( \sigma^2 S_t^2\, dt \right).
$$

### Step 4 — simplify and group

Recognise $Y_t = e^{r(T-t)}/S_t$ throughout:

$$
dY(t, S_t) \;=\; -r\,Y_t\,dt - \mu\,Y_t\,dt - \sigma\,Y_t\,dW_t + \sigma^2\,Y_t\,dt.
$$

Grouping the $dt$ coefficient:

$$
\boxed{\;dY(t, S_t) \;=\; (\sigma^2 - r - \mu)\,Y_t\,dt - \sigma\,Y_t\,dW_t.\;}
$$

**Interpretation.** The inverse contract has volatility of the *same
magnitude* ($\sigma$) but **opposite sign** — when $S_t$ goes up, $Y_t$
goes down. The drift picks up an extra $+\sigma^2$ Itô‑correction term,
reflecting the convexity of $1/x$.

---

## 4. Key terminology (compact glossary)

- **Stochastic differential equation (SDE):** an equation in which one or
  more terms is a stochastic process; the solution is itself a stochastic
  process.
- **Wiener process / Brownian motion ($W_t$):** continuous‑time random
  process with independent, normally distributed increments. The canonical
  model for market noise.
- **Geometric Brownian Motion (GBM):** the continuous‑time process whose
  logarithm follows a Brownian motion with drift. Standard model for asset
  prices because it remains positive.
- **Drift ($\mu$):** deterministic, predictable trend or average rate of
  growth.
- **Volatility ($\sigma$):** stochastic component representing the amplitude
  of random fluctuations.
- **Itô's lemma:** the stochastic counterpart of the chain rule, used to
  find the differential of a time‑dependent function of a stochastic
  process. See [[03 - Itô's Lemma - Statement and Derivation]].

---

## Cross‑references

- [[03 - Itô's Lemma - Statement and Derivation]] — the lemma itself.
- [[05 - Diffusion Processes Catalogue]] — the GBM that drives both
  examples.
- [[07 - Solving SDEs with Itô's Lemma]] — the *inverse* application:
  using Itô's lemma to find closed‑form solutions to SDEs.
- [[10 - Geometric Brownian Motion]] — properties and full derivation of
  GBM.
