### **1. The 2-Period Setup and Dynamic Hedging**

In a single-period model, you set your hedge at time 0 and hold it until time 1. In a two-period model, you have the opportunity to **adjust your hedge** after the first period (toss 1) based on whether the stock went up or down.

  

- At time 1, the investor has wealth equal to $X_1$ dollars.
- They decide to adjust their portfolio by holding a new amount of shares, $\Delta_1$.
- Just like in period 1, the remainder of their wealth, which is $(X_1 - \Delta_1 S_1)$, is invested in the money market.
Therefore, in the next period (time 2), the investor's wealth ($C_2$) will be calculated as:
$$C_2 = \Delta_1 S_2 + (1+r)\{X_1 - \Delta_1 S_1\}$$
### **2. Expanding the Tree (The Four States)**

Because there are now two tosses, the stock price path branches into four possible terminal states at time 2:
- **If Heads then Heads:** $S_2(HH) = u^2 S_0$
- **If Heads then Tails:** $S_2(HT) = ud S_0$
- **If Tails then Heads:** $S_2(TH) = ud S_0$ (Note: the order of $T$ and $H$ results in the same $ud$multiplier)
- **If Tails then Tails:** $S_2(TT) = d^2 S_0$

### **3. Setting up the Equations**

To price the option, we must ensure our replicating portfolio matches the option's payoff at time 2 for _every_possible path. Because the new hedge ratio ($\Delta_1$) depends entirely on what happened at time 1, we get two distinct sets of equations:

**If the first toss was Heads ($H$):**
1. $$C_2(HH) = \Delta_1(H) S_2(HH) + (1+r)\{X_1(H) - \Delta_1(H) S_1(H)\}$$
2. $$C_2(HT) = \Delta_1(H) S_2(HT) + (1+r)\{X_1(H) - \Delta_1(H) S_1(H)\}$$

**If the first toss was Tails ($T$):**

3.
$$C_2(TH) = \Delta_1(T) S_2(TH) + (1+r)\{X_1(T) - \Delta_1(T) S_1(T)\}$$
4.
$$C_2(TT) = \Delta_1(T) S_2(TT) + (1+r)\{X_1(T) - \Delta_1(T) S_1(T)\}$$

The immediate goal established at the bottom of the page is to solve for the adjusted hedge ratios ($\Delta_1(H)$ and $\Delta_1(T)$) and the portfolio values at time 1 ($X_1(H)$ and $X_1(T)$).

### **4. Solving for the Hedge Ratios ($\Delta_1$)**

The method here is identical to what you did in the single-period model: subtract one equation from the other to eliminate the cash portion of the portfolio.

**Finding the hedge ratio if the first toss was Tails ($\Delta_1(T)$):**

We subtract equation (4) from equation (3):

  

$$C_2(TH) - C_2(TT) = \Delta_1(T) S_2(TH) - \Delta_1(T) S_2(TT)$$

Isolating $\Delta_1(T)$:

$$\Delta_1(T) = \frac{C_2(TH) - C_2(TT)}{S_2(TH) - S_2(TT)}$$

**Finding the hedge ratio if the first toss was Heads ($\Delta_1(H)$):**

We subtract equation (2) from equation (1):

$$C_2(HH) - C_2(HT) = \Delta_1(H) S_2(HH) - \Delta_1(H) S_2(HT)$$

Isolating $\Delta_1(H)$:

$$\Delta_1(H) = \frac{C_2(HH) - C_2(HT)}{S_2(HH) - S_2(HT)}$$

### **5. Valuing the Portfolio at Time 1 using Risk-Neutral Probabilities**

To find the value of the portfolio at time 1 ($X_1(H)$ and $X_1(T)$), we could do the long, messy algebraic substitution again. However, your notes introduce a crucial shortcut: **"We use the risk neutral valuation approach, which simplifies the process."**

The core principle stated here is that the value of the portfolio at time 1 must be the **discounted expected value of its future payoff at time 2**, calculated using risk-neutral probabilities.

Because the portfolio is designed to perfectly replicate the option, the value of the option at time 1 is exactly equal to the value of the portfolio:

- $C_1(H) = X_1(H)$
- $C_1(T) = X_1(T)$

