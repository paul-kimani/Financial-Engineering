### **1. Introduction to the Binomial Option Pricing Model (BOPM)**

The primary purpose of the BOPM in these notes is to provide a discrete-time framework to understand two major concepts: **arbitrage theory** and **probability theory**. Ultimately, this builds up to the **Fundamental Theorem of Arbitrage Pricing (FTAP)**.
Here is how the model is constructed:

- **Discrete Time:** Unlike continuous-time models (like Black-Scholes), the BOPM evaluates the stock price at specific, discrete intervals (e.g., "tosses" or periods).
- **The Initial State:** We start at time $0$ with a known initial stock price, denoted as $S_0$.
- **The Price Multipliers:** The model assumes the stock price can only do one of two things in the next period: go up or go down.  
    - It goes up by a factor of $u$ (where $u > 1$).
    - It goes down by a factor of $d$ (where $d < 1$).
    - This gives us the fundamental boundary condition: $0 < d < 1 < u$.

**Building the State Tree**
From $S_0$, we can map out the potential future states of the stock:
- **After the 1st Period (1st Toss):**
    - **Heads (Up):** $S_1(H) = uS_0$
    - **Tails (Down):** $S_1(T) = dS_0$
- **After the 2nd Period (2nd Toss):**
    - **Two Heads (Up, Up):** $S_2(HH) = u^2S_0$
    - **One Head, One Tail (Up, Down / Down, Up):** $S_2(HT) = udS_0$
    - **Two Tails (Down, Down):** $S_2(TT) = d^2S_0$

### **2. The No-Arbitrage Condition**
To accurately price options, the model assumes a "no-arbitrage" environment. This means there is no way to make a guaranteed, risk-free profit out of thin air.


To establish this, we introduce a risk-free asset (like a money market account) with an interest rate of $r$. If you invest 1 unit of currency today, it will grow to $(1+r)$ in the next period.


For the binomial model to be valid and free of arbitrage, the following mathematical condition must hold true:
  

$$d < 1+r < u$$

**Why is this condition necessary?**

Your notes explore what would happen if this condition were violated. For example, **what happens if $u < 1+r$?**
  
If $u$ (the maximum possible return factor of the stock) is less than $(1+r)$ (the guaranteed return factor of the risk-free asset), a glaring arbitrage opportunity exists. Here is how you would exploit it:

1. **Short sell the stock:** You borrow the stock and sell it today at $S_0$, giving you cash in hand.

2. **Invest the proceeds:** You put that cash into the risk-free money market account.

3. **The Result:** In the next period, your risk-free investment grows by a factor of $(1+r)$. Meanwhile, the absolute maximum the stock price could have grown is by a factor of $u$.

Because $(1+r) > u$, the money you earned from the risk-free asset will always be greater than the maximum amount you would ever have to pay to buy back the stock and cover your short position. You have just locked in a guaranteed, risk-free profit without using any of your own money.


To prevent this theoretical "free money" scenario, the return of the risk-free asset $(1+r)$ must always sit strictly between the stock's down-factor ($d$) and up-factor ($u$).

### **3. Setting Up a Single Period BOPM & The Replicating Portfolio**

Now that the rules of the environment are established, the notes move to pricing an actual derivative using a "replicating portfolio."

**The Option Setup**

Consider a standard European Call option with a strike price of $K > 0$.

At maturity ($T$), the payoff of this call option is defined as:

$$\text{Payoff} = \max(S_T - K, 0) = (S_T - K)^+$$

Suppose at time 0, you decide to short (sell) this call option for an initial price of $C_0$. Because you sold the option, you take on an obligation to pay out the payoff at maturity based on what the stock does:

- **If Heads ($H$):** You are obligated to pay $(uS_0 - K)^+$
- **If Tails ($T$):** You are obligated to pay $(dS_0 - K)^+$

**Building the Replicating Portfolio ($X$)**
To figure out what the fair price $C_0$ should be, we construct a portfolio that perfectly mimics (replicates) these exact obligations, regardless of whether the stock goes up or down.


