# 25 — Vasicek Model

> The Vasicek short-rate model is the Ornstein–Uhlenbeck process dressed for
> interest rates: $dr_t = a(b - r_t)\,dt + \sigma\,dW_t$, with $b$ the long-run
> level and $a$ the speed of reversion. The same integrating-factor trick yields
> the closed form
> $r_t = r_0 e^{-at} + b(1 - e^{-at}) + \int_0^t \sigma e^{-a(t-s)}\,dW_s$.
> Its Gaussian nature is also its flaw — rates can go negative.

---

The Vasicek model is a mathematical framework used to model the evolution of the short-term interest rate $r_t$. It is a direct application of the Ornstein-Uhlenbeck process (see [[22.1 Properties of Ito Integral And the O-U Process.]]).

---

## 1. The Stochastic Differential Equation (SDE)

Under this model, the short rate $r_t$ is governed by:

$$dr_t = a(b - r_t)dt + \sigma dW_t$$

Where:

- **$b$ (Equilibrium Rate):** The long-term average interest rate.

- **$a$ (Speed of Reversion):** The "pull back" rate at which the short rate $r_t$ is pulled back toward $b$.

    > **A Quick Correction for Your Notes:** Your handwritten notes mention _"the bigger the value of $b$, the faster the pulling effect."_ This is actually a minor slip-up! It is the parameter **$a$** (the coefficient of the drift) that determines how fast the rate is pulled back to the mean. The parameter $b$ just determines _where_ it is being pulled.

- **$\sigma$ (Volatility):** The variability parameter representing random market shocks.

### How Mean Reversion Works Physically

The term $a(b - r_t)dt$ is the deterministic drift, and it acts like an elastic band:

- **If $r_t > b$:** The term $(b - r_t)$ becomes negative, making the drift negative. This pulls the interest rate **down** toward $b$.

- **If $r_t < b$:** The term $(b - r_t)$ becomes positive, making the drift positive. This pushes the interest rate **up** toward $b$.

---

## 2. Core Drawback of the Vasicek Model

### Allows for Negative Interest Rates

Because $r_t$ is ultimately modeled using a normal distribution (just like the general O-U process we solved earlier), there is always a non-zero probability that $r_t$ can drop below zero. Mathematically, the state space is $(-\infty, \infty)$.

While negative interest rates have occasionally occurred in real-world central bank policies, in a standard economic environment, allowing interest rates to easily drift deep into negative territory is considered a significant modeling flaw. This drawback eventually led to the development of other models, like the Cox-Ingersoll-Ross (CIR) model, which prevents negative rates.

---

## 3. Deriving the Solution of the Vasicek SDE

To solve the Vasicek SDE, we follow the exact same "decoupling" trick we used for the general O-U process (see [[22.2 Helper Process]] and [[22.3 Decoupling]]).

### Step 1: The Transition Function (Helper Process)

We define our helper process $U_t$ using the integrating factor $e^{at}$:

$$U_t = f(t, r_t) = r_t e^{at}$$

### Step 2: Set up Itô's Lemma

To find the differential $dU_t$, we write out the general Itô's Lemma expansion:

$$dU_t = \frac{\partial f}{\partial t} dt + \frac{\partial f}{\partial r_t} dr_t + \frac{1}{2} \frac{\partial^2 f}{\partial r_t^2} (dr_t)^2$$

### Step 3: Calculate the Partial Derivatives

- **Time derivative:**

    $$\frac{\partial f}{\partial t} = a r_t e^{at}$$

- **First derivative with respect to $r_t$:**

    $$\frac{\partial f}{\partial r_t} = e^{at}$$

- **Second derivative with respect to $r_t$:**

    $$\frac{\partial^2 f}{\partial r_t^2} = 0$$

### Step 4: Substitute the Derivatives and the SDE

Substitute these partial derivatives back into the Itô expansion:

$$dU_t = a r_t e^{at} dt + e^{at} dr_t + 0$$

