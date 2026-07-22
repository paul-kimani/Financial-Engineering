# 20 — Proof of Replicating Portfolio

> The two-way equivalence at the heart of arbitrage-free pricing: a portfolio is
> **self-financing** *if and only if* its discounted value $\tilde{V}_t = V_t e^{-rt}$
> is a **$\mathbb{Q}$-martingale**. The forward direction shows the drift terms
> cancel, leaving $d\tilde{V}_t = \phi_t e^{-rt}\sigma S_t\,dW_t^\mathbb{Q}$; the
> reverse recovers the self-financing condition from that martingale form.

---

Here we lay out an elegant mathematical proof. This proof demonstrates a two-way equivalence: if a portfolio is **self-financing**, its discounted value is a **martingale** under the risk-neutral measure $\mathbb{Q}$; conversely, if the discounted portfolio value acts as a martingale with that specific volatility structure, the portfolio must be **self-financing**.

---

## 1. Defining the Framework and Applying Itô's Lemma

We set up the mathematical definitions for your portfolio and sets the stage for Itô's Lemma.

### The Portfolio Definitions

We define the nominal value of our trading portfolio $V_t$ containing $\phi_t$ shares of stock and $\psi_t$ units of a risk-free bond:

$$V_t = \phi_t S_t + \psi_t B_t$$

We then define the **discounted portfolio value** $\tilde{V}_t$ by discounting $V_t$ at the continuous risk-free rate $r$:

$$\tilde{V}_t = V_t e^{-rt}$$

### Setting Up Itô's Lemma

To find the stochastic differential of the discounted portfolio, $d\tilde{V}_t$, we define a function $f(V_t, t) = V_t e^{-rt}$. Applying Itô's Lemma:

$$df(V_t, t) = \frac{\partial f}{\partial t} dt + \frac{\partial f}{\partial V_t} dV_t + \frac{1}{2}\frac{\partial^2 f}{\partial V_t^2} (dV_t)^2$$

---

## 2. Deriving the Differential & Risk-Neutral Dynamics

This page calculates the partial derivatives of $f(V_t, t)$ and introduces the asset dynamics under the risk-neutral measure $\mathbb{Q}$.

### Evaluating the Itô Expansion

Calculating the partial derivatives of $f(V_t, t) = V_t e^{-rt}$:

- $\frac{\partial f}{\partial t} = -r V_t e^{-rt}$

- $\frac{\partial f}{\partial V_t} = e^{-rt}$

- $\frac{\partial^2 f}{\partial V_t^2} = 0$ (since $f$ is linear with respect to $V_t$)

Substituting these back in yields the general differential equation for the discounted portfolio:

$$d\tilde{V}_t = -r V_t e^{-rt} dt + e^{-rt} dV_t$$

### Under the Risk-Neutral Measure $\mathbb{Q}$

Under $\mathbb{Q}$, we define the SDEs of our underlying stock ($S_t$) and risk-free bond ($B_t$):

- **Stock Price:** $dS_t = r S_t dt + \sigma S_t dW_t^\mathbb{Q}$

- **Bond/Cash:** $dB_t = r B_t dt$

Assuming the portfolio is **self-financing**, its change in value $dV_t$ is purely driven by asset price changes:

$$dV_t = \phi_t dS_t + \psi_t dB_t$$

Substituting the stock and bond SDEs into this equation gives:

$$dV_t = \phi_t \left( r S_t dt + \sigma S_t dW_t^\mathbb{Q} \right) + \psi_t \left( r B_t dt \right)$$

---

## 3. Proving the Martingale Property (Forward Direction)

This completes the first half of the proof: showing that a self-financing portfolio results in a discounted value $\tilde{V}_t$ that is a martingale.

### Expanding and Simplifying $d\tilde{V}_t$

First, we group the $dt$ and $dW_t^\mathbb{Q}$ terms for the undiscounted portfolio variation $dV_t$:

$$dV_t = \left( \phi_t r S_t + \psi_t r B_t \right) dt + \phi_t \sigma S_t dW_t^\mathbb{Q}$$

