# 17 — Example Application of Girsanov's Theorem

> A full worked proof: start from the physical stock SDE, apply Itô's lemma to
> the discounted price $X_t = S_t e^{-rt}$, and demand that its drift vanish.
> Forcing the drift to zero pins down the market price of risk
> $\theta = \tfrac{\mu-r}{\sigma}$, and the Girsanov shift then turns the
> real-world dynamics into the risk-neutral $dS_t = r S_t\,dt + \sigma S_t\,dW_t^\mathbb{Q}$.

---

We have a rigorous proof showing how to apply **Girsanov's Theorem** to change the stock price dynamics from the real-world measure $\mathbb{P}$ to the risk-neutral measure $\mathbb{Q}$.

Let's go through the steps to make sure the transitions, calculus, and intuition are crystal clear.

---

## 1. Problem Definition & Applying Itô's Lemma

We begin with the stock price dynamics under the real-world measure $\mathbb{P}$:

$$dS_t = \mu S_t dt + \sigma S_t dW_t \quad \implies \quad \frac{dS_t}{S_t} = \mu dt + \sigma dW_t \quad \cdots (1)$$

### Objective

We want to find an equivalent probability measure $\mathbb{Q}$ such that the discounted stock price $X_t = S_t e^{-rt}$ is a **martingale** under $\mathbb{Q}$.

To do this, we apply Itô's Lemma to the function $f(t, S_t) = S_t e^{-rt}$.

---

## 2. Finding the SDE of the Discounted Stock Price ($dX_t$)

You then calculate the derivatives of $f(t, S_t) = S_t e^{-rt}$ to construct $dX_t$:

- $\frac{\partial f}{\partial t} = -r S_t e^{-rt}$

- $\frac{\partial f}{\partial S_t} = e^{-rt}$

- $\frac{\partial^2 f}{\partial S_t^2} = 0$

Plugging these into Itô's formula:

$$df(t, S_t) = -r S_t e^{-rt} dt + e^{-rt} dS_t + 0$$

Substitute the real-world SDE for $dS_t$:

$$df(t, S_t) = -r S_t e^{-rt} dt + e^{-rt} \left( \mu S_t dt + \sigma S_t dW_t \right)$$

$$df(t, S_t) = (\mu - r) S_t e^{-rt} dt + \sigma S_t e^{-rt} dW_t$$

Since $X_t = S_t e^{-rt}$, we can write the SDE of the discounted price process as:

$$dX_t = (\mu - r) X_t dt + \sigma X_t dW_t \quad \cdots (2)$$

### The Martingale Requirement

For $X_t$ to be a martingale under the target measure $\mathbb{Q}$, its **drift term must be exactly zero**. In other words, under $\mathbb{Q}$, the SDE must simplify to a pure diffusion process:

$$dX_t = \sigma X_t dW_t^\mathbb{Q} \quad \cdots (3)$$

where $dW_t^\mathbb{Q}$ is the increment of a standard Brownian motion under $\mathbb{Q}$.

---

## 3. Finding the Market Price of Risk ($\theta$)

To force the drift of $dX_t$ to zero, we invoke **Girsanov's Theorem**, which allows us to shift the Brownian motion:

$$dW_t = dW_t^\mathbb{Q} - \theta dt \quad \cdots (4)$$

Now, we substitute this relationship back into our real-world equation for $dX_t$ (Equation 2):

$$dX_t = (\mu - r) X_t dt + \sigma X_t \left( dW_t^\mathbb{Q} - \theta dt \right)$$

$$dX_t = X_t \left[ (\mu - r) - \sigma\theta \right] dt + \sigma X_t dW_t^\mathbb{Q} \quad \cdots (5)$$

To satisfy our martingale condition (Equation 3), the drift coefficient in front of $dt$ must equal zero:

$$(\mu - r) - \sigma\theta = 0 \implies \mu - r = \sigma\theta$$

$$\theta = \frac{\mu - r}{\sigma} \quad \cdots (6)$$

This parameter $\theta$ is the **Market Price of Risk** (or the Sharpe ratio of the stock). It represents the risk premium required by real-world investors per unit of volatility.

---

## 4. Transforming the Stock Price SDE

Now we can finally write the stock price dynamics under the risk-neutral measure $\mathbb{Q}$.

First, substitute our solved value of $\theta$ back into the Girsanov shift equation (Equation 4):

$$dW_t = dW_t^\mathbb{Q} - \left( \frac{\mu - r}{\sigma} \right) dt$$

Next, substitute this expression for $dW_t$ back into our original physical stock SDE (Equation 1):

$$\frac{dS_t}{S_t} = \mu dt + \sigma \left[ dW_t^\mathbb{Q} - \left( \frac{\mu - r}{\sigma} \right) dt \right]$$

$$\frac{dS_t}{S_t} = \mu dt + \sigma dW_t^\mathbb{Q} - (\mu - r) dt$$

$$\frac{dS_t}{S_t} = r dt + \sigma dW_t^\mathbb{Q}$$

Multiply both sides by $S_t$ to get the final result:

$$dS_t = r S_t dt + \sigma S_t dW_t^\mathbb{Q}$$

### Summary of the Proof

By defining the change of measure using the market price of risk $\theta = \frac{\mu - r}{\sigma}$, we successfully transformed the real-world SDE (where the stock grows at rate $\mu$) into the risk-neutral SDE (where the stock grows at the risk-free rate $r$).

This elegant mathematical step is what allows us to price derivatives without ever needing to estimate the highly subjective and elusive real-world return parameter $\mu$!

---

## 5. Why This Proof Matters

Notice what disappeared: the subjective drift $\mu$. The whole point of the exercise is that the *discounted* stock is a martingale under $\mathbb{Q}$, which is the precondition for the [[21 - Martingale Pricing Of European Contingent Claims|risk-neutral pricing formula]]. The same $\mathbb{Q}$-dynamics $dS_t = rS_t\,dt + \sigma S_t\,dW_t^\mathbb{Q}$ are the starting point for both routes to Black–Scholes: the [[28 - Derivation of the Black Scholes PDE|PDE via delta-hedging]] and the [[29 - Derivation of The Black Scholes merton formula|direct integration of the expectation]]. This is the "engine room" step that everything downstream relies on.

---

## Connections

**Within Stochastics**
- [[03 - Itô's Lemma - Statement and Derivation]] · [[07 - Solving SDEs with Itô's Lemma]] — the Itô calculus used to differentiate $S_t e^{-rt}$.
- [[16 - Equivalent Probability Measures and Girsanov's Theorem]] — the theorem being applied here.
- [[14 - Asset Dynamics under the probability measures]] — the target $\mathbb{Q}$-dynamics obtained at the end.
- [[18 - Previsible Process and Martingale Representation Theorem]] · [[21 - Martingale Pricing Of European Contingent Claims]] — where the martingale property is put to work.

**Across the programme**
- [[../Derivatives/7 May 2026 - Personal Notes 1]] — GBM as the underlying whose measure we are changing.