Here is how the portfolio is constructed at time 0:
1. You hold a certain quantity of the stock, denoted as $\Delta_0$.
2. You take the cash received from selling the call ($C_0$) and subtract the cost of buying the stock ($\Delta_0 S_0$).
3. This remaining cash balance, $[C_0 - \Delta_0 S_0]$, is invested in the risk-free money market to earn the interest rate $r$.

**Evaluating the Portfolio at Time 1**

In the next period, your stock position will change in value, and your cash position will have grown by the interest rate $(1+r)$. We evaluate this in both possible states:


- **If the stock price goes UP ($H$):**
$$X_1(H) = \Delta_0 S_1(H) + [C_0 - \Delta_0 S_0](1+r)$$
- **If the stock price goes DOWN ($T$):**
 
$$X_1(T) = \Delta_0 S_1(T) + [C_0 - \Delta_0 S_0](1+r)$$
**The Core Objective**

For this to be a true replicating portfolio, we need to mathematically deduce the exact option price ($C_0$) and the exact stock quantity ($\Delta_0$) such that the portfolio's value matches the call option's value in _both_states simultaneously:

1. $$C_1(H) = X_1(H) = \Delta_0 S_1(H) + [C_0 - \Delta_0 S_0](1+r)$$
2. $$C_1(T) = X_1(T) = \Delta_0 S_1(T) + [C_0 - \Delta_0 S_0](1+r)$$
### **4. Solving for Delta ($\Delta_0$) and the Initial Option Price ($C_0$)**

From the previous step, we have a system of two equations with two unknowns ($\Delta_0$ and $C_0$):
1. $$C_1(H) = \Delta_0 S_1(H) + [C_0 - \Delta_0 S_0](1+r)$$
2. $$C_1(T) = \Delta_0 S_1(T) + [C_0 - \Delta_0 S_0](1+r)$$
**Step 1: Finding the Hedge Ratio ($\Delta_0$)**

To isolate $\Delta_0$, we can eliminate the cash/bond portion of the portfolio by subtracting equation (2) from equation (1). Because the term $[C_0 - \Delta_0 S_0](1+r)$ is identical in both equations, it cancels out completely:

  

$$C_1(H) - C_1(T) = \Delta_0 S_1(H) - \Delta_0 S_1(T)$$

Factoring out $\Delta_0$, we get:

  

$$\Delta_0 = \frac{C_1(H) - C_1(T)}{S_1(H) - S_1(T)}$$

_Note: This is a crucial financial concept. $\Delta_0$ (Delta) represents the change in the option's value divided by the change in the stock's value. It tells you exactly how many shares of stock you need to hold to perfectly hedge the option._

**Step 2: Finding the Initial Option Price ($C_0$)**

Now that we have the formula for $\Delta_0$, the notes proceed to solve for $C_0$ by substituting $\Delta_0$back into equation (2) (though equation 1 would also work).

The notes (across the third and fourth pages) go through a rigorous algebraic expansion to isolate $C_0$. Here is the logical flow of that algebra:
1. Substitute the $\Delta_0$ fraction into equation (2).
2. Substitute $S_1(H)$ with $uS_0$ and $S_1(T)$ with $dS_0$.
3. Group the terms containing $C_1(H)$ and $C_1(T)$ together.
4. Multiply across by $(1+r)$ and find common denominators to simplify the fractions.

After factoring out $S_0$ (which cancels out) and rearranging the terms, we arrive at the isolated formula for the initial arbitrage price of the European call option:

  

$$C_0 = \frac{1}{(1+r)} \left[ C_1(H)\frac{(1+r)-d}{u-d} + C_1(T)\frac{u-(1+r)}{u-d} \right]$$

This equation tells us that the price of the option today ($C_0$) is the discounted value (discounted by $\frac{1}{1+r}$) of the future payoffs $C_1(H)$ and $C_1(T)$, weighted by those complex fractional terms.