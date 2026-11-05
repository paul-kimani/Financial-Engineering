# 03 — Asset Returns

> Prices are what markets show you; returns are what you model. There are two
> kinds — simple and log — and each does one job the other cannot: **log
> returns add across time, simple returns add across assets.** The gap between
> them, about $R^2/2$ per period, is small on any single day and becomes the
> single most important number in compounding once it accumulates.

---

## 1. Simple (net) returns

The simple net return between $t-1$ and $t$ is the percentage change in
price:

$$
R_t=\frac{P_t-P_{t-1}}{P_{t-1}}
$$

The denominator is always the **previous** price. If the asset pays a
dividend $D_t$ during the period, add it to the numerator — it is cash
actually received:

$$
R_t=\frac{P_t+D_t-P_{t-1}}{P_{t-1}}
$$

The **gross return** is the multiplicative growth factor of your money:

$$
1+R_t=\frac{P_t}{P_{t-1}}
$$

### 1.1 Simple returns do not add across time

Gold at \$2,000 → \$2,200 → \$1,980:

$$
R_1=\frac{2200-2000}{2000}=0.10\qquad R_2=\frac{1980-2200}{2200}=\frac{-220}{2200}=-0.10
$$

Adding them gives $R_1+R_2=0$ — "you broke even." But the actual two-day
return is

$$
R_{0\to2}=\frac{1980-2000}{2000}=-0.01
$$

a **1% loss**. Chaining simple returns requires multiplying gross returns:

$$
(1+R_1)(1+R_2)-1=(1.10)(0.90)-1=-0.01\;✓
$$

**Why adding lies.** The −10% on day 2 is 10% of a *bigger* base (\$2,200),
so it removes \$220 — more than the \$200 the +10% added. Percentages taken
from different bases cannot simply be summed.

---

## 2. Log (continuously compounded) returns

$$
r_t=\ln(1+R_t)=\ln\!\left(\frac{P_t}{P_{t-1}}\right)=\ln P_t-\ln P_{t-1}
$$

### 2.1 Log returns add across time

Same prices:

$$
r_1=\ln\tfrac{2200}{2000}=\ln1.1\approx0.0953\qquad r_2=\ln\tfrac{1980}{2200}=\ln0.9\approx-0.1054
$$

$$
r_1+r_2=-0.0101=\ln0.99=\ln\tfrac{1980}{2000}\;✓
$$

**Why.** The two-day gross return is a product, and $\ln(ab)=\ln a+\ln b$:

$$
\ln\frac{P_2}{P_0}=\ln\!\left(\frac{P_1}{P_0}\cdot\frac{P_2}{P_1}\right)=\ln\frac{P_1}{P_0}+\ln\frac{P_2}{P_1}=r_1+r_2
$$

Over $T$ periods the sum **telescopes** — every intermediate price cancels:

$$
\sum_{t=1}^{T}r_t=(\ln P_1-\ln P_0)+(\ln P_2-\ln P_1)+\dots+(\ln P_T-\ln P_{T-1})=\ln P_T-\ln P_0
$$

A year of daily log returns sums to the year's log return. This is why
`diff(log(prices))` is the standard first line of return code: it is this
formula applied to a whole series at once.

**Honest about asymmetry.** A +10% simple move is $r=+0.0953$; a −10% simple
move is $r=-0.1054$. In log terms the fall is *larger* than the rise — which is
exactly why the round trip lost 1%. A $+x$ followed by a $-x$ **in log terms**
returns you precisely to the start.

### 2.2 Range

Simple returns have a floor at $R=-1$ (price falls to zero); log returns range
over all of $\mathbb R$, with $r\to-\infty$ as $P_t\to0$. For series that can
go *negative* (futures, rates, spreads), neither works — model dollar changes
instead (see [[02 Data-sets]] §4.2).

---

## 3. When does $\ln(1+R)\approx R$?

| $R$ | $r=\ln(1+R)$ | Gap $R-r$ | $R^2/2$ |
| :-: | :-: | :-: | :-: |
| 0.01 | 0.00995 | 0.00005 | 0.00005 |
| 0.10 | 0.0953 | 0.0047 | 0.0050 |
| −0.10 | −0.1054 | 0.0054 | 0.0050 |
| 0.50 | 0.4055 | 0.0945 | 0.1250 |
| −0.50 | −0.6931 | 0.1931 | 0.1250 |

**Where it comes from — the Taylor series:**

$$
\ln(1+R)=R-\frac{R^2}{2}+\frac{R^3}{3}-\dots\quad\Longrightarrow\quad R-r\approx\frac{R^2}{2}
$$

Three facts follow:

1. **The gap grows like $R^2$**, so the *relative* error is
   $\frac{R^2/2}{R}=\frac R2$. A 1% move: 0.5% of the return — negligible. A
   50% move: 25% of the return — useless.
2. **The gap is always positive:** $r<R$ in every row. The log return always
   sits below the simple return.
3. **The gap is larger for falls than for rises** (0.1931 at −50% vs 0.0945
   at +50%). The $+R^3/3$ term shrinks the gap for $R>0$ and widens it for
   $R<0$ — the same asymmetry as §2.1.

**Rule of thumb.** Daily moves (~1% for gold) — treat the two as
interchangeable. Crash days or annual figures (30–50%) — never quote a number
without saying which kind of return it is.

---

## 4. Simple returns add across assets

Split \$2,000 equally between A (+20%) and B (−10%).

- End value: $1000(1.20)+1000(0.90)=\$2{,}100$, a **5%** return.
- Weighted simple returns: $0.5(0.20)+0.5(-0.10)=0.05$ ✓ — exact.
- Weighted log returns: $0.5(0.1823)+0.5(-0.1054)=0.0385$, and
  $e^{0.0385}-1\approx3.93\%$ ✗ — falls short.

**Why.** A portfolio's value is a **sum** of dollar amounts. With
$w_A+w_B=1$,

$$
V_1=w_AV_0(1+R_A)+w_BV_0(1+R_B)=V_0\big(1+w_AR_A+w_BR_B\big)
\;\Longrightarrow\;
R_p=w_AR_A+w_BR_B
$$

exactly. The log of a sum does not split: $\ln(x+y)\neq\ln x+\ln y$.

| | Adds across **time** | Adds across **assets** |
| :--- | :-: | :-: |
| **Log returns** | ✓ (logs turn products into sums) | ✗ |
| **Simple returns** | ✗ (multiply gross returns) | ✓ (money adds linearly) |

**In practice (backtesting).** Combine several open positions on the *same*
day with position-weighted **simple** returns; convert that day's portfolio
return to $\ln(1+R_p)$ before chaining days into an equity curve. Mixing these
up is one of the most common bugs in backtest code.

---

## 5. Volatility drag

\$100 in an asset that returns +50% then −50%:

- Ending wealth $100\times1.5\times0.5=\$75$: total return **−25%**.
- Arithmetic mean: $\frac{0.50+(-0.50)}{2}=0$.
- Geometric mean $g$, the constant rate reaching the same wealth:
  $(1+g)^2=0.75\Rightarrow1+g=\sqrt{0.75}\approx0.866\Rightarrow g\approx-13.4\%$.
- Gap: $\text{AM}-\text{GM}=0-(-13.4\%)=13.4\%$, compared with
  $\sigma^2/2=0.5^2/2=12.5\%$.

$$
\boxed{\,g\approx\mu-\frac{\sigma^2}{2}\,}
$$

**Compound growth equals the average return minus half the variance.** (The
match is loose here because ±50% swings are large — the §3 breakdown again.)

**Where it comes from.** Per period, $r\approx R-R^2/2$ (§3). Averaging over
many periods, the mean of $R$ is $\mu$ and the mean of $R^2$ is roughly
$\sigma^2$ when $\mu$ is small. And the average log return *is* the log of the
compound growth factor:

$$
\frac{\ln1.5+\ln0.5}{2}=\frac{0.4055-0.6931}{2}=-0.1438=\ln(0.866)\;✓
$$

So the per-period $R^2/2$ gap, accumulated through time, **is** volatility
drag.

**Consequences.**

- Two strategies with the same average return do not grow the same; the more
  volatile one compounds to less.
- Average-return-per-trade overstates equity growth, which compounds at
  $\mu-\sigma^2/2$, not $\mu$.
- Leveraged ETFs decay in choppy markets even when the underlying ends flat.

### 5.1 Leverage and the growth-optimal fraction

A bot earns $\mu=0.05\%$ per day with $\sigma=1\%$. Leverage $L$ scales the
mean to $L\mu$ and the standard deviation to $L\sigma$:

$$
g(L)\approx L\mu-\frac{L^2\sigma^2}{2}
$$

With $\mu=0.0005$, $\sigma^2=0.0001$:

| $L$ | $L\mu$ | $L^2\sigma^2/2$ | $g(L)$ per day |
| :-: | :-: | :-: | :-: |
| 1 | 0.0005 | 0.00005 | **0.00045** |
| 3 | 0.0015 | 0.00045 | **0.00105** |
| 5 | 0.0025 | 0.00125 | **0.00125** |
| 10 | 0.0050 | 0.00500 | **0** |

