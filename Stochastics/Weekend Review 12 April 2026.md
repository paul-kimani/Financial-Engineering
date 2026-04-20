### 1. Basic Concepts on Stochastic Processes

A **Stochastic Process** is a collection of random variables $({X_t : t \in T})$ indexed by time $(t)$. It describes the evolution of a system over time where there is inherent randomness or uncertainty.

**Key Foundational Concepts:**

- **State Space:** The set of all possible values $(X_t)$ can take (e.g., stock price can be $(\mathbb{R}^+)$, a coin flip is $({H, T}))$.
- **Index Set (Time):**
    - **Discrete-Time:** $(T = {0, 1, 2, ...})$ (e.g., daily closing price).
    - **Continuous-Time:** $(T = [0, \infty))$ (e.g., instantaneous temperature or tick-by-tick price).
- **Sample Path (Trajectory):** The actual observed sequence of values over time for one specific outcome $(\omega)$: $(t \mapsto X_t(\omega))$.
- **Filtration $((\mathcal{F}_t))$:** The **information set** available up to time $(t)$. This is a crucial concept for finance. It means we cannot "look into the future." A process is **Adapted** if the value $(X_t)$ is known based only on information available at time $(t)$.

---

### 2. Ordinary vs. Stochastic Calculus / ODE vs. SDE

This is the most critical distinction in quantitative finance. Standard calculus fails when modeling random shocks.

