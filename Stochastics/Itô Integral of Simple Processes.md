The Itô Integral**. While standard calculus deals with smooth, predictable curves, stochastic calculus deals with randomness, specifically Brownian motion (think of a completely unpredictable, jagged random walk).

Because Brownian motion is so erratic, standard rules for finding the "area under the curve" (integration) don't work. This page explains how to build a new kind of integral from the ground up, starting with the easiest possible case: a **simple, non-random process**.

Here is a breakdown of the three main concepts on this page.

### 1. What is a "Simple Process"? 
Before integrating complex random functions, the text starts with a "dummy" function called a simple process, denoted as $X(t)$.

- **Non-random:** Its values are predetermined; there are no surprises.
- **Simple:** It’s just a step function. Imagine a staircase.
- **The Math:**$X(t) = c_0 I_0(t) + \sum_{i=0}^{n-1} c_i I_{(t_i, t_{i+1}]}(t)$, is just a fancy way of saying: "Between time 0 and time 1, the value is constant $c_0$. Between time 1 and time 2, the value is constant $c_1$," and so on.

### 2. Defining the Itô Integral (Equation 4.2)

In regular calculus, an integral is roughly the height of a function multiplied by a tiny step in time ($dt$).
In Itô calculus, the integral $\int_0^T X(t) dB(t)$ is the height of our step function multiplied by a tiny step in **Brownian motion** ($dB(t)$).

The Equation shows exactly how to calculate this for our staircase function:

$$\int_0^T X(t) dB(t) = \sum_{i=0}^{n-1} c_i (B(t_{i+1}) - B(t_i))$$

- $c_i$ is the constant height of our step function during a specific time chunk.
- $(B(t_{i+1}) - B(t_i))$ is the **increment**—how much the random Brownian motion moved up or down during that exact same time chunk.
- You simply multiply them together for each step and add them all up.
### 3. The Variance.

Because the integral is built by adding up pieces of Brownian motion (which are random), the final result of the integral isn't a single set number—it is a **random variable** itself. Specifically, it's a "Gaussian" (normal, bell-curve) random variable.

The bottom section calculates how spread out (the variance) this random integral will be.

1. **Mean Zero:** Because Brownian motion is just as likely to go up as it is down, its average change is zero. Therefore, the average (mean) of our entire integral is zero.

2. **Independence:** The core trick is that changes in Brownian motion over non-overlapping time steps are completely independent. What happens between $t_1$ and $t_2$ doesn't care about what happened between $t_0$ and $t_1$.

3. **The Calculation:** Because they are independent, the total variance is just the sum of the individual variances of each step.

4. **The Result:** The variance of a standard Brownian step is simply the time elapsed $(t_{i+1} - t_i)$. So, the variance of our step $c_i \Delta B$ becomes $c_i^2 (t_{i+1} - t_i)$.


Adding them all up gives the final formula at the bottom of the page. This result is incredibly important and is often called the **Itô Isometry**. It essentially links the messy world of stochastic integrals back to standard, predictable integrals.
