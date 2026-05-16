# 12 — GBM Parameter Estimation

> The probability and confidence‑interval formulas in
> [[11 - GBM Probability Calculations]] take $\mu$ and $\sigma$ as inputs.
> Here we show how to estimate them from historical price data — first as
> daily quantities, then annualised.

The GBM model has two parameters:

- $\mu$ — the **drift** (expected log return per unit time);
- $\sigma$ — the **volatility** (standard deviation of log returns per unit
  time).

Both can be estimated from a sample of consecutive closing prices.

---

## 1. The method — log returns

Given closing prices $S_1, S_2, \dots, S_n$ over $n$ consecutive trading
days, compute the **log returns**

$$
U_t \;=\; \ln\!\left(\frac{S_t}{S_{t-1}}\right), \qquad t = 2, 3, \dots, n.
$$

Under the GBM model these log returns are i.i.d.
$\mathcal{N}\!\bigl((\mu - \tfrac{1}{2}\sigma^2)\Delta t,\; \sigma^2 \Delta t\bigr)$,
where $\Delta t$ is the time between observations (e.g. $1$ day in our
units). Sample statistics of $\{U_t\}$ therefore translate directly into
estimates of $\mu$ and $\sigma$.

---

## 2. Estimating the drift

**Step 1 — mean log return.**

$$
\bar U \;=\; \frac{1}{n}\sum_{t=1}^{n} U_t.
$$

**Step 2 — daily drift.** Since
$\bar U \approx (\mu - \tfrac{1}{2}\sigma^2)\,\Delta t$, the daily drift
estimate is

$$
\hat\mu_{\text{daily}} \;=\; \bar U + \tfrac{1}{2}\,\hat\sigma^2_{\text{daily}}.
$$

(Note the $+\tfrac{1}{2}\sigma^2$ correction — it adds back the Itô drag
that the log return formula subtracted.)

**Step 3 — annualise.** With $252$ trading days per year,

$$
\boxed{\;\hat\mu_{\text{annual}} \;=\; 252 \cdot \hat\mu_{\text{daily}}.\;}
$$

---

## 3. Estimating the volatility

**Step 1 — sample variance of log returns.**

$$
S^2 \;=\; \frac{1}{n - 1}\!\left(\sum_{t=1}^{n} U_t^{\,2} - n\,\bar U^{\,2}\right).
$$

(Equivalent to the more familiar
$\tfrac{1}{n-1}\sum (U_t - \bar U)^2$ — Bessel's correction.)

**Step 2 — daily volatility.**

$$
\hat\sigma_{\text{daily}} \;=\; \sqrt{S^2}.
$$

**Step 3 — annualise.** Volatility scales with the *square root* of time
(variance scales linearly):

$$
\boxed{\;\hat\sigma_{\text{annual}} \;=\; \sqrt{252} \cdot \hat\sigma_{\text{daily}}.\;}
$$

---

## 4. Worked example

**Setup.** The closing prices of a share over the last $10$ trading days
were

$$
20.00,\; 20.10,\; 19.91,\; 20.01,\; 20.50,\; 20.25,\; 20.90,\; 20.90,\; 20.99,\; 20.71.
$$

Estimate the **annualised** mean and volatility, assuming $252$ trading
days per year.

### Step 1 — log returns ($n - 1 = 9$ values)

| Day | $S_{t-1}$ | $S_t$ | $U_t = \ln(S_t/S_{t-1})$    |
| :-: | :-------: | :---: | :-------------------------: |
| 2   | 20.00     | 20.10 | $\;\;+0.004988$             |
| 3   | 20.10     | 19.91 | $\;\;-0.009501$             |
| 4   | 19.91     | 20.01 | $\;\;+0.005010$             |
| 5   | 20.01     | 20.50 | $\;\;+0.024199$             |
| 6   | 20.50     | 20.25 | $\;\;-0.012270$             |
| 7   | 20.25     | 20.90 | $\;\;+0.031600$             |
| 8   | 20.90     | 20.90 | $\;\;0.000000$              |
| 9   | 20.90     | 20.99 | $\;\;+0.004298$             |
| 10  | 20.99     | 20.71 | $\;\;-0.013427$             |

### Step 2 — sample mean

$$
\bar U \;=\; \frac{1}{9}\!\bigl(0.00499 - 0.00950 + 0.00501 + 0.02420 - 0.01227 + 0.03160 + 0 + 0.00430 - 0.01343\bigr) \;\approx\; 0.00388.
$$

### Step 3 — sample standard deviation

Compute $\sum U_t^2 \approx 0.002250$. Then

$$
S^2 \;=\; \frac{1}{8}\bigl(0.002250 - 9\cdot(0.00388)^2\bigr) \;\approx\; \frac{1}{8}\bigl(0.002250 - 0.000136\bigr) \;\approx\; 0.0002643,
$$

so $\hat\sigma_{\text{daily}} \approx \sqrt{0.0002643} \approx 0.01626$
(i.e. about $1.63\%$ per day).

### Step 4 — daily drift

$$
\hat\mu_{\text{daily}} \;=\; \bar U + \tfrac{1}{2}\,\hat\sigma_{\text{daily}}^2 \;\approx\; 0.00388 + \tfrac{1}{2}\cdot 0.0002643 \;\approx\; 0.00401.
$$

### Step 5 — annualise

$$
\hat\mu_{\text{annual}} \;\approx\; 252 \cdot 0.00401 \;\approx\; \mathbf{1.011\;\,(101.1\%)}.
$$

$$
\hat\sigma_{\text{annual}} \;\approx\; \sqrt{252}\cdot 0.01626 \;\approx\; 15.87 \cdot 0.01626 \;\approx\; \mathbf{0.258\;\,(25.8\%)}.
$$

> The drift number is wildly large because we are working with a tiny
> 10‑day sample and a benign upward run — *real* drift estimates need
> hundreds or thousands of observations to be remotely stable. Volatility
> estimates converge much faster.

---

## 5. Practical caveats

A few things to keep in mind whenever you estimate GBM parameters from
real data:

- **Volatility is far more reliably estimated than drift.** A few months of
  data give a usable $\hat\sigma$; you typically need *years* of data
  before $\hat\mu$ stabilises.
- **Stationarity.** GBM assumes $\mu$ and $\sigma$ are constants. In real
  markets both change over time. Rolling‑window estimates or stochastic
  volatility models (e.g. Heston, see [[05 - Diffusion Processes Catalogue]])
  address this.
- **Time scaling.** Variance scales linearly with time; volatility scales
  with $\sqrt{t}$. This is the only reason $\sqrt{252}$ appears in the
  annualisation.
- **Overnight returns vs. intraday returns.** A "daily" log return
  computed close‑to‑close contains overnight news; some practitioners
  separate the two.
- **Outliers and jumps.** GBM has continuous paths and no jumps; large
  one‑day moves will inflate $\hat\sigma$ and bias inference. Robust
  estimators or jump‑diffusion models help.

---

## Cross‑references

- [[10 - Geometric Brownian Motion]] — distribution of $\ln(S_t/S_0)$ that
  these estimators target.
- [[11 - GBM Probability Calculations]] — the downstream use of $\hat\mu$
  and $\hat\sigma$.
- [[05 - Diffusion Processes Catalogue]] — more realistic models when GBM
  assumptions break down.
- [[../Time Series/]] — econometric techniques for parameter estimation.