Maximise: $g'(L)=\mu-L\sigma^2=0$, so

$$
L^*=\frac{\mu}{\sigma^2}=\frac{0.0005}{0.0001}=5,\qquad g(L^*)=\frac{\mu^2}{2\sigma^2}=0.00125
$$

$g(L)$ is a downward parabola with roots at $L=0$ and $L=2L^*=10$. **Beyond
$2L^*$, compound growth is negative even though every trade has positive
expected return** — the drag term, growing like $L^2$, overtakes the mean,
growing like $L$.

This is the continuous-time **Kelly criterion**. Practitioners commonly trade
a fraction of it ("half-Kelly"), because $\mu$ is estimated with large error,
and overestimating it pushes you up the steep side of the parabola.

---

## 6. The shape of the return distribution

### 6.1 Skewness and kurtosis

With $\mu=\mathbb E[X]$ and $\sigma^2=\operatorname{Var}(X)$:

$$
\text{Skewness}=\frac{\mathbb E\big[(X-\mu)^3\big]}{\sigma^3}\qquad\qquad\text{Kurtosis}=\frac{\mathbb E\big[(X-\mu)^4\big]}{\sigma^4}
$$

- The Normal distribution has skewness 0 and kurtosis 3. **Excess kurtosis**
  is kurtosis $-3$.
- Cubing keeps sign, so skewness measures which side the large deviations fall
  on. Raising to the fourth power makes kurtosis dominated by the *largest*
  deviations — it is a measure of **tail weight**.

**Skew in trading.**

- **Negative skew:** frequent small gains, rare large losses — equity indices,
  selling options, and many mean-reversion bots with a tight take-profit and a
  wide stop. Equity curves look smooth until they don't.
- **Positive skew:** frequent small losses, rare large gains — trend-following,
  breakout systems with trailing exits.

**Fat tails.** Daily asset returns typically show excess kurtosis well above
zero: extreme moves happen far more often than a Normal with the same variance
predicts. This is why "delete anything beyond $3\sigma$" is dangerous
([[01 The Data Life Cycle.]] §5.3), and why risk estimates that assume
Normality understate the chance of large losses.

### 6.2 Log returns are closer to Normal — but not Normal

Log returns remove the hard floor at −100% and are more symmetric than simple
returns, so they are *closer* to Normal. Over longer horizons, a period's log
return is a **sum** of many shorter log returns (§2.1), and sums of many pieces
tend toward Normality — so monthly returns look more Normal than daily ones.
Daily returns, however, remain clearly fat-tailed.

**Volatility clustering.** Large moves tend to follow large moves, of either
sign. Returns themselves show little correlation from day to day, but *squared*
returns are clearly correlated. Calm and turbulent regimes alternate, which
both fattens the tails of the overall distribution and slows the drift toward
Normality under aggregation.

### 6.3 Seeing the shape: the kernel density estimator

A histogram forces data into rigid bins, and shifting the bin edges changes the
picture. The **KDE** places a small smooth curve (the kernel) on every
observation and adds them up:

$$
\hat f(x)=\frac{1}{nb}\sum_{i=1}^{n}K\!\left(\frac{x-X_i}{b}\right)
$$

(The same bandwidth $b$ appears in the prefactor and inside the kernel — the
standalone [[Kernel Density Function]] note previously had $d$ inside the
kernel, a typo for $b$, now fixed.)

- $K$ is a density symmetric about 0, such as the standard Normal $N(0,1)$.
- $x-X_i$ centres one kernel on each observation.
- $b$, the **bandwidth**, is the smoothing dial: too small gives a spiky curve
  that chases every point; too large gives a blob that hides the tails and
  skew.

**Why the prefactor is $\frac{1}{nb}$.** Each scaled kernel integrates to 1.
Substituting $u=\frac{x-X_i}{b}$, so $dx=b\,du$:

$$
\int\frac1bK\!\left(\frac{x-X_i}{b}\right)dx=\int\frac1bK(u)\,b\,du=\int K(u)\,du=1
$$

The $\frac1b$ compensates for stretching the kernel horizontally by $b$, so
each bump keeps area 1. Dividing the sum of $n$ unit-area bumps by $n$ makes
the total area 1 — so $\hat f$ is a genuine density.

**Choosing $b$.** A common default is Silverman's rule of thumb,
$b=0.9\,\min\!\big(\hat\sigma,\ \text{IQR}/1.34\big)\,n^{-1/5}$. Using the
smaller of the two spread measures stops a few extreme values from inflating
$b$. With fat-tailed returns a single bandwidth is a compromise: one that
resolves the peak well leaves the sparse tails bumpy, and one that smooths the
tails flattens the peak.