Now, we substitute this $dV_t$ expression back into our general formula for $d\tilde{V}_t$:

$$d\tilde{V}_t = -r V_t e^{-rt} dt + e^{-rt} \left[ \left( \phi_t r S_t + \psi_t r B_t \right) dt + \phi_t \sigma S_t dW_t^\mathbb{Q} \right]$$

Distribute the $e^{-rt}$ factor:

$$d\tilde{V}_t = -r V_t e^{-rt} dt + r e^{-rt} \left( \phi_t S_t + \psi_t B_t \right) dt + \phi_t e^{-rt} \sigma S_t dW_t^\mathbb{Q}$$

### The Great Cancellation

Because the static value of the portfolio is defined as $V_t = \phi_t S_t + \psi_t B_t$, we can substitute this back in:

$$d\tilde{V}_t = \underbrace{-r e^{-rt} \left( \phi_t S_t + \psi_t B_t \right) dt + r e^{-rt} \left( \phi_t S_t + \psi_t B_t \right) dt}_{0} + \phi_t e^{-rt} \sigma S_t dW_t^\mathbb{Q}$$

The drift terms cancel each other out completely, leaving:

$$d\tilde{V}_t = \phi_t e^{-rt} \sigma S_t dW_t^\mathbb{Q}$$

Because this SDE contains **no $dt$ (drift) term**, the expected change in the discounted portfolio value is zero. This proves that **$\tilde{V}_t$ is a martingale under $\mathbb{Q}$**.

---

## 4. Proving the Reverse Direction (Equivalency)

The final page proves the reverse logic: if the discounted portfolio evolves as a martingale according to $d\tilde{V}_t = \phi_t e^{-rt} \sigma S_t dW_t^\mathbb{Q}$, then the portfolio must satisfy the self-financing condition.

### Equating both SDE Expressions

We set the two definitions of $d\tilde{V}_t$ equal to each other:

- **Expression A (from Itô's Lemma):** $d\tilde{V}_t = e^{-rt} dV_t - r V_t e^{-rt} dt$

- **Expression B (from Martingale Representation):** $d\tilde{V}_t = \phi_t e^{-rt} \sigma S_t dW_t^\mathbb{Q}$

$$e^{-rt} dV_t - r V_t e^{-rt} dt = \phi_t e^{-rt} \sigma S_t dW_t^\mathbb{Q}$$

### Backout the Undiscounted $dV_t$

To isolate $dV_t$, we multiply the entire equation by $e^{rt}$:

$$dV_t - r V_t dt = \phi_t \sigma S_t dW_t^\mathbb{Q}$$

$$dV_t = r V_t dt + \phi_t \sigma S_t dW_t^\mathbb{Q}$$

Now substitute the portfolio definition $V_t = \phi_t S_t + \psi_t B_t$ back into the drift:

$$dV_t = r \left( \phi_t S_t + \psi_t B_t \right) dt + \phi_t \sigma S_t dW_t^\mathbb{Q}$$

Rearrange and group the terms by their trading strategy allocations ($\phi_t$ and $\psi_t$):

$$dV_t = \phi_t \left( r S_t dt + \sigma S_t dW_t^\mathbb{Q} \right) + \psi_t \left( r B_t dt \right)$$

Since the terms in parentheses are the differentials $dS_t$ and $dB_t$, we get:

$$dV_t = \phi_t dS_t + \psi_t dB_t$$

### Conclusion

This step successfully matches the **self-financing condition**!

This two-way proof is the mathematical cornerstone of the Black-Scholes-Merton model. It guarantees that pricing derivative contracts by taking expectations under the risk-neutral measure $\mathbb{Q}$ is mathematically identical to building a risk-free, self-financing replicating trading strategy in the market.

---

**See also:** [[19 The self financing Portfolio]] · [[18 Previsible Process and Martingale Representation Theorem]] · [[21 Martingale Pricing Of European Contingent Claims]]