Therefore, we can simply jump straight to the risk-neutral formulas for time 1 (which you'll find explicitly written at the top of the next page):

- $X_1(T) = C_1(T) = (1+r)^{-1} \{ \tilde{p} C_2(TH) + \tilde{q} C_2(TT) \}$
- $X_1(H) = C_1(H) = (1+r)^{-1} \{ \tilde{p} C_2(HH) + \tilde{q} C_2(HT) \}$

This shows that the value at any node is just the discounted, probability-weighted average of the two nodes directly in front of it.

### **4. Solving for the Hedge Ratios ($\Delta_1$)**

The method here is identical to what you did in the single-period model: subtract one equation from the other to eliminate the cash portion of the portfolio.

**Finding the hedge ratio if the first toss was Tails ($\Delta_1(T)$):**

We subtract equation (4) from equation (3):
$$C_2(TH) - C_2(TT) = \Delta_1(T) S_2(TH) - \Delta_1(T) S_2(TT)$$
Isolating $\Delta_1(T)$:
$$\Delta_1(T) = \frac{C_2(TH) - C_2(TT)}{S_2(TH) - S_2(TT)}$$
**Finding the hedge ratio if the first toss was Heads ($\Delta_1(H)$):**

We subtract equation (2) from equation (1):
$$C_2(HH) - C_2(HT) = \Delta_1(H) S_2(HH) - \Delta_1(H) S_2(HT)$$
Isolating $\Delta_1(H)$:
$$\Delta_1(H) = \frac{C_2(HH) - C_2(HT)}{S_2(HH) - S_2(HT)}$$
### **5. Valuing the Portfolio at Time 1 using Risk-Neutral Probabilities**

To find the value of the portfolio at time 1 ($X_1(H)$ and $X_1(T)$), we could do the long, messy algebraic substitution again. However, your notes introduce a crucial shortcut: **"We use the risk neutral valuation approach, which simplifies the process."**

The core principle stated here is that the value of the portfolio at time 1 must be the **discounted expected value of its future payoff at time 2**, calculated using risk-neutral probabilities.

Because the portfolio is designed to perfectly replicate the option, the value of the option at time 1 is exactly equal to the value of the portfolio:
- $C_1(H) = X_1(H)$
- $C_1(T) = X_1(T)$
Therefore, we can simply jump straight to the risk-neutral formulas for time 1 (which you'll find explicitly written at the top of the next page):

- $X_1(T) = C_1(T) = (1+r)^{-1} \{ \tilde{p} C_2(TH) + \tilde{q} C_2(TT) \}$
- $X_1(H) = C_1(H) = (1+r)^{-1} \{ \tilde{p} C_2(HH) + \tilde{q} C_2(HT) \}$

This shows that the value at any node is just the discounted, probability-weighted average of the two nodes directly in front of it.

### **6. Stepping Back to Time 0 (Backward Induction)**

We have the values of the option at time 1 ($C_1(H)$ and $C_1(T)$), but the ultimate goal is to find the price of the option today, at time 0 ($C_0$).

  

Your notes recall the fundamental single-period formula we derived earlier:

  

$$C_0 = (1+r)^{-1} \{ \tilde{p} C_1(H) + \tilde{q} C_1(T) \}$$

To get the full 2-period BOPM formula, we simply take the big formulas we just found for $C_1(H)$ and $C_1(T)$ and substitute them directly into this equation.

  

Here is that massive substitution block shown in your notes:

  

$$C_0 = (1+r)^{-1} \left[ \tilde{p} \left\{ (1+r)^{-1} [\tilde{p} C_2(HH) + \tilde{q} C_2(HT)] \right\} + \tilde{q} \left\{ (1+r)^{-1} [\tilde{p} C_2(TH) + \tilde{q} C_2(TT)] \right\} \right]$$

### **7. Expanding and Simplifying the 2-Period Formula**

To make this monster equation usable, we need to expand the brackets and simplify, which is shown at the very bottom of "IMG_2927.jpg" and carries over to the top of "IMG_2928.jpg".

  

**Step A: Factor out the discount rate**

Both of the inner brackets contain $(1+r)^{-1}$. When we pull that out and multiply it by the $(1+r)^{-1}$already sitting at the front, we get $(1+r)^{-2}$. This makes intuitive sense: we are discounting cash flows from two periods in the future back to today.

  

**Step B: Multiply the probabilities**

Next, we distribute the outer $\tilde{p}$ and $\tilde{q}$ to the inner terms:

  

- $\tilde{p} \times \tilde{p} C_2(HH) \rightarrow \tilde{p}^2 C_2(HH)$

- $\tilde{p} \times \tilde{q} C_2(HT) \rightarrow \tilde{p}\tilde{q} C_2(HT)$

- $\tilde{q} \times \tilde{p} C_2(TH) \rightarrow \tilde{q}\tilde{p} C_2(TH)$

- $\tilde{q} \times \tilde{q} C_2(TT) \rightarrow \tilde{q}^2 C_2(TT)$

**Step C: Combine the middle paths**

Because the order of the tosses doesn't matter for the final stock price (an "Up then Down" results in $udS_0$, and a "Down then Up" also results in $udS_0$), the option payoffs at those nodes are identical: $C_2(HT) = C_2(TH)$.

  

Because they are identical, we can combine $\tilde{p}\tilde{q}$ and $\tilde{q}\tilde{p}$ into a single term: $2\tilde{p}\tilde{q} C_2(HT)$.

  

This leaves us with the final, simplified formula for a 2-Period Binomial Model (at the top of "IMG_2928.jpg"):

  

$$C_0 = (1+r)^{-2} \{ \tilde{p}^2 C_2(HH) + 2\tilde{p}\tilde{q} C_2(HT) + \tilde{q}^2 C_2(TT) \}$$

Take a moment to look at that final formula. Does it remind you of anything from basic algebra or probability? (Hint: look at the coefficients 1, 2, 1 and the powers of $\tilde{p}$ and $\tilde{q}$). Once that clicks, generalizing this to $n$-periods becomes incredibly easy.