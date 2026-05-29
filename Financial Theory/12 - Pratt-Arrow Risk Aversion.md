# 12 — Pratt-Arrow Risk Aversion

> Curvature of $U(\cdot)$ tells you *whether* an investor is risk-averse
> (chapter [[11 - Risk Aversion - Certainty Equivalent and Risk Premium]]).
> The **Pratt-Arrow** apparatus tells you *how much*. Two indices —
> **ARA** for absolute risk aversion and **RRA** for relative risk
> aversion — emerge from a Taylor expansion of $U$ around the expected
> wealth and turn risk premiums into formulas.

---

## 1. The Taylor-expansion derivation

Let $W$ be a random wealth with mean $\mu$ and variance $\sigma_W^2$.
Let $c$ be the cost of the risk: $W_{\text{certainty equivalent}} = \mu - c$.

By definition of the certainty equivalent,

$$
U(\mu - c) \;=\; \mathbb{E}\bigl[U(W)\bigr].
$$

Expand the left-hand side around $\mu$ to first order in $c$ (assumed
small):

$$
U(\mu - c) \;\approx\; U(\mu) - U'(\mu)\,c.
$$

Expand $U(W)$ around $\mu$ to second order in $W - \mu$:

$$
U(W) \;\approx\; U(\mu) + U'(\mu)\,(W - \mu) + \tfrac{1}{2}\,U''(\mu)\,(W - \mu)^2.
$$

Take expectations. Since $\mathbb{E}[W - \mu] = 0$ and
$\mathbb{E}[(W - \mu)^2] = \sigma_W^2$:

$$
\mathbb{E}\bigl[U(W)\bigr] \;\approx\; U(\mu) + \tfrac{1}{2}\,U''(\mu)\,\sigma_W^2.
$$

Equating the two sides and cancelling $U(\mu)$:

$$
- U'(\mu)\,c \;\approx\; \tfrac{1}{2}\,U''(\mu)\,\sigma_W^2,
$$

which solves for the **cost of risk**:

