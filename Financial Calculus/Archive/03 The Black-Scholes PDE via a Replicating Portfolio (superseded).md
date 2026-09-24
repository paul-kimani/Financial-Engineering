### **1. Motivation: Pricing by Arbitrage**

The Black-Scholes PDE can be derived using the concepts of **pricing by arbitrage**. This method assumes that in a complete market, any derivative can be priced by constructing a **self-financing replicating strategy** that produces the same payoff as the derivative. The value of the derivative must be equal to the value of its replicating portfolio at all times, to prevent arbitrage opportunities.

### **2. Setting Up the Replicating Portfolio and Self-Financing Strategy**

A **self-financing trading strategy** is one where the change in the portfolio's value is due only to changes in the value of the assets, and not to external fund flows.

To replicate the derivative $V$, we form a self-financing portfolio consisting of a stock ($S$) and a bond ($B$). The value of this portfolio at time $t$ is:

$$\Pi(t) = a(t)S(t) + b(t)B(t)$$

- $a(t)$ — the number of units of stock
- $b(t)$ — the number of units of bond

The self-financing assumption implies that the change in the portfolio's value $d\Pi$ is given by:

$$d\Pi = a\,dS + b\,dB$$

### **3. Equating the Derivative's Value to the Portfolio's Value**

This implies that the value of the derivative $V$ is equal to the value of the replicating portfolio:

$$dV = d\Pi \implies dV = a\,dS + b\,dB$$

But the stochastic processes for the stock and the bond are:

$$dS = \mu S\,dt + \sigma S\,dW_t$$
$$dB = rB\,dt$$

By Itô's Lemma, the change in the option's value $dV$ is given by:

$$dV = \left(\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}\right)dt + \left(\sigma S\frac{\partial V}{\partial S}\right)dW_t$$

### **4. Deriving the PDE by Equating the Stochastic and Deterministic Parts**

Substitute the expressions for $dV$, $dS$ and $dB$ into the self-financing equation $dV = a\,dS + b\,dB$:

$$\left(\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}\right)dt + \left(\sigma S\frac{\partial V}{\partial S}\right)dW_t = a(\mu S\,dt + \sigma S\,dW_t) + b(rB\,dt)$$

$$= (a\mu S + rbB)\,dt + (a\sigma S)\,dW_t$$

For a portfolio to be riskless, the stochastic term must be eliminated. We achieve this by setting the coefficients of $dW_t$ on both sides equal to each other:

$$\sigma S\frac{\partial V}{\partial S} = a\sigma S \implies a = \frac{\partial V}{\partial S}$$

*Note: $a$ (delta, $\Delta$) is the number of stocks needed to replicate the derivative's value.*

Equating the coefficients of the deterministic terms ($dt$):

$$\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = a\mu S + rbB$$

but $a = \dfrac{\partial V}{\partial S}$, so the $\mu S\frac{\partial V}{\partial S}$ term on both sides cancels:

$$\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = \mu S\frac{\partial V}{\partial S} + rbB$$

$$\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = rbB$$

### **5. Solving for $bB$ and Completing the PDE**

From the portfolio value equation, $\Pi = V = aS + bB$, we can solve for $bB$:

$$bB = V - aS, \quad \text{but } a = \frac{\partial V}{\partial S}$$
$$\implies bB = V - S\frac{\partial V}{\partial S}$$

Substitute the value of $bB$ back into the equation of the deterministic terms:

$$\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = r\left[V - S\frac{\partial V}{\partial S}\right]$$

$$\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = rV - rS\frac{\partial V}{\partial S}$$

To get the PDE, rearrange the terms and equate to zero:

$$\frac{\partial V}{\partial t} + rS\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} - rV = 0$$

This is the **Black-Scholes-Merton PDE**.

### **6. Discussion Question**

Compare and contrast the assumptions of the CAPM and the BSM model — noting similarities and differences. *(Flagged as a discussion prompt in the notes; not yet worked through.)*
