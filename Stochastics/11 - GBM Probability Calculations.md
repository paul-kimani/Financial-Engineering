# 11 — GBM Probability Calculations

> Using the log‑normal distribution of $S_t$ to answer two practical
> questions: *what is the probability that the share price exceeds a target
> value?* and *what is a confidence interval for the share price at a future
> date?*

The distributional fact we will use repeatedly (from
[[10 - Geometric Brownian Motion]]):

$$
\ln S_t \;\sim\; \mathcal{N}\!\left(\ln S_0 + \bigl(\mu - \tfrac{1}{2}\sigma^2\bigr) t,\; \sigma^2 t\right).
$$

---

## 1. Determining the probability of hitting a target

Sometimes we want to know the probability that the price of the share will
reach a particular value. For instance, an investor in **call options** may
want to know the chance that a call option will be exercised.

Because the share follows a log‑normal distribution, the cleanest approach
is to convert to a standard normal $Z$ via

$$
Z \;=\; \frac{X - \mu_{\ln S_t}}{\sigma_{\ln S_t}},
$$

then read a probability off the standard normal tables.

### Worked example — probability that $S_t > 45$

**Setup.** Annual parameters $\mu = 25\%$ and $\sigma = 20\%$ are used to
model a company's share. The current price is $S_0 = 38$. Find the
probability that the share price exceeds $45$ in $4$ months
($t = 1/3$ year).

**Step 1 — parameters of $\ln S_t$.**

$$
\mu_{\ln S_t} \;=\; \ln S_0 + \bigl(\mu - \tfrac{1}{2}\sigma^2\bigr) t \;=\; \ln 38 + \bigl(0.25 - \tfrac{1}{2}(0.2)^2\bigr)\cdot\tfrac{1}{3} \;\approx\; 3.7142.
$$

$$
\sigma_{\ln S_t}^2 \;=\; \sigma^2 t \;=\; (0.2)^2 \cdot \tfrac{1}{3} \;\approx\; 0.0133.
$$

So $\ln S_t \sim \mathcal{N}(3.7142,\;0.0133)$.

**Step 2 — convert to $Z$.**

$$
\mathbb{P}\bigl[S_t > 45\bigr] \;=\; 1 - \mathbb{P}\bigl[S_t \le 45\bigr] \;=\; 1 - \mathbb{P}\!\left[Z \le \frac{\ln 45 - 3.7142}{\sqrt{0.0133}}\right].
$$

Compute the standardised threshold:

$$
\frac{\ln 45 - 3.7142}{\sqrt{0.0133}} \;\approx\; \frac{3.8067 - 3.7142}{0.1153} \;\approx\; 0.80.
$$

**Step 3 — read the standard normal table.**

$$
\mathbb{P}\bigl[S_t > 45\bigr] \;=\; 1 - \Phi(0.80) \;\approx\; 1 - 0.7881 \;=\; \boxed{\,0.2119\,}.
$$

So roughly a **21% chance** the share price exceeds $45$ in $4$ months.

---

## 2. Confidence interval for the future share price

Given some of the unrealistic assumptions of GBM, it may not be reliable to
predict the *exact* future share price. However, we can estimate the
**predicted range** at a chosen confidence level — extremely useful for
risk assessment.

### Worked example — 95% confidence interval for $S_t$

**Setup.** Same parameters as before: $\mu = 25\%$, $\sigma = 20\%$,
$S_0 = 38$, $t = 4 \text{ months} = 1/3 \text{ year}$. Find the **95%
confidence interval** for the share price at $t = 4$ months.

We want bounds $A < S_t < B$ with $\mathbb{P}[A < S_t < B] = 0.95$. With
$\alpha = 5\%$ in each tail of size $\alpha/2 = 2.5\%$, the standard normal
critical values are $\pm z_{0.025} = \pm 1.96$.

**Step 1 — parameters of $\ln S_t$.** As before,

$$
\ln S_t \sim \mathcal{N}(3.7142,\;0.0133).
$$

**Step 2 — lower bound $A$.** Solve
$\mathbb{P}\bigl[S_t \le A\bigr] = 0.025$:

$$
\mathbb{P}\!\left[Z \le \frac{\ln A - 3.7142}{\sqrt{0.0133}}\right] = 0.025
\quad\Longrightarrow\quad
\frac{\ln A - 3.7142}{\sqrt{0.0133}} = -1.96.
$$

Solving for $A$:

$$
\ln A = 3.7142 - 1.96\cdot\sqrt{0.0133} \;\approx\; 3.7142 - 0.2260 \;\approx\; 3.4882,
$$

$$
A \;=\; e^{3.4882} \;\approx\; \mathbf{32.73}.
$$

**Step 3 — upper bound $B$.** Solve
$\mathbb{P}\bigl[S_t \le B\bigr] = 0.975$ (i.e. $+1.96$ on the right):

$$
\frac{\ln B - 3.7142}{\sqrt{0.0133}} = +1.96
\quad\Longrightarrow\quad
\ln B \approx 3.7142 + 0.2260 \approx 3.9402,
$$

$$
B \;=\; e^{3.9402} \;\approx\; \mathbf{51.43}.
$$

**Step 4 — state the interval.**

$$
\boxed{\;\mathbb{P}\bigl[\,32.73 \;<\; S_t \;<\; 51.43\,\bigr] \;=\; 0.95.\;}
$$

---

## 3. Closed‑form confidence interval

Steps 2–3 can be packaged into a single closed‑form expression. Define

$$
m \;=\; \bigl(\mu - \tfrac{1}{2}\sigma^2\bigr)\,t.
$$

Then the $(1-\alpha)$ confidence interval for $S_t$ is

$$
\boxed{\;\mathrm{CI}_{1-\alpha} \;=\; \Bigl[\,S_0\,e^{\,m \,-\, z_{\alpha/2}\,\sigma\sqrt{t}}\;,\;\;S_0\,e^{\,m \,+\, z_{\alpha/2}\,\sigma\sqrt{t}}\,\Bigr].\;}
$$

### Verification on the worked example

- $m = (0.25 - 0.02)\cdot \tfrac{1}{3} \approx 0.0767$.
- $\sigma\sqrt{t} = 0.2 \cdot \sqrt{1/3} \approx 0.1155$.
- $z_{0.025} = 1.96$.

Lower bound: $38 \cdot e^{0.0767 - 1.96\cdot 0.1155} = 38 \cdot e^{-0.1497} \approx 38 \cdot 0.861 \approx 32.72.$
Upper bound: $38 \cdot e^{0.0767 + 1.96\cdot 0.1155} = 38 \cdot e^{0.3031} \approx 38 \cdot 1.354 \approx 51.43.$ ✓

The closed form matches the step‑by‑step calculation.

---

## 4. Quick checklist

When you face a "probability/CI under GBM" problem:

1. Compute $\mu_{\ln S_t} = \ln S_0 + (\mu - \tfrac{1}{2}\sigma^2) t$ and
   $\sigma_{\ln S_t}^2 = \sigma^2 t$.
2. Standardise: $Z = (\ln K - \mu_{\ln S_t}) / \sigma_{\ln S_t}$.
3. Use $\Phi(\cdot)$ (the standard normal CDF) and a $Z$-table.
4. For a CI, plug into the closed‑form box above.

---

## Cross‑references

- [[10 - Geometric Brownian Motion]] — derivation of the log‑normal
  distribution that drives every formula on this page.
- [[12 - GBM Parameter Estimation]] — where the inputs $\mu$ and $\sigma$
  come from when working with historical data.
- [[../Derivatives/]] — the same probability calculation is the building
  block for option payoff expectations.
