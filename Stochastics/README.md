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

$$S_t \;=\; S_0 \exp\!\Bigl\{\bigl(\mu - \tfrac{1}{2}\sigma^2\bigr)t + \sigma W_t\Bigr\},\qquad \ln\!\bigl(S_t/S_0\bigr) \sim \mathcal{N}\!\bigl((\mu-\tfrac{1}{2}\sigma^2)t,\;\sigma^2 t\bigr).$$

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

- [[../Derivatives/]] — applications to options pricing.
- [[../Fixed Income Securities/]] — interest‑rate models built on these SDEs.
- [[../Introduction to stochastic calculus with applications/]] — textbook rough notes.
