### **1. The Discounted Stock Price is a Martingale (Theorem 1)**

Similarly to the option-pricing formula, one can show that the discounted stock price process is a martingale under the risk-neutral measure $\tilde{\mathbb{Q}}$. For the single step from time $0$ to time $1$, this is the statement:

$$S_0 = \frac{1}{1+r}\,\mathbb{E}_{\tilde{\mathbb{Q}}}[S_1]$$

**Proof:**

$$\mathbb{E}_{\tilde{\mathbb{Q}}}[S_1] = \tilde{p}\,S_1(H) + \tilde{q}\,S_1(T) = \tilde{p}\,uS_0 + \tilde{q}\,dS_0$$

but recall $\tilde{p} = \dfrac{(1+r)-d}{u-d}$ and $\tilde{q} = \dfrac{u-(1+r)}{u-d}$.

$$\therefore S_0 \stackrel{?}{=} \frac{1}{1+r}\left\{\frac{(1+r)-d}{u-d}\cdot uS_0 + \frac{u-(1+r)}{u-d}\cdot dS_0\right\}$$

$$= \frac{S_0}{(1+r)(u-d)}\Big\{[(1+r)-d]\,u + [u-(1+r)]\,d\Big\}$$

$$= \frac{S_0}{(1+r)(u-d)}\Big\{(1+r)u - ud + ud - (1+r)d\Big\}$$

$$= \frac{S_0}{(1+r)(u-d)}\cdot(1+r)(u-d) = S_0 \quad \checkmark$$

By an identical argument at every step $n$, the discounted stock price process is a martingale under $\tilde{\mathbb{Q}}$:

$$\mathbb{E}_{\tilde{\mathbb{Q}}}\left[(1+r)^{-(n+1)}S_{n+1}\,\middle|\,\mathcal{F}_n\right] = (1+r)^{-n}S_n$$

### **2. The Discounted Self-Financing Portfolio Value is a Martingale (Theorem 2)**

**Theorem 2:** Under $\tilde{\mathbb{Q}}$, the discounted self-financing process value $\left\{(1+r)^{-(n+1)}X_{n+1}\,\middle|\,\mathcal{F}_n\right\}_{n=0}^{2^N}$ is a martingale.

**Proof:** The value of the portfolio at time $n+1$, denoted $X_{n+1}$, is equal to:

$$X_{n+1} = \Delta_nS_{n+1} + (1+r)\{X_n - \Delta_nS_n\}$$

The equation represents the value of a portfolio holding $\Delta_n$ shares of stock and a risk-free bond. The value of the remaining wealth, $\{X_n - \Delta_nS_n\}$, is compounded at the risk-free rate.

$$\mathbb{E}_{\tilde{\mathbb{Q}}}\left[(1+r)^{-(n+1)}X_{n+1}\,\middle|\,\mathcal{F}_n\right] = \mathbb{E}_{\tilde{\mathbb{Q}}}\left[(1+r)^{-(n+1)}\big\{\Delta_nS_{n+1} + (1+r)(X_n-\Delta_nS_n)\big\}\,\middle|\,\mathcal{F}_n\right]$$

Since $X_n$ and $\Delta_nS_n$ are known at time $n$, they are $\mathcal{F}_n$-measurable, so they can be treated as constants:

$$= \Delta_n\,\mathbb{E}_{\tilde{\mathbb{Q}}}\left[(1+r)^{-(n+1)}S_{n+1}\,\middle|\,\mathcal{F}_n\right] + (1+r)^{-n}(X_n - \Delta_nS_n)$$

From **Theorem 1**, we know that the discounted stock price process is a martingale under the risk-neutral measure:

$$\mathbb{E}_{\tilde{\mathbb{Q}}}\left[(1+r)^{-(n+1)}S_{n+1}\,\middle|\,\mathcal{F}_n\right] = (1+r)^{-n}S_n$$

$$\therefore \mathbb{E}_{\tilde{\mathbb{Q}}}\left[(1+r)^{-(n+1)}X_{n+1}\,\middle|\,\mathcal{F}_n\right] = \Delta_n(1+r)^{-n}S_n + (1+r)^{-n}(X_n-\Delta_nS_n)$$

