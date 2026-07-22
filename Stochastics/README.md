# Stochastic Calculus for Financial Engineering

> A structured set of notes covering stochastic processes, the Itô integral,
> Itô's lemma, diffusion models, martingales, and Geometric Brownian Motion —
> with a focus on the tools needed to price derivatives and model asset
> dynamics.

---

## How to read these notes

The chapters are numbered in the recommended study order. Each chapter is a
single self‑contained file:

| #  | Chapter                                                                                                                | Status        |
| -: | :--------------------------------------------------------------------------------------------------------------------- | :------------ |
| 00 | [[00 - Foundations Review]] — vocabulary, ODE vs SDE, taxonomy of processes                                            | Reference     |
| 01 | [[01 - Introduction to Stochastic Processes]] — definitions, Wiener process, SDE form                                  | Core          |
| 02 | [[02 - The Itô Integral]] — building the integral from simple processes; Itô isometry                                  | Core          |
| 03 | [[03 - Itô's Lemma - Statement and Derivation]] — the stochastic chain rule (heuristic)                                | Core          |
| 04 | [[04 - Proof of Itô's Lemma]] — three levels of rigour, from exam sketch to L² limit                                   | Reference     |
| 05 | [[05 - Diffusion Processes Catalogue]] — GBM, OU, Vasicek, CIR, Brownian bridge                                        | Core          |
| 06 | [[06 - Itô's Lemma Worked Examples]] — forward contract, inverse contract                                              | Core          |
| 07 | [[07 - Solving SDEs with Itô's Lemma]] — ∫W dW, GBM closed form, log transform                                         | Core          |
| 08 | [[08 - Itô Integral Exercises and Moments]] — computing E[W_t^n] via Itô + ODE trick                                   | Practice      |
| 09 | [[09 - Martingales]] — definition, properties, risk‑neutral measure                                                    | Core          |
| 10 | [[10 - Geometric Brownian Motion]] — derivation, log‑normal distribution                                               | Core          |
| 11 | [[11 - GBM Probability Calculations]] — hitting probabilities, confidence intervals                                    | Application   |
| 12 | [[12 - GBM Parameter Estimation]] — drift and volatility from log returns                                              | Application   |

### Part II — Risk-neutral pricing, Girsanov, and Black–Scholes

| #    | Chapter                                                                                                              | Status      |
| ---: | :----------------------------------------------------------------------------------------------------------------- | :---------- |
| 13   | [[13 - Probability Measures]] — real-world $\mathbb{P}$ vs risk-neutral $\mathbb{Q}$                                  | Core        |
| 14   | [[14 - Asset Dynamics under the probability measures]] — discounted assets as $\mathbb{Q}$-martingales                | Core        |
| 15   | [[15 - Lets understand the brownian motion under Q]] — $W_t^\mathbb{Q}$ vs $W_t^\mathbb{P}$                           | Core        |
| 16   | [[16 - Equivalent Probability Measures and Girsanov's Theorem]] — equivalence + Cameron–Martin–Girsanov               | Core        |
| 17   | [[17 - Example application of Girsanov's Theorem]] — worked change-of-measure proof                                   | Practice    |
| 18   | [[18 - Previsible Process and Martingale Representation Theorem]] — MRT and hedging strategies                        | Core        |
| 19   | [[19 - The self financing Portfolio]] — the self-financing condition under $\mathbb{P}$                               | Core        |
| 20   | [[20 - Proof of Replicating Portfolio]] — self-financing $\iff$ discounted value is a $\mathbb{Q}$-martingale         | Core        |
| 21   | [[21 - Martingale Pricing Of European Contingent Claims]] — the general risk-neutral pricing formula                 | Core        |
| 21.1 | [[21.1 - Less Technical Explanation of Martingale Pricing Of European Contingent Claims]] — intuition-first version   | Reference   |
| 22   | [[22 - General Formula]] — the 3-step pricing recipe; call/put/forward                                                | Core        |
| 22.1 | [[22.1 - Properties of Ito Integral And the O-U Process]] — Itô isometry and the O-U solution                        | Core        |
| 22.2 | [[22.2 - Helper Process]] — the integrating factor $U_t = X_t e^{\theta t}$                                           | Reference   |
| 22.3 | [[22.3 - Decoupling]] — breaking the $dX_t$–$X_t$ feedback loop                                                       | Reference   |
| 23   | [[23 - Statistical Distribution of the O-U Process]] — conditional mean and variance                                  | Core        |
| 24   | [[24 - Long-Term Statistical Distribution Of the O-U Process]] — the stationary distribution                          | Core        |
| 25   | [[25 - Vasicek Model]] — the O-U short-rate model and its closed form                                                 | Application |
| 26   | [[26 - Statistical Distribution of the Vasicek Model]] — conditional and long-run law                                 | Application |
| 27   | [[27 - Stochastic Models of Derivative Prices]] — BOPM vs BSM; assumptions                                            | Core        |
| 28   | [[28 - Derivation of the Black Scholes PDE]] — delta-hedging route to the PDE                                       | Core        |
| 29   | [[29 - Derivation of The Black Scholes merton formula]] — direct integration; worked example                        | Core        |
| 30   | [[30 - Put-Call Parity]] — the model-free parity identity                                                             | Core        |
| 31   | [[31 - Proof of Put-Call Parity]] — replication proof                                                                 | Practice    |
| 31.1 | [[31.1 - Law of One Price]] — the no-arbitrage foundation                                                             | Reference   |

---

## Quick reference

**Itô multiplication table (Box calculus):**

| ×       | $dt$ | $dW_t$ |
| :------ | :--: | :----: |
| $dt$    |  0   |   0    |
| $dW_t$  |  0   |  $dt$  |

**Itô's Lemma (one‑dimensional):** for $X_t$ following $dX_t = \mu\,dt + \sigma\,dW_t$
and $f \in C^{1,2}$,

$$
df(t, X_t) \;=\; \Bigl(\tfrac{\partial f}{\partial t} + \mu\,\tfrac{\partial f}{\partial x} + \tfrac{1}{2}\sigma^2\,\tfrac{\partial^2 f}{\partial x^2}\Bigr) dt + \sigma\,\tfrac{\partial f}{\partial x}\,dW_t.
$$

**Geometric Brownian Motion closed form:**

$$
S_t = S_0 \exp\left[ \left(\mu - \tfrac{1}{2}\sigma^2\right) t + \sigma W_t \right]
$$

$$
\ln(S_t / S_0) \sim \mathcal{N}\!\left( (\mu - \tfrac{1}{2}\sigma^2) t,\; \sigma^2 t \right)
$$

---

## Conventions

- $W_t$ and $B_t$ both denote a standard Brownian motion / Wiener process.
- $\mathcal{F}_t$ denotes the natural filtration of $W_t$ (the information
  available up to time $t$).
- All integrals against $dW_t$ are **Itô** (left‑endpoint, non‑anticipating),
  unless explicitly noted as Stratonovich.
- A function is in $C^{1,2}$ when it is once continuously differentiable in
  time and twice continuously differentiable in the space variable.

## Related folders

- [[../Derivatives/7 May 2026 - Personal Notes 1]] — derivatives definition; GBM as the underlying.
- [[../Derivatives/Lehman Brothers 2007]] — case study in OTC derivatives risk.
- [[../Fixed Income Securities/22 April 2026]] — bond valuation; the PV machinery that feeds into rate-tree models.
- [[../Fixed Income Securities/29 April online excercise]] — one-period interest-rate tree, the discrete analogue of Vasicek / CIR from [[05 - Diffusion Processes Catalogue]].
- [[../Financial Theory/21 April 2026 A]] — Black-Scholes presented as the statistical option-pricing model assuming GBM.
- [[../Time Series/10 April 2026]] — the discrete-time / stationarity vocabulary that overlaps [[00 - Foundations Review]].
- [[../Time Series/24 April Assignment]] — OLS regression methodology used in [[12 - GBM Parameter Estimation]].
- [[../QUANTFRAME/Concepts/BackGround to Kelly Criterion]] — log-wealth and the $\tfrac{1}{2}\sigma^2$ correction shared with GBM.
- [[../Introduction to stochastic calculus with applications/Preliminaries of calculus Rough Notes]] — continuity, càdlàg, total variation prerequisites.