Now, plug in the Vasicek SDE for $dr_t$:

$$dU_t = a r_t e^{at} dt + e^{at} \left( a(b - r_t)dt + \sigma dW_t \right)$$

### Step 5: Expand and Decouple

Multiply through by $e^{at}$:

$$dU_t = a r_t e^{at} dt + ab e^{at} dt - a r_t e^{at} dt + \sigma e^{at} dW_t$$

Just like magic, the $+a r_t e^{at} dt$ and $-a r_t e^{at} dt$ terms cancel each other out completely:

$$dU_t = ab e^{at} dt + \sigma e^{at} dW_t$$

Since there are no $U_t$ or $r_t$ terms on the right side of the equation, the system is fully decoupled and ready to be integrated directly from $0$ to $t$!

---

## 4. Integrating the Decoupled SDE

Since we successfully decoupled our SDE into:

$$dU_t = ab e^{at} dt + \sigma e^{at} dW_t$$

We change our dummy variables of integration to $s$ (to avoid confusion with the upper limit $t$) and integrate both sides from $0$ to $t$:

$$\int_0^t dU_s = \int_0^t ab e^{as} ds + \int_0^t \sigma e^{as} dW_s$$

Evaluating each term step-by-step:

#### 1. Left Side:

$$\int_0^t dU_s = \left[ U_s \right]_0^t = U_t - U_0$$

#### 2. Right Side (First Term):

Since $a$ and $b$ are constants, we pull them out of the integral:

$$\int_0^t ab e^{as} ds = ab \left[ \frac{e^{as}}{a} \right]_0^t$$

Notice that the $a$ in the numerator and the $a$ in the denominator cancel out:

$$\frac{ab}{a} \left( e^{at} - e^0 \right) = b \left( e^{at} - 1 \right)$$

#### 3. Right Side (Second Term):

Because the integrand $\sigma e^{as}$ contains the stochastic term $dW_s$, we **cannot** integrate it using standard calculus rules. We must leave it in its stochastic integral form:

$$\int_0^t \sigma e^{as} dW_s$$

### Substituting back $U_t = r_t e^{at}$

Now, we substitute the definition of our helper process ($U_t = r_t e^{at}$ and $U_0 = r_0 e^0 = r_0$) back into our integrated equation:

$$\left( r_t e^{at} - r_0 \right) = b \left( e^{at} - 1 \right) + \int_0^t \sigma e^{as} dW_s$$

To isolate $r_t e^{at}$, we shift $r_0$ to the right side:

$$r_t e^{at} = r_0 + b \left( e^{at} - 1 \right) + \int_0^t \sigma e^{as} dW_s$$

---

## 5. The Closed-Form Solution

The final step is to isolate the short-term interest rate $r_t$. To do this, we multiply the entire equation by $e^{-at}$:

$$r_t = e^{-at} \left( r_0 + b \left( e^{at} - 1 \right) + \int_0^t \sigma e^{as} dW_s \right)$$

Now, distribute the $e^{-at}$ factor to each of the three terms inside:

1. **First term:** $r_0 \cdot e^{-at} = r_0 e^{-at}$

2. **Second term:** $b \left( e^{at} - 1 \right) \cdot e^{-at} = b \left( e^{at} e^{-at} - e^{-at} \right) = b \left( 1 - e^{-at} \right)$

3. **Third term:** $e^{-at} \int_0^t \sigma e^{as} dW_s = \int_0^t \sigma e^{-a(t-s)} dW_s$

Putting it all together gives the **famous closed-form solution of the Vasicek model**:

$$\boxed{r_t = r_0 e^{-at} + b \left( 1 - e^{-at} \right) + \int_0^t \sigma e^{-a(t-s)} dW_s}$$

---

**See also:** [[22.1 Properties of Ito Integral And the O-U Process.]] · [[26 Statistical Distribution of the Vasicek Model]] · [[24 Long-Term Statistical Distribution Of the O-U Process]]