$$= (1+r)^{-n}\big[\Delta_nS_n + X_n - \Delta_nS_n\big] = (1+r)^{-n}X_n$$

$$\therefore \mathbb{E}_{\tilde{\mathbb{Q}}}\left[(1+r)^{-(n+1)}X_{n+1}\,\middle|\,\mathcal{F}_n\right] = (1+r)^{-n}X_n$$

— which is a martingale. $\blacksquare$

### **3. Is the BOPM Complete? (Theorem 3)**

If any derivative can be hedged, then the model is **complete** — the market admits no arbitrage.

Define **recursively backward in time** the sequence of random variables $C_N,\dots,C_0$ by:

$$C_n(w_1,\dots,w_n) = (1+r)^{-1}\left[\tilde{p}\,C_{n+1}(w_1,\dots,w_n;H) + \tilde{q}\,C_{n+1}(w_1,\dots,w_n;T)\right]$$

Next, define the portfolio process:

$$\Delta_n(w_1,\dots,w_n) = \frac{C_{n+1}(w_1,\dots,w_n;H) - C_{n+1}(w_1,\dots,w_n;T)}{S_{n+1}(w_1,\dots,w_n;H) - S_{n+1}(w_1,\dots,w_n;T)}$$

If we set $X_0 = C_0$ and define, **recursively forward in time**:

$$X_{n+1} = \Delta_nS_{n+1} + (1+r)(X_n - \Delta_nS_n)$$

then $X_n = C_n$, and $C_{n+1}$ is the option process value, provided that at time $n$ the sequence $(w_1,w_2,\dots,w_n)$ happened.

**Proof (up-state case):**

The goal is to prove that the BOPM is a complete market by showing that the replicating portfolio $X_{n+1}$ matches the value of the option $C_{n+1}$ at time $n+1$. We prove this using the case for the "up state," denoted $H$.

The value of the self-financing portfolio in the up-state is:

$$X_{n+1}(H) = \Delta_nS_{n+1}(H) + (1+r)(X_n-\Delta_nS_n)$$

but $S_{n+1}(H) = uS_n$:

$$X_{n+1}(H) = \Delta_n\,uS_n + (1+r)X_n - (1+r)\Delta_nS_n = (1+r)X_n + \Delta_nS_n\{u-(1+r)\}$$

but the hedge ratio is:

$$\Delta_n = \frac{C_{n+1}(H)-C_{n+1}(T)}{uS_n - dS_n}$$

$$\therefore X_{n+1}(H) = (1+r)X_n + \frac{C_{n+1}(H)-C_{n+1}(T)}{u-d}\{u-(1+r)\}$$

but the risk-neutral down-probability is $\tilde{q} = \dfrac{u-(1+r)}{u-d}$, so:

$$X_{n+1}(H) = (1+r)X_n + \tilde{q}\big[C_{n+1}(H) - C_{n+1}(T)\big]$$

but recall that $X_n = C_n$, and this holds also for the next period. Therefore the fundamental relationship of risk-neutral pricing is:

$$C_n = \frac{1}{1+r}\left[\tilde{p}\,C_{n+1}(H) + \tilde{q}\,C_{n+1}(T)\right]$$

Rearranging this, we have:

$$C_n(1+r) = \tilde{p}\,C_{n+1}(H) + \tilde{q}\,C_{n+1}(T)$$

but $X_n = C_n$:

$$\therefore X_{n+1}(H) = \tilde{p}\,C_{n+1}(H) + \tilde{q}\,C_{n+1}(T) + \tilde{q}\,C_{n+1}(H) - \tilde{q}\,C_{n+1}(T)$$

$$= \tilde{p}\,C_{n+1}(H) + \tilde{q}\,C_{n+1}(H) = (\tilde{p}+\tilde{q})\,C_{n+1}(H)$$

$$\therefore X_{n+1}(H) = C_{n+1}(H)$$

**NB:** This proves that in the Binomial model, the value of the replicating portfolio perfectly matches the value of the option in the up-state. A similar proof can be done in all cases — this is what makes the BOPM a complete market.

### **4. Exercise: The Down-State**

> Using the down state, show that $X_{n+1}(T) = C_{n+1}(T)$.

*(Left open in the notes — attempted in pencil but not yet completed. Worth working through live rather than transcribing an answer here.)*
