# 11 — Risk Aversion, Certainty Equivalent and Risk Premium

> The shape of the VNM utility $U(\cdot)$ encodes the investor's
> attitude to risk. Strictly **concave** → risk-averse; **linear** →
> risk-neutral; strictly **convex** → risk-loving. Risk aversion shows
> up quantitatively as a positive **risk premium** — the amount the
> investor is willing to pay to avoid an actuarially fair gamble.

---

## 1. Three risk preferences via utility curvature

Take a gamble $G$ with expected payoff $\mathbb{E}[W]$ and expected
utility $\mathbb{E}[U(W)]$. Compare two quantities:

- $U(\mathbb{E}[W])$ — the utility of the **certain** average payoff.
- $\mathbb{E}[U(W)]$ — the **expected utility** of the gamble itself.

Their relative size determines the investor's risk preference.

$$
\boxed{\;
\begin{aligned}
\text{Risk averse:} \quad & U(\mathbb{E}[W]) > \mathbb{E}[U(W)] \\
\text{Risk neutral:} \quad & U(\mathbb{E}[W]) = \mathbb{E}[U(W)] \\
\text{Risk loving:} \quad & U(\mathbb{E}[W]) < \mathbb{E}[U(W)]
\end{aligned}
\;}
$$

This is just **Jensen's inequality** applied to the utility function:

- $U$ strictly **concave** ($U'' < 0$) $\Rightarrow$ risk-averse.
- $U$ **linear** ($U'' = 0$) $\Rightarrow$ risk-neutral.
- $U$ strictly **convex** ($U'' > 0$) $\Rightarrow$ risk-loving.

So **risk aversion is exactly concavity of $U(\cdot)$**.

---

## 2. The certainty equivalent

Recall from chapter [[10 - VNM Axioms and Utility Function]] the
**certainty equivalent** of a gamble $G(x, z, p)$ — the certain amount
$\text{CE}$ such that

$$
U(\text{CE}) \;=\; \mathbb{E}[U(W)] \;=\; p\,U(x) + (1 - p)\,U(z).
$$

For a risk-averse investor, concavity gives
$U(\mathbb{E}[W]) > \mathbb{E}[U(W)]$, so $\mathbb{E}[W] > \text{CE}$:
the certainty equivalent is **strictly less than** the expected
payoff. The investor would accept a *smaller* sure amount in exchange
for giving up the gamble.

---

## 3. Risk premium and cost of risk

Two related quantities measure how much risk hurts.

| Quantity              | Definition                                                          | Plain meaning                                                                                |
| :-------------------- | :------------------------------------------------------------------ | :------------------------------------------------------------------------------------------- |
| **Risk premium**      | $\mathbb{E}[W] \;-\; \text{CE}$                                     | How much *less* than the expected payoff the investor would accept for certain.              |
| **Cost of the gamble**| $W_{\text{current}} \;-\; \text{CE}$                                | How much the investor would pay (out of current wealth) to walk away from the gamble entirely. |

The risk premium is sometimes called the **Markowitz risk premium**.

For a *risk-averse* investor both quantities are positive. For a
*risk-neutral* investor both are zero. For a *risk-loving* investor
both are negative (they would need to be *paid* to walk away).

---

## 4. Illustration I — log utility

Alex has log utility, $U(W) = \ln W$, and is offered the gamble

$$
G(5, 30; 0.80) \;\equiv\; \text{pays } 5 \text{ with prob } 0.80,\; 30 \text{ with prob } 0.20.
$$

**Actuarial (expected) value:**

$$
\mathbb{E}[W] \;=\; 0.80 \cdot 5 + 0.20 \cdot 30 \;=\; 4 + 6 \;=\; 10.
$$

**Expected utility:**

$$
\mathbb{E}[U(W)] \;=\; 0.80 \cdot \ln 5 + 0.20 \cdot \ln 30 \;\approx\; 1.97.
$$

**Certainty equivalent** — solve $\ln(\text{CE}) = 1.97$:

$$
\text{CE} \;=\; e^{1.97} \;\approx\; 7.17.
$$

**Risk premium:**

$$
\mathbb{E}[W] - \text{CE} \;=\; 10 - 7.17 \;=\; 2.83.
$$

Alex would pay up to $2.83$ to avoid the gamble.

**Is Alex risk-averse?** Yes — $U(W) = \ln W$ is strictly concave
($U'' = -1/W^2 < 0$), and we verified $U(\mathbb{E}[W]) = \ln 10
\approx 2.30 > 1.97 = \mathbb{E}[U(W)]$.

---

## 5. Illustration II — exponential utility

Ben has utility $U(W) = 1 - e^{-\lambda W}$ with $\lambda = 0.5$. This
is a classic **CARA** utility (constant absolute risk aversion — see
chapter [[12 - Pratt-Arrow Risk Aversion]]).

Given a gamble whose expected wealth is $\mathbb{E}[W] = 5$, the
expected utility works out to $\mathbb{E}[U(W)] = 0.495$. Solving
$1 - e^{-0.5 \cdot \text{CE}} = 0.495$ gives the **certainty
equivalent** $\text{CE} \approx 1.37$.

The investor would accept $W = 1.37$ for certain in lieu of a gamble
with expected wealth $W = 5$. The **risk premium** is

$$
\mathbb{E}[W] - \text{CE} \;=\; 5 - 1.37 \;=\; 3.63.
$$

Ben would pay up to $3.63$ to avoid the gamble. Compare to Alex
(log utility, less curved at the same wealth level): Ben is *more*
risk-averse. Chapter [[12 - Pratt-Arrow Risk Aversion]] formalises the
comparison via the Pratt–Arrow ARA index.

---

## 6. Recap — the chain of definitions

1. Start with a VNM utility function $U(\cdot)$ (chapter
   [[10 - VNM Axioms and Utility Function]]).
2. Take its expectation over a gamble's payoffs — that is the
   **expected utility** $\mathbb{E}[U(W)]$.
3. Invert $U$ at that level to get the **certainty equivalent**
   $\text{CE}$.
4. Subtract from the expected payoff to get the **risk premium**.
5. Subtract from current wealth to get the **cost of the gamble**.

Curvature of $U$ pins down the sign and size of the risk premium —
which the next chapter measures locally via the Pratt–Arrow ARA and
RRA indices.

---

## Cross‑references

- [[10 - VNM Axioms and Utility Function]] — the utility function and
  certainty-equivalent definition this chapter builds on.
- [[09 - Expected Utility Theory]] — why we use $U(\cdot)$ at all.
- [[12 - Pratt-Arrow Risk Aversion]] — local measures of risk aversion
  derived from $U''$ and $U'$.
- [[04 - Indifference Curves, MRS and Utility]] — diminishing marginal
  utility on the certainty side is exactly the concavity that drives
  risk aversion here.
- [[../QUANTFRAME/Concepts/BackGround to Kelly Criterion]] — log
  utility (used in Illustration I) is the canonical Kelly-betting
  utility.
- [[../Stochastics/10 - Geometric Brownian Motion]] — the
  $\tfrac{1}{2}\sigma^2$ Itô drag is the continuous-time analogue of
  the risk premium an investor *receives* for taking on diffusion
  risk.
- [[09.1 - Expected Utility theory Question]] · [[12.1 - Revision CAT1]] — worked problems classifying risk attitudes and computing CE / risk premium.
- [[../Stochastics/13 - Probability Measures]] — the risk premium seen on the pricing side, as the market price of risk.

