# 21 — Martingale Pricing of European Contingent Claims

> The payoff of everything so far: the **general risk-neutral pricing formula**
> $V_t = e^{-r(T-t)}\,E^\mathbb{Q}[V_T \mid \mathcal{F}_t]$. Building a
> self-financing replicating portfolio, showing its discounted value is a
> $\mathbb{Q}$-martingale, and equating portfolio to claim at maturity delivers
> the fair price as a discounted risk-neutral expectation of the terminal payoff.

---

## 1. Core Framework and Assumptions

### Replicating Portfolio

- A **replicating strategy** for a derivative contract $X$ is a trading portfolio containing traded assets (typically a risky stock $S_t$ and a risk-free bond $B_t$).

- The portfolio value $V_t$ at any time $t \in [0, T]$ must perfectly equate to the value of the derivative $V_t^X$:

$$V_t = \phi_t S_t + \psi_t B_t = V_t^X \quad \forall t \in [0, T]$$

- At maturity $T$, the terminal payoff of the portfolio must match the contract's payoff:

$$V_T = X_T$$

### Risk-Neutral Derivative Pricing (Martingale Approach)

- This mathematical framework relies on the assumption that a derivative $X$ can be replicated by a **self-financing portfolio** of the underlying risky assets and a risk-free bond.

- To prevent any **arbitrage opportunities**, the value of this replicating portfolio at any time $t$ must equal the fair price of the derivative at that same time $t$.

- The framework formula holds under the confluence of four factors:

    1. **The Martingale Property** under the risk-neutral measure $\mathbb{Q}$.

    2. **Previsible (predictable) trading processes** ($\phi_t, \psi_t$).

    3. **The Self-Financing** nature of the portfolio.

    4. **Perfect Replication** of the portfolio to match the payoff.

### The General Risk-Neutral Pricing Formula

$$\boxed{V_t = e^{-r(T-t)} E^\mathbb{Q}[ \text{Derivative Payoff} \mid \mathcal{F}_t ]}$$

_This formula states that the fair price of a derivative at time $t$ is the risk-neutral expectation of its terminal payoff, discounted back to $t$ at the risk-free rate $r$._

---

## 2. Derivation of the Risk-Neutral Valuation Formula

### Step 1: Setting up the Replicating Portfolio

We want to price a European contingent claim with a payoff $V_T$ at maturity $T$.

1. Let $\Pi_t$ represent the value of a portfolio with allocation strategy $(\phi_t, \psi_t)$ of stocks and risk-free bonds:

    $$\Pi_t = \phi_t S_t + \psi_t B_t$$

2. For perfect replication, our portfolio value must equal the derivative's value at all times, meaning:

    $$V_t = \Pi_t \quad \forall t \in [0, T] \quad \text{and} \quad V_T = \Pi_T$$

### Step 2: Formulating Portfolio Dynamics under $\mathbb{Q}$

By definition, a **self-financing strategy** means the change in portfolio value is driven strictly by asset price changes:

$$d\Pi_t = \phi_t dS_t + \psi_t dB_t$$

We substitute the asset price dynamics under the risk-neutral measure $\mathbb{Q}$:

- **Stock Price:** $dS_t = r S_t dt + \sigma S_t dW_t^\mathbb{Q}$

- **Bond Price:** $dB_t = r B_t dt$

Substituting these SDEs back into the portfolio dynamics:

$$d\Pi_t = \phi_t \left( r S_t dt + \sigma S_t dW_t^\mathbb{Q} \right) + \psi_t \left( r B_t dt \right)$$

$$d\Pi_t = r \underbrace{(\phi_t S_t + \psi_t B_t)}_{\Pi_t} dt + \phi_t \sigma S_t dW_t^\mathbb{Q}$$

$$d\Pi_t = r \Pi_t dt + \phi_t \sigma S_t dW_t^\mathbb{Q}$$

### Step 3: Proving the Discounted Portfolio is a $\mathbb{Q}$-Martingale

Let $\tilde{\Pi}_t = \Pi_t e^{-rt}$ be the **discounted portfolio value**. We apply Itô's Lemma to the function $f(\Pi_t, t) = \Pi_t e^{-rt}$ to find its differential $d\tilde{\Pi}_t$:

$$d\tilde{\Pi}_t = \frac{\partial f}{\partial t} dt + \frac{\partial f}{\partial \Pi_t} d\Pi_t + \frac{1}{2} \frac{\partial^2 f}{\partial \Pi_t^2} (d\Pi_t)^2$$

Evaluating the partial derivatives:

- $\frac{\partial f}{\partial t} = -r \Pi_t e^{-rt}$

- $\frac{\partial f}{\partial \Pi_t} = e^{-rt}$

- $\frac{\partial^2 f}{\partial \Pi_t^2} = 0$

This gives us the general discounted differential formula:

$$d\tilde{\Pi}_t = e^{-rt} d\Pi_t - r \Pi_t e^{-rt} dt$$

Now, substitute the portfolio dynamics $d\Pi_t$ derived in Step 2:

$$d\tilde{\Pi}_t = e^{-rt} \left[ r \Pi_t dt + \phi_t \sigma S_t dW_t^\mathbb{Q} \right] - r \Pi_t e^{-rt} dt$$

$$d\tilde{\Pi}_t = r \Pi_t e^{-rt} dt + \phi_t e^{-rt} \sigma S_t dW_t^\mathbb{Q} - r \Pi_t e^{-rt} dt$$

The drift ($dt$) terms cancel completely, leaving:

$$d\tilde{\Pi}_t = \phi_t e^{-rt} \sigma S_t dW_t^\mathbb{Q}$$

Because $d\tilde{\Pi}_t$ has a drift coefficient of $0$, the discounted portfolio value $\tilde{\Pi}_t$ has no trend and is a **martingale** under measure $\mathbb{Q}$.

### Step 4: Applying the Martingale Property

Since $\tilde{\Pi}_t$ is a $\mathbb{Q}$-martingale, its value today is equal to the conditional expectation of its future value at maturity $T$:

$$\tilde{\Pi}_t = E^\mathbb{Q}[ \tilde{\Pi}_T \mid \mathcal{F}_t ]$$

Expanding this by substituting $\tilde{\Pi}_t = \Pi_t e^{-rt}$ and $\tilde{\Pi}_T = \Pi_T e^{-rT}$:

$$\Pi_t e^{-rt} = E^\mathbb{Q}[ \Pi_T e^{-rT} \mid \mathcal{F}_t ]$$

### Step 5: Final Substitution and Pricing Formula

Because our portfolio perfectly replicates the European contingent claim, the portfolio value at maturity $T$ equals the payoff of the derivative ($\Pi_T = V_T$):

$$\Pi_t e^{-rt} = E^\mathbb{Q}[ V_T e^{-rT} \mid \mathcal{F}_t ]$$

To isolate the current replicating portfolio value $\Pi_t$ (which equals the fair price of our option $V_t$), we multiply both sides by $e^{rt}$:

$$\Pi_t = e^{rt} E^\mathbb{Q}[ V_T e^{-rT} \mid \mathcal{F}_t ]$$

Since the exponential discount term inside the expectation is deterministic with respect to $t$, we combine the exponents to get:

$$\boxed{V_t = E^\mathbb{Q}[ V_T e^{-r(T-t)} \mid \mathcal{F}_t ]}$$

This completes the proof.

---

**See also:** [[21.1 Less Technical Explanation of Martingale Pricing Of European Contingent Claims]] · [[20 Proof of Replicating Portfolio]] · [[29 Derivation of The Black Scholes merton formula.]]
