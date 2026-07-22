# 28 — Derivation of the Black-Scholes PDE

> The delta-hedging route to Black–Scholes. Apply Itô's lemma to $V(S_t,t)$,
> then hold $-\Delta = -\tfrac{\partial V}{\partial S}$ shares against the option:
> the $\mu$ and $dW_t$ terms cancel, leaving a **risk-free** portfolio. Forcing
> it to earn $r$ (no-arbitrage) gives the
> $\tfrac{\partial V}{\partial t} + rS\tfrac{\partial V}{\partial S} + \tfrac{1}{2}\sigma^2 S^2 \tfrac{\partial^2 V}{\partial S^2} - rV = 0$ PDE.

---

## 1. Derivation of the Black-Scholes PDE

### Step 1: Model Setup

Under the physical probability measure $\mathbb{P}$, the stock price process $S_t$ follows a Geometric Brownian Motion (GBM):

$$dS_t = \mu S_t dt + \sigma S_t dW_t$$

Where:

- $\mu$ is the expected rate of return (drift).

- $\sigma$ is the volatility.

- $W_t$ is a standard Brownian motion under $\mathbb{P}$.

Let $V(S_t, t)$ be the value of a European option expiring at time $T$.

### Step 2: Applying Itô's Lemma

Using Itô's Lemma, the differential $dV(S_t, t)$ is given by:

$$dV = \frac{\partial V}{\partial t} dt + \frac{\partial V}{\partial S} dS_t + \frac{1}{2} \frac{\partial^2 V}{\partial S^2} (dS_t)^2$$

Using the multiplication rules $(dt)^2 = 0$, $dt \cdot dW_t = 0$, and $(dW_t)^2 = dt$:

$$(dS_t)^2 = (\mu S_t dt + \sigma S_t dW_t)^2 = \sigma^2 S_t^2 dt$$

Substituting $dS_t$ and $(dS_t)^2$ into the expansion yields:

$$dV = \left( \frac{\partial V}{\partial t} + \mu S_t \frac{\partial V}{\partial S} + \frac{1}{2} \sigma^2 S_t^2 \frac{\partial^2 V}{\partial S^2} \right) dt + \sigma S_t \frac{\partial V}{\partial S} dW_t$$

### Step 3: Constructing a Delta-Hedged Portfolio

To eliminate risk (the $dW_t$ term), construct a portfolio $\Pi$ consisting of:

- $+1$ option (value $V$)

- $-\Delta$ shares of stock (where $\Delta = \frac{\partial V}{\partial S}$)

$$\Pi = V - \Delta S$$

The change in portfolio value over an infinitesimal time step $dt$ is:

$$d\Pi = dV - \Delta dS$$

Substituting $dV$ and $dS$:

$$d\Pi = \left( \frac{\partial V}{\partial t} + \mu S_t \frac{\partial V}{\partial S} + \frac{1}{2} \sigma^2 S_t^2 \frac{\partial^2 V}{\partial S^2} \right) dt + \sigma S_t \frac{\partial V}{\partial S} dW_t - \frac{\partial V}{\partial S} \left( \mu S_t dt + \sigma S_t dW_t \right)$$

Notice that the $\mu$ and $dW_t$ terms cancel out completely:

$$d\Pi = \left( \frac{\partial V}{\partial t} + \frac{1}{2} \sigma^2 S_t^2 \frac{\partial^2 V}{\partial S^2} \right) dt$$

### Step 4: Applying No-Arbitrage

Because $d\Pi$ is now completely deterministic (risk-free), it must earn the continuous risk-free rate $r$:

$$d\Pi = r \Pi dt = r \left( V - \frac{\partial V}{\partial S} S \right) dt$$

Equating the two expressions for $d\Pi$:

$$\left( \frac{\partial V}{\partial t} + \frac{1}{2} \sigma^2 S^2 \frac{\partial^2 V}{\partial S^2} \right) dt = r \left( V - S \frac{\partial V}{\partial S} \right) dt$$

Canceling $dt$ and rearranging terms gives the **Black-Scholes PDE**:

$$\frac{\partial V}{\partial t} + r S \frac{\partial V}{\partial S} + \frac{1}{2} \sigma^2 S^2 \frac{\partial^2 V}{\partial S^2} - rV = 0$$

---

## 2. Risk-Neutral Valuation & Black-Scholes Formulae

By solving this PDE subject to terminal payoff conditions $C(S, T) = \max(S_T - K, 0)$ for a call option, or using risk-neutral valuation under measure $\mathbb{Q}$:

$$C_t = e^{-r(T-t)} \mathbb{E}^\mathbb{Q} [\max(S_T - K, 0) \mid \mathcal{F}_t]$$

The solution yields the standard Black-Scholes pricing equations:

### European Call Price

$$C_t = S_t N(d_1) - K e^{-r(T-t)} N(d_2)$$

### European Put Price

$$P_t = K e^{-r(T-t)} N(-d_2) - S_t N(-d_1)$$

Where:

$$d_1 = \frac{\ln\left(\frac{S_t}{K}\right) + \left(r + \frac{\sigma^2}{2}\right)(T-t)}{\sigma \sqrt{T-t}}$$

$$d_2 = d_1 - \sigma \sqrt{T-t} = \frac{\ln\left(\frac{S_t}{K}\right) + \left(r - \frac{\sigma^2}{2}\right)(T-t)}{\sigma \sqrt{T-t}}$$

---

## 3. Financial Interpretation of Terms

- **$N(d_2)$:** The risk-neutral probability that the option finishes in-the-money ($S_T > K$).

- **$K e^{-r(T-t)} N(d_2)$:** The present value of paying the strike price $K$ conditional on exercise.

- **$N(d_1)$:** Delta ($\Delta = \frac{\partial C}{\partial S}$), representing the exact number of stock shares needed to hedge one long call option.

- **$S_t N(d_1)$:** The expected present value of receiving the stock at maturity $T$, scaled by the hedge ratio.

---

**See also:** [[27 Stochastic Models of Derivative Prices]] · [[29 Derivation of The Black Scholes merton formula.]] · [[19 The self financing Portfolio]]
