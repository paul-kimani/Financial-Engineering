# 10 — Geometric Brownian Motion

> The workhorse model for equity prices. This chapter pulls together the
> SDE, its closed‑form solution, the log‑normal distribution that falls out
> of it, and the moments $\mathbb{E}[S_t]$ and $\operatorname{Var}(S_t)$
> that everything else depends on.

---

## 1. Why GBM?

When modelling the price of financial assets such as stocks, we assume that
prices evolve continuously over time and are influenced by random factors.

**Geometric Brownian Motion** is the standard choice because:

- It is **mathematically tractable** — there is a closed‑form solution.
- It captures the **continuous stochastic nature** of price movements.
- It guarantees $S_t > 0$ (a log‑normal distribution).
- It is consistent with the empirical observation that *log returns* are
  roughly stationary.

Structurally, GBM is the prototype of an SDE whose **unknown process
appears on both sides** of the equation:

$$
(i)\quad dS_t = \mu\,S_t\,dt + \sigma\,S_t\,dW_t,
$$

$$
(ii)\quad dX_t = \mu\,X_t\,dt + \sigma\,X_t\,dW_t.
$$

In both cases the right‑hand side depends on the unknown ($S_t$ or $X_t$),
so naïve integration fails. We use Itô's lemma with the log transform to
solve.

---

## 2. Solving the GBM SDE

