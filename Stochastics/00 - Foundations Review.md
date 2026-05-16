# 00 — Foundations Review

> A consolidated reference of the vocabulary and structural results used
> throughout the rest of the notes. Skim this first; return to it whenever a
> term feels unfamiliar.

---

## 1. Basic concepts on stochastic processes

A **stochastic process** is a collection of random variables
$\{X_t : t \in T\}$ indexed by time $t$, describing the evolution of a system
under inherent randomness.

**Foundational concepts**

- **State space:** the set of values $X_t$ can take (e.g. stock prices live
  in $\mathbb{R}^+$; a coin flip lives in $\{H, T\}$).
- **Index set (time):**
    - *Discrete‑time:* $T = \{0, 1, 2, \dots\}$ (e.g. daily closes).
    - *Continuous‑time:* $T = [0, \infty)$ (e.g. tick‑by‑tick prices).
- **Sample path / trajectory:** the realised sequence of values for one
  outcome $\omega$: $t \mapsto X_t(\omega)$.
- **Filtration $(\mathcal{F}_t)$:** the *information set* available up to time
  $t$. Critical for finance — it formalises the "no peeking into the future"
  rule. A process is **adapted** if $X_t$ is $\mathcal{F}_t$‑measurable.

---

## 2. Ordinary vs. stochastic calculus / ODE vs. SDE

This is the single most important distinction in quantitative finance.

| Feature           | **Ordinary Calculus (ODE)**            | **Stochastic Calculus (SDE)**                                                       |
| :---------------- | :------------------------------------- | :---------------------------------------------------------------------------------- |
| **Equation**      | $\dfrac{dX}{dt} = a(X, t)$             | $dX_t = \mu(X_t, t)\,dt + \sigma(X_t, t)\,dW_t$                                     |
| **Driving noise** | none (smooth function of time)         | white noise / Brownian motion $W_t$                                                 |
| **Smoothness**    | solutions differentiable everywhere    | solutions **continuous but nowhere differentiable** (fractal‑like, jagged)          |
| **Chain rule**    | standard: $df(X) = f'(X)\,dX$          | **Itô's lemma:** $df(X) = f'(X)\,dX + \tfrac{1}{2}f''(X)\,(dX)^2$                   |
| **Prediction**    | exact deterministic forecast           | probability distribution forecast                                                   |

**Example visualisation**

- ODE: $\dfrac{dP}{dt} = rP \implies P_t = P_0 e^{rt}$ (smooth exponential curve).
- SDE: $dS_t = \mu S_t\,dt + \sigma S_t\,dW_t$ (jagged, volatile path with an
  exponential trend).

---

## 3. The Itô integral (heuristic)

The integral $\int f(t)\,dW_t$ cannot be defined via classical
Riemann–Stieltjes methods because Brownian motion has **infinite total
variation** on any interval. The Itô integral is defined instead as the
mean‑square limit

$$
\int_0^T f(t,\omega)\,dW_t \;=\; \lim_{n \to \infty} \sum_{i=0}^{n-1} f(t_i,\omega)\bigl(W_{t_{i+1}} - W_{t_i}\bigr).
$$

**Crucial property — non‑anticipating (adapted):**
The integrand is evaluated at the **left endpoint** $t_i$.

- **Financial interpretation:** you decide how many shares to hold *before*
  observing the next price move. No clairvoyance.
- **Consequence:** the Itô integral is a **martingale**. Using the midpoint
  (the Stratonovich integral) destroys the martingale property and breaks
  no‑arbitrage pricing.

A full derivation lives in [[02 - The Itô Integral]].

---

## 4. Taxonomy of processes

### A. Discrete‑time processes

1. **Random walk:** $X_n = X_{n-1} + \epsilon_n$ with i.i.d. noise $\epsilon$.
   *Variance grows linearly with $n$ — non‑stationary.*
2. **AR(1) process:** $X_n = \phi X_{n-1} + \epsilon_n$.
   *If $|\phi| < 1$, the process is **mean reverting**.*

### B. Continuous‑time processes

1. **Brownian motion $W_t$** — the building block.
    1. $W_0 = 0$.
    2. Independent increments: $W_{t+s} - W_t \perp \mathcal{F}_t$.
    3. Gaussian increments: $W_{t+s} - W_t \sim \mathcal{N}(0, s)$.
    4. Continuous paths, nowhere differentiable.
2. **Poisson process $N_t$** — counts jumps; jumps of $+1$ at exponentially
   spaced random times. Used for defaults and trade arrivals.

### C. Key properties (vocabulary)

- **Stationarity:** statistical properties (mean, variance) do not change
  with time. *Returns* may be stationary; *prices* generally are not.
- **Martingale:** $\mathbb{E}[X_{t+s}\mid\mathcal{F}_t] = X_t$. A "fair game."
  See [[09 - Martingales]].
- **Markov property:** the future depends only on the *present* state, not
  the full history. *Geometric Brownian Motion is Markov.*

---

## 5. Diffusion processes and financial examples

A **diffusion process** is a continuous‑time Markov process with continuous
sample paths — the solution to an SDE of the form

$$
dX_t = \mu(t, X_t)\,dt + \sigma(t, X_t)\,dW_t,
$$

where $\mu$ is the **drift** (deterministic trend) and $\sigma$ is the
**diffusion coefficient** (volatility).

| Process                              | SDE                                              | Financial application                                       | Key characteristic                                     |
| :----------------------------------- | :----------------------------------------------- | :---------------------------------------------------------- | :----------------------------------------------------- |
| **Arithmetic Brownian Motion**       | $dX = \mu\,dt + \sigma\,dW$                      | spread between two stocks; particle position                | can go **negative** — bad for stock prices             |
| **Geometric Brownian Motion (GBM)**  | $dS = \mu S\,dt + \sigma S\,dW$                  | Black–Scholes stock prices                                  | log‑normal; **stays positive**                         |
| **Ornstein–Uhlenbeck (OU)**          | $dX = \theta(\mu - X)\,dt + \sigma\,dW$          | Vasicek interest rates; commodity prices                    | **mean‑reverting** to $\mu$ at speed $\theta$          |
| **Cox–Ingersoll–Ross (CIR)**         | $dX = \theta(\mu - X)\,dt + \sigma\sqrt{X}\,dW$  | non‑negative interest rates; Heston stochastic volatility   | volatility scales with level (chi‑squared distribution)|
| **Brownian bridge**                  | $dX = \dfrac{b - X}{T - t}\,dt + dW$             | bond prices pinned to par at maturity                       | **pinned** to a future value at time $T$               |

Full derivations and worked examples are in [[05 - Diffusion Processes Catalogue]].

---

## Cross‑references

- [[01 - Introduction to Stochastic Processes]] — formal definitions and the
  general SDE.
- [[02 - The Itô Integral]] — rigorous construction of $\int X_s\,dW_s$.
- [[03 - Itô's Lemma - Statement and Derivation]] — the stochastic chain rule.
- [[09 - Martingales]] — the martingale property and risk‑neutral pricing.