$$
\boxed{\;c \;\approx\; -\,\tfrac{1}{2}\,\frac{U''(\mu)}{U'(\mu)}\,\sigma_W^2.\;}
$$

The coefficient of $\sigma_W^2 / 2$ that emerges is the **Pratt-Arrow
Absolute Risk Aversion**.

---

## 2. Absolute Risk Aversion (ARA)

$$
\boxed{\;\text{ARA}(W) \;=\; -\,\frac{U''(W)}{U'(W)}.\;}
$$

Properties.

- **Positive** for risk-averse investors ($U'' < 0$, $U' > 0$).
- **Zero** for risk-neutral.
- **Negative** for risk-loving.

Plugging into the cost-of-risk formula:

$$
c \;\approx\; \tfrac{1}{2}\,\text{ARA}(\mu)\,\sigma_W^2.
$$

So **the risk premium scales linearly with variance, with ARA as the
slope**.

### Decreasing ARA

Empirically and theoretically reasonable assumption: as wealth
increases, the *same* absolute loss bites less. ARA should
**decrease** with $W$:

$$
\frac{d\,\text{ARA}(W)}{dW} \;<\; 0.
$$

A wealthier investor accepts the same absolute gamble for a smaller
risk premium.

---

## 3. Relative Risk Aversion (RRA)

For **multiplicative** (proportional) gambles — losing 10% of wealth
rather than losing $10 — the right local measure is the **Relative
Risk Aversion**:

$$
\boxed{\;\text{RRA}(W) \;=\; -\,W\,\frac{U''(W)}{U'(W)} \;=\; W \cdot \text{ARA}(W).\;}
$$

The relative cost of risk is

$$
\frac{c}{\mu} \;\approx\; \tfrac{1}{2}\,\text{RRA}(\mu)\,\frac{\sigma_W^2}{\mu^2}
\;=\; \tfrac{1}{2}\,\text{RRA}(\mu)\,(\text{coefficient of variation})^2.
$$

The relative cost of risk is **directly proportional to the square of
the coefficient of variation**.

### Constant RRA

It is empirically common to assume **constant RRA** — a proportional
loss feels equally bad regardless of wealth. The canonical examples
are the **power utility** family

$$
U(W) \;=\; \frac{W^{1 - \gamma}}{1 - \gamma}, \qquad \gamma > 0, \;\gamma \neq 1,
$$

for which $\text{RRA}(W) = \gamma$ everywhere, and the **log utility**
$U(W) = \ln W$ (the $\gamma = 1$ limit), for which $\text{RRA}(W) = 1$.

---

## 4. ARA, RRA and common utility families

| Utility family              | $U(W)$                                                | ARA                          | RRA                          |
| :-------------------------- | :---------------------------------------------------- | :--------------------------- | :--------------------------- |
| **Linear (risk neutral)**   | $aW + b$                                              | $0$                          | $0$                          |
| **Quadratic**               | $aW - \tfrac{1}{2} b W^2$                             | $\dfrac{b}{a - bW}$ (increasing) | $\dfrac{bW}{a - bW}$         |
| **Log utility**             | $\ln W$                                               | $\dfrac{1}{W}$ (decreasing)  | $1$ (constant)               |
| **Power / CRRA**            | $\dfrac{W^{1 - \gamma}}{1 - \gamma}$                  | $\dfrac{\gamma}{W}$          | $\gamma$ (constant)          |
| **Exponential / CARA**      | $1 - e^{-\lambda W}$                                  | $\lambda$ (constant)         | $\lambda W$                  |

Read the ARA column: **log and power utilities have decreasing ARA**
(consistent with the empirical observation that wealthier investors
take on more absolute risk). **Quadratic** has *increasing* ARA — a
classic argument against using it despite its mean-variance
convenience.

---

## 5. Worked example — $20{,}000 wealth, log utility, two gambles

An investor has $U(W) = \ln W$ and current wealth $W = \$20{,}000$.

### Gamble A — symmetric, small stakes

$50/50$ chance of winning or losing $\$10$. So $W_A \in \{19{,}990,\;
20{,}010\}$ each with probability $0.5$.

**Expected wealth:** $\mathbb{E}[W_A] = 20{,}000$.

**Variance:** $\sigma_A^2 = 0.5 \cdot 10^2 + 0.5 \cdot 10^2 = 100$.

**ARA at $\mu = 20{,}000$:** $1 / 20{,}000 = 5 \times 10^{-5}$.

**Risk premium (approximation):**

$$
\pi_A \;\approx\; \tfrac{1}{2}\,\text{ARA}(\mu)\,\sigma_A^2
\;=\; \tfrac{1}{2} \cdot 5 \times 10^{-5} \cdot 100 \;=\; 0.0025.
$$

Quarter of a cent — negligible. The investor will essentially accept
a fair coin-flip for $\$10$.

### Gamble B — asymmetric, big stakes

$80\%$ chance of winning $\$1{,}000$; $20\%$ chance of losing $\$10{,}000$.

**Expected wealth:**

$$
\mathbb{E}[W_B] \;=\; 0.8 \cdot 21{,}000 + 0.2 \cdot 10{,}000 \;=\; 16{,}800 + 2{,}000 \;=\; 18{,}800.
$$

**Variance** of $W_B - \mathbb{E}[W_B]$:

$$
\sigma_B^2 \;=\; 0.8 \cdot (21{,}000 - 18{,}800)^2 + 0.2 \cdot (10{,}000 - 18{,}800)^2
\;=\; 0.8 \cdot 2200^2 + 0.2 \cdot (-8800)^2
\;=\; 19{,}360{,}000.
$$

**ARA at $\mu = 18{,}800$:** $1 / 18{,}800 \;\approx\; 5.32 \times 10^{-5}$.

**Risk premium (approximation):**

$$
\pi_B \;\approx\; \tfrac{1}{2} \cdot 5.32 \times 10^{-5} \cdot 19{,}360{,}000 \;\approx\; 514.85.
$$

The investor would pay up to $\approx \$515$ to avoid Gamble B (the
exact answer differs slightly because the variance is no longer
"small"; the Taylor approximation degrades). Compare with the
**negative expected return** of Gamble B itself: $\mathbb{E}[W_B] -
20{,}000 = -1{,}200$. So a rational log-utility investor would
**definitely** decline Gamble B.

---

## 6. The big picture

| Measure | Sensitive to                              | Constant for                  | Empirical preference                           |
| :------ | :---------------------------------------- | :---------------------------- | :--------------------------------------------- |
| ARA     | **Absolute** wealth changes ($\Delta W$). | Exponential / CARA utility.   | Should **decrease** with wealth.               |
| RRA     | **Proportional** wealth changes.          | Log / Power / CRRA utility.   | Often assumed **constant** with wealth.        |

Together with chapter
[[11 - Risk Aversion - Certainty Equivalent and Risk Premium]], the
Pratt-Arrow machinery turns the qualitative statement "the investor
is risk-averse" into quantitative pricing of risk that flows directly
into the CAPM and other equilibrium models discussed in
[[01 - Models in Financial Economics]].

---

## Cross‑references

- [[11 - Risk Aversion - Certainty Equivalent and Risk Premium]] — the
  global risk-premium definitions this chapter localises.
- [[10 - VNM Axioms and Utility Function]] — the utility function
  whose curvature is being measured.
- [[09 - Expected Utility Theory]] — the motivation for using
  $U(\cdot)$ at all.
- [[01 - Models in Financial Economics]] — CAPM and other equilibrium
  models built on top of risk-averse expected utility.
- [[../QUANTFRAME/Concepts/BackGround to Kelly Criterion]] — the
  Kelly fraction $f^\ast = p - q$ (or $f^\ast = (bp - q)/b$) is the
  optimal-bet companion to risk aversion under log utility (RRA = 1).
- [[../Stochastics/10 - Geometric Brownian Motion]] — the Itô drag
  $\tfrac{1}{2}\sigma^2$ is the continuous-time analogue of the
  variance-times-ARA risk premium derived here.
- [[../Stochastics/12 - GBM Parameter Estimation]] — empirical
  $\sigma$ from log returns is the input to risk-premium calculations
  in practice.