**The practical comparison.** Overlay the KDE of a return series on a Normal
density with the same mean and standard deviation. Fat-tailed returns show the
characteristic pattern: a **taller, narrower peak**, **thinner shoulders**,
and **heavier tails** than the Normal.


---

## 7. Additions from the lecture notes (BSF 3216, §1.4)

### 7.1 Notation for many assets

The log returns of $N$ assets at time $t$ are collected in a vector
$\mathbf r_t\in\mathbb R^N$, with
$\mathbf r_t=\log\dfrac{\mathbf P_t}{\mathbf P_{t-1}}$ taken element by element.

### 7.2 Bond yield and price

A bond pays its **face (par) value** at maturity. The price $P$ paid to achieve
a yield of $i$ per annum is

$$
P=\text{PV at rate } i \text{ of net interest payments}+\text{PV at rate } i \text{ of net capital payments}
$$

i.e. the present value of everything the investor receives, net of tax.

**Zero-coupon bonds** ("zeros", pure discount bonds) pay no interest and no
principal until maturity, when they pay par. They therefore sell **below** par —
the discount is the investor's entire return.

### 7.3 Stylized facts of univariate return series

Empirical regularities found across many financial time series (McNeil, Frey &
Embrechts, *Quantitative Risk Management*):

1. Compound (log) returns are **not iid**, but show **low autocorrelation**.
2. **Squared and absolute** returns show **high autocorrelation**.
3. **Volatility changes over time** and clusters.
4. For a high threshold, **exceedances appear in clusters** — extremes come in
   bunches, not evenly spread.
5. Returns are **leptokurtic / heavy-tailed**.

Facts 1–3 are the formal version of §6.2's "returns look uncorrelated but
squared returns do not". Fact 4 is why a run of extreme days is more likely than
independent draws would suggest — relevant to stop placement and drawdown
estimates.

### 7.4 Multivariate stylized facts

For a vector of returns $\mathbf r_t$:

1. Little evidence of **serial cross-correlation**, except for
   **contemporaneous** returns (assets move together on the same day, but one
   asset's return today barely predicts another's tomorrow).
2. $|\mathbf r_t|$ series show **profound** serial cross-correlation
   (volatility spills over across assets and time).
3. **Correlations between contemporaneous returns vary over time.**
4. **Extreme returns in one series often coincide with extreme returns in
   several others** — diversification weakens exactly in crises.

### 7.5 Market invariants

**Market invariants** are market variables that can be modelled as realisations
of **independent and identically distributed** random variables $X_t$ — which
makes them "easy" to forecast and model. The quantity you model is the
invariant, not the raw price. Preferred invariants by asset class:

| Asset class | Invariant |
| :--- | :--- |
| Equities, commodities, forex | Log return $r_{t+\tau}$ |
| Fixed income | Change in yield to maturity, $X_{t+\tau}=Y_{t+\tau}-Y_t$ |
| Derivatives | Change in implied volatility, $X_{t+\tau}=\sigma_{t+\tau}-\sigma_t$ |

For fixed income, $Y_t=-\dfrac1v\log Z_t^{t+v}$ is the yield to maturity of a
zero-coupon bond with price $Z_t^{t+v}$ maturing at $t+v$, redeemed at par
(par $=1$).

**Why changes, not levels.** A bond's price is pulled to par as it ages and an
option's price decays with time, so neither price is identically distributed
over time; yields and implied volatilities are far more stable, and their
*changes* are approximately iid. This also matches [[02 Data-sets]] §4.2: for
quantities that are differences-meaningful rather than ratio-meaningful,
model changes.

### 7.6 Exploratory data analysis of a return series

A financial time series in discrete time $\{X_t\}_{t\in\mathbb N}$ is a sequence
of variables (prices or returns) observed at discrete points — end of each
trading day, last trading day of each month.

**Summary statistics:** sample mean, mode, median, variance, standard
deviation, skewness, kurtosis.

**Basic plots:**

- **Histogram** — a simple estimator of the density.
- **Kernel density estimator** — §6.3.
- **Boxplot** — the box spans the lower to upper quartile with a line at the
  median; whiskers extend to the most extreme points within about
  $1.5\times$IQR; points beyond are drawn individually. Useful for **comparing
  several samples** side by side (e.g. return distributions of several stocks or
  several years).

Histogram and KDE are visual estimators of the density of the data.

**Lecturer's example.** The NSE 20 Share Index price (2000–2016) wanders with
no fixed level, while its returns fluctuate around zero with occasional very
large spikes and calmer and wilder stretches — the stylized facts of §7.3 seen
directly.