We outline the derivation; the full step‑by‑step lives in
[[07 - Solving SDEs with Itô's Lemma]].

Set $F(t, S_t) = \ln S_t$ and apply Itô's lemma:

$$
\frac{\partial F}{\partial t} = 0, \quad \frac{\partial F}{\partial S_t} = \frac{1}{S_t}, \quad \frac{\partial^2 F}{\partial S_t^2} = -\frac{1}{S_t^2}.
$$

Substituting and using $(dS_t)^2 = \sigma^2 S_t^2\,dt$:

$$
d(\ln S_t) \;=\; \bigl(\mu - \tfrac{1}{2}\sigma^2\bigr)\,dt + \sigma\,dW_t.
$$

Integrate from $0$ to $t$ and exponentiate:

$$
\ln S_t - \ln S_0 \;=\; \bigl(\mu - \tfrac{1}{2}\sigma^2\bigr) t + \sigma W_t,
$$

$$
\boxed{\;S_t \;=\; S_0\,\exp\!\left\{\bigl(\mu - \tfrac{1}{2}\sigma^2\bigr) t + \sigma W_t\right\}.\;}
$$

The constant $-\tfrac{1}{2}\sigma^2$ is the Itô correction — a *real*,
non‑negligible drag on expected log returns produced by volatility.

---

## 3. Distribution of $\ln(S_t / S_0)$

Because $W_t \sim \mathcal{N}(0, t)$ is Gaussian and the right‑hand side is
linear in $W_t$,

$$
\ln\!\left(\frac{S_t}{S_0}\right) \;=\; \bigl(\mu - \tfrac{1}{2}\sigma^2\bigr) t + \sigma W_t
$$

is itself Gaussian:

$$
\boxed{\;\ln\!\left(\frac{S_t}{S_0}\right) \sim \mathcal{N}\!\left(\bigl(\mu - \tfrac{1}{2}\sigma^2\bigr) t,\; \sigma^2 t\right).\;}
$$

Equivalently, $S_t$ is **log‑normal** with parameters

$$
\ln S_t \sim \mathcal{N}\!\left(\ln S_0 + \bigl(\mu - \tfrac{1}{2}\sigma^2\bigr) t,\; \sigma^2 t\right).
$$

The **mean** of the log return is $(\mu - \tfrac{1}{2}\sigma^2) t$ and the
**variance** is $\sigma^2 t$.

---

## 4. Expected value and variance of $S_t$

Using the log‑normal moment formulas — if $Y \sim \mathcal{N}(m, v)$ then
$\mathbb{E}[e^Y] = e^{m + v/2}$ and
$\operatorname{Var}(e^Y) = e^{2 m + v}\bigl(e^v - 1\bigr)$.

Apply with $m = \ln S_0 + (\mu - \tfrac{1}{2}\sigma^2) t$ and $v = \sigma^2 t$:

$$
\boxed{\;\mathbb{E}[S_t] \;=\; S_0\,e^{\mu t},\quad \operatorname{Var}(S_t) \;=\; S_0^2\,e^{2\mu t}\bigl(e^{\sigma^2 t} - 1\bigr).\;}
$$

> **Sanity check.** The drift $\mu$ governs the *expected* growth rate of
> the price; volatility *only* affects the variance, not the mean. This is
> why financial planners quote $\mu$ as the "expected return."

---

## 5. Worked example

**Setup.** The share price of the X9Z company is currently trading at
$\text{KSh } 1.75$ and can be modelled by

$$
dS_t \;=\; 0.4\,S_t\,dt + 0.5\,S_t\,dW_t.
$$

So $\mu = 0.4$, $\sigma = 0.5$, $S_0 = 1.75$. Compute the expectation and
variance of the X9Z share price in $T = 2$ years.

**Expectation.**

$$
\mathbb{E}[S_T] \;=\; S_0\,e^{\mu T} \;=\; 1.75 \cdot e^{0.4 \cdot 2} \;=\; 1.75 \cdot e^{0.8} \;\approx\; 1.75 \cdot 2.2255 \;\approx\; \mathbf{3.8947}.
$$

> The notebook reads "$8.8947$"; this is a transcription error — recompute
> with the correct $S_0$. The formula gives $\approx 3.89$ for $S_0 = 1.75$.
> If $S_0 = 4$ were intended, $4\cdot e^{0.8} \approx 8.8947$ matches.

**Variance.**

$$
\operatorname{Var}(S_T) \;=\; S_0^2\,e^{2 \mu T}\bigl(e^{\sigma^2 T} - 1\bigr) \;=\; (1.75)^2 \cdot e^{1.6} \cdot \bigl(e^{0.5} - 1\bigr).
$$

Numerically,

$$
= 3.0625 \cdot 4.9530 \cdot 0.6487 \;\approx\; \mathbf{9.840}.
$$

So $\operatorname{Var}(S_T) \approx 9.84$ — consistent with the notebook
value.

---

## 6. Statistical distribution of GBM — summary

Putting all the pieces together:

| Quantity                    | Formula                                                                     |
| :-------------------------- | :-------------------------------------------------------------------------- |
| **SDE**                     | $dS_t = \mu S_t\,dt + \sigma S_t\,dW_t$                                     |
| **Closed form**             | $S_t = S_0\,\exp\!\bigl((\mu - \tfrac{1}{2}\sigma^2) t + \sigma W_t\bigr)$  |
| **Log return distribution** | $\ln(S_t / S_0) \sim \mathcal{N}\bigl((\mu - \tfrac{1}{2}\sigma^2) t,\; \sigma^2 t\bigr)$ |
| **Price distribution**      | $S_t$ log‑normal                                                            |
| **$\mathbb{E}[S_t]$**       | $S_0\,e^{\mu t}$                                                            |
| **$\operatorname{Var}(S_t)$** | $S_0^2\,e^{2\mu t}\bigl(e^{\sigma^2 t} - 1\bigr)$                           |
| **Median**                  | $S_0\,e^{(\mu - \tfrac{1}{2}\sigma^2) t}$                                   |

---

## Cross‑references

- [[05 - Diffusion Processes Catalogue]] — GBM in the context of other
  diffusion processes.
- [[07 - Solving SDEs with Itô's Lemma]] — full derivation of the closed
  form.
- [[11 - GBM Probability Calculations]] — using the log‑normal distribution
  to answer "what is the probability the share exceeds $X$?" and to build
  confidence intervals.
- [[12 - GBM Parameter Estimation]] — estimating $\mu$ and $\sigma$ from
  historical prices.
- [[09 - Martingales]] — switching to the risk‑neutral measure to obtain
  the discounted‑price martingale.