| Feature           | **Ordinary Calculus (ODE)**              | **Stochastic Calculus (SDE)**                                                            |
| :---------------- | :--------------------------------------- | :--------------------------------------------------------------------------------------- |
| **Equation**      | $(\frac{dX}{dt} = a(X, t))$              | $(dX_t = \mu(X_t, t)dt + \sigma(X_t, t)dW_t)$                                            |
| **Driving Noise** | None (Smooth function of time)           | **White Noise / Brownian Motion $((W_t))$**                                              |
| **Smoothness**    | Solutions are differentiable everywhere. | Solutions are **continuous but nowhere differentiable**. They are fractal-like (jagged). |
| **Chain Rule**    | Standard: $(df(X) = f'(X)dX)$            | **Ito's Lemma:** $(df(X) = f'(X)dX + \frac{1}{2}f''(X)(dX)^2)$                           |
| **Prediction**    | Exact deterministic forecast.            | Probability distribution forecast.                                                       |

**Example Visualization:**
- **ODE:** $(\frac{dP}{dt} = rP \implies P_t = P_0 e^{rt})$ (Smooth exponential curve).
- **SDE:** $(dS_t = \mu S_t dt + \sigma S_t dW_t)$ (Jagged, volatile path with an exponential **trend**).

---

### 3. Defining Stochastic Integrals (Focus on Itô Integral)

The integral $(\int f(t) dW_t)$ cannot be defined using Riemann-Stieltjes methods because Brownian Motion $((W_t))$ has **infinite total variation** on any interval.

**The Itô Integral Definition:**
The Itô integral is defined as the limit in mean square of the sum:
$$
\int_0^T f(t, \omega) dW_t = \lim_{n \to \infty} \sum_{i=0}^{n-1} f(t_i, \omega) \cdot (W_{t_{i+1}} - W_{t_i})
$$

**Crucial Property: Non-Anticipating (Adapted)**
The integrand $(f)$ is evaluated at the **left endpoint** $(t_i)$ (the beginning of the time interval).
- **Financial Interpretation:** You decide how many shares to buy **before** you know the price movement over the next instant. You cannot use information from the future ($(t_{i+1})$).
- **Consequence:** The Itô integral is a **Martingale** (its expected future value is its current value). If you used the midpoint (Stratonovich integral), the process would not be a martingale, which breaks the "no-arbitrage" pricing theory.

---

### 4. Types of Stochastic Processes & Properties

Here is a taxonomy of processes from simple to complex, as required by the objective.

#### A. Discrete-Time Processes
1.  **Random Walk:** $(X_{n} = X_{n-1} + \epsilon_n)$ where $(\epsilon)$ is i.i.d. noise (e.g., coin flip).
    - *Property:* Non-stationary variance (variance grows with time).
2.  **AR(1) Process:** $(X_n = \phi X_{n-1} + \epsilon_n)$.
    - *Property:* **Mean Reversion** (if $(|\phi|<1)$, it pulls back to a long-term mean).

#### B. Continuous-Time Processes
1.  **Brownian Motion ($(W_t)$ or $(B_t)$):** The building block.
    - *Properties:*
        1.  $(W_0 = 0)$.
        2.  **Independent Increments:** $(W_{t+s} - W_t)$ is independent of past history.
        3.  **Gaussian Increments:** $(W_{t+s} - W_t \sim \mathcal{N}(0, s))$.
        4.  **Continuous Paths:** Nowhere differentiable.

2.  **Poisson Process ($(N_t)$):** Counting jumps.
    - *Examples:* Number of defaults, number of trades arriving.
    - *Property:* Jumps by +1 at random, exponentially distributed times.

#### C. Key Properties of Processes (Vocabulary)
- **Stationarity:** Statistical properties (mean, variance) do not change over time. *Example: Returns might be stationary, but prices are not.*
- **Martingale:** $(E[X_{t+s} | \mathcal{F}_t] = X_t)$. A "fair game." *Example: The Itô integral itself.*
- **Markov Property:** The future depends only on the **present state**, not the full history. *Example: Stock price models (Geometric Brownian Motion).*

---

### 5. Diffusion Processes & Financial Examples

A **Diffusion Process** is a continuous-time Markov process with **continuous sample paths**. It is the solution to an SDE of the form:
$$
dX_t = \mu(t, X_t)dt + \sigma(t, X_t)dW_t
$$
- $(\mu)$: **Drift** (deterministic trend).
- $(\sigma)$: **Diffusion coefficient** (volatility/randomness).

**Examples of Diffusion Processes Used in Finance:**

| Process Name                         | SDE                                             | Financial Application                                                                                      | Key Characteristic                                                   |
| :----------------------------------- | :---------------------------------------------- | :--------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------- |
| **Arithmetic Brownian Motion**       | $(dX = \mu dt + \sigma dW)$                     | Modeling spread between two stocks; Physical position of a particle.                                       | Value can go **negative**. Bad for stock prices.                     |
| **Geometric Brownian Motion (GBM)**  | $(dS = \mu S dt + \sigma S dW)$                 | **Black-Scholes Model** for stock prices.                                                                  | Lognormal distribution. **Stays positive.**                          |
| **Ornstein-Uhlenbeck (OU) Process**  | $(dX = \theta(\mu - X)dt + \sigma dW)$          | **Interest Rates (Vasicek Model)**; Commodity prices.                                                      | **Mean-Reverting**. Pulls back to level \(\mu\) at speed \(\theta\). |
| **Cox-Ingersoll-Ross (CIR) Process** | $(dX = \theta(\mu - X)dt + \sigma \sqrt{X} dW)$ | **Interest Rates** (prevents negative rates if parameters met); **Stochastic Volatility (Heston Model)** . | Volatility depends on level.                                         |
| **Brownian Bridge**                  | $(dX = \frac{b-X}{T-t}dt + dW)$                 | Modeling bond prices converging to par at maturity.                                                        | Value is **pinned** to a specific value at future time \(T\).        |
### The Big Problem
We want to calculate something like:
> `(Amount of Stock I Own) × (Change in Stock Price)`

The "Change in Stock Price" is Brownian Motion, $( B(t) )$. But we know $( B(t) )$ is **too jagged** to use normal calculus rules (it's like trying to measure the length of a lightning bolt with a ruler).

So, mathematicians cheat: **We start with easy shapes, then take the limit to get the hard shape.**

### Step 1: The Simplest Case (The Flat Line)
> *"if $( X(t) = 1 )$, then $( \int dB(t) = B(T) - B(0) )$"*

**Plain English:** Imagine you hold **exactly 1 share of stock** from market open to market close. You never buy more, you never sell.
- How much money did you make?
- **Answer:** Just `Final Price - Starting Price`.
- That's it. If $( X=1 )$, the integral is just $( B(T) - B(0) )$. The "X" just tags along for the ride.

### Step 2: The Constant Case (The Higher Flat Line)
> *"if $( X(t) )$ is a constant $( c )$, then the integral should be $( c(B(T) - B(0)) )$"*

**Plain English:** Imagine you hold **exactly 3 shares** all day ($( c=3 )$).
- How much money did you make?
- **Answer:** $( 3 \times \text{(Price Change)} )$.
- Because $( c )$ is constant, we can just pull it outside the integral. This is the only time the Itô integral acts exactly like a normal high school integral.

### Step 3: The "Simple Process" (The Staircase)
> *"if $( X(t) )$ takes two values, $( c_1 )$ on $( (0, a] )$, and $( c_2 )$ on $( (a, T] )$..."*

This is the key "Aha!" moment.
You cannot predict the jagged path of $( B(t) )$, but **YOU control how many shares you hold.**

**Scenario:**
- **Morning (0 to Noon):** You hold 2 shares ($( c_1 = 2 )$).
- **At Noon:** You instantly change your position to 5 shares ($( c_2 = 5 )$).
- **Afternoon (Noon to Close):** You hold 5 shares.

**How do we calculate total profit?**
Just add the two parts separately!
1.  **Morning Profit:** $( 2 \times (\text{Price at Noon} - \text{Price at Open}) )$
2.  **Afternoon Profit:** $( 5 \times (\text{Price at Close} - \text{Price at Noon}) )$
3.  **Total Integral:** Morning Profit + Afternoon Profit.

**Visual Analogy:**
Imagine $( B(t) )$ is a wiggly road going uphill. $( X(t) )$ is the gear you are in.
- Even though the road is bumpy, if you stay in **2nd gear** for the first mile, the engine work is easy to calculate.
- You shift to **5th gear** for the second mile.

The **Simple Process** is just a list of instructions: "Gear 2 from 0 to A, Gear 5 from A to T."

### Step 4: The Limiting Procedure (Turning Stairs into a Slide)
> *"According to the limiting procedure the integral is then defined for more general processes."*

Now we get to the genius part.
In real life, a trader doesn't just change shares twice a day. They trade **continuously** (or every millisecond). So the "staircase" $( X(t) )$ becomes a smooth-ish curve.

**The Procedure:**
1.  Approximate the smooth curve with **many tiny steps** (a staircase with 1,000 steps).
2.  Calculate the integral for that staircase (it's just adding up 1,000 little profits).
3.  Take the limit as the steps get **infinitely small**.

**The Crucial Catch (The Itô Part):**
*When you are on the step from 9:00:00am to 9:00:01am, which price do you use to calculate the number of shares?*
- **Itô says:** You MUST use the price at **9:00:00am** (the left side of the step). *You cannot look into the future.*
- If you used the midpoint or the end of the step, you'd be cheating (predicting the future). This choice makes all the finance math work correctly (No Arbitrage).

### Summary: The "So What?"

| Concept                | Math Symbol                        | Simple Meaning                                                       |
| :--------------------- | :--------------------------------- | :------------------------------------------------------------------- |
| **Simple Process**     | $( X(t) = c )$ on intervals        | **A Trading Plan:** "Buy and Hold for 1 hour, then Sell."            |
| **Integral of Simple** | Sum of $( c_i \times \Delta B_i )$ | **Mark-to-Market P&L:** Add up profit from each holding period.      |
| **Limiting Procedure** | Making steps smaller               | Moving from *trading once an hour* to *trading every microsecond*.   |
| **Result**             | $( \int X dB )$                    | **Total Profit** from a dynamic trading strategy in a random market. |

We are basically saying: *"We know how to measure the area under a straight line. We know how to measure the area under a staircase. By making the steps infinitely tiny, we can measure the area under a curve—even if that curve is a chaotic, jagged mess."*