# 02 — The Two-Period and n-Period BOPM

> A one-period hedge is set once and held. A two-period hedge is **adjusted**
> after the first toss, using whatever new information the toss revealed.
> Pricing the option means solving the replicating-portfolio equations
> backward through the tree, one period at a time — which turns out to
> collapse into the same risk-neutral expectation from Chapter 1, applied
> recursively. The reward for seeing this is the general $n$-period formula,
> a direct binomial-expansion sum.

---

## 1. Dynamic hedging

In a two-period model you set your hedge at time 0, observe the first toss,
then **re-hedge**: hold a new number of shares $\Delta_1$, with the
remaining wealth $(X_1 - \Delta_1 S_1)$ invested at the risk-free rate.
Wealth evolves as

$$X_2 = \Delta_1 S_2 + (1+r)\big[X_1 - \Delta_1 S_1\big].$$

The tree now has four terminal states:

$$S_2(HH) = u^2S_0, \quad S_2(HT) = S_2(TH) = udS_0, \quad S_2(TT) = d^2S_0.$$

## 2. Setting up the equations

Because the adjusted hedge $\Delta_1$ depends on what happened at time 1,
there are two *separate* systems of equations — one branch for each first
toss.

**If the first toss was heads:**

$$C_2(HH) = \Delta_1(H)\,S_2(HH) + (1+r)\big[X_1(H) - \Delta_1(H)S_1(H)\big] \qquad (1)$$
$$C_2(HT) = \Delta_1(H)\,S_2(HT) + (1+r)\big[X_1(H) - \Delta_1(H)S_1(H)\big] \qquad (2)$$

**If the first toss was tails:**

$$C_2(TH) = \Delta_1(T)\,S_2(TH) + (1+r)\big[X_1(T) - \Delta_1(T)S_1(T)\big] \qquad (3)$$
$$C_2(TT) = \Delta_1(T)\,S_2(TT) + (1+r)\big[X_1(T) - \Delta_1(T)S_1(T)\big] \qquad (4)$$

## 3. Solving for the adjusted hedge ratios

Exactly as in Chapter 1, subtract each pair to eliminate the cash/bond term:

$$\Delta_1(H) = \frac{C_2(HH) - C_2(HT)}{S_2(HH) - S_2(HT)}, \qquad \Delta_1(T) = \frac{C_2(TH) - C_2(TT)}{S_2(TH) - S_2(TT)}.$$

## 4. Valuing the portfolio at time 1

Because the portfolio replicates the option, its value equals the option's
value at every node: $C_1(H)=X_1(H)$ and $C_1(T)=X_1(T)$. There is no need
to redo the messy algebra of Chapter 1 — the risk-neutral pricing formula
applies directly at each node:

$$X_1(H) = C_1(H) = \frac{1}{1+r}\Big[\tilde p\,C_2(HH) + \tilde q\,C_2(HT)\Big],$$
$$X_1(T) = C_1(T) = \frac{1}{1+r}\Big[\tilde p\,C_2(TH) + \tilde q\,C_2(TT)\Big].$$

**The key idea.** Each node at $t=1$ is a *fresh one-period BOPM*: from
$S_1(H)$ the stock goes to $uS_1(H)$ or $dS_1(H)$, and the option to
$C_2(HH)$ or $C_2(HT)$. So the Chapter 1 formula applies verbatim, just
re-indexed one step forward. $\tilde p,\tilde q$ are the same at every
node because they depend only on $u,d,r$, which are constant across
periods.

> **Watch the discount factor.** Every step back through the tree is one
> period of discounting. Writing $C_1(H) = \tilde p\,C_2(HH) + \tilde q\,C_2(HT)$
> (without the $\tfrac{1}{1+r}$) compares a time-2 amount to a time-1
> amount — it mixes money at different dates. Likewise the $n$-period sum
> in §6 needs $(1+r)^{-n}$, one factor per period.

## 5. The full two-period formula

Substitute both into the single-period formula $C_0 = \frac{1}{1+r}[\tilde p\,C_1(H) + \tilde q\,C_1(T)]$:

$$C_0 = \frac{1}{1+r}\left[\tilde p\left(\frac{1}{1+r}\big[\tilde p\,C_2(HH)+\tilde q\,C_2(HT)\big]\right) + \tilde q\left(\frac{1}{1+r}\big[\tilde p\,C_2(TH)+\tilde q\,C_2(TT)\big]\right)\right]$$

$$= (1+r)^{-2}\Big[\tilde p^2 C_2(HH) + \tilde p\tilde q\,C_2(HT) + \tilde q\tilde p\,C_2(TH) + \tilde q^2 C_2(TT)\Big].$$

Since $C_2(HT) = C_2(TH)$ (the tree recombines), the two middle terms
combine:

$$\boxed{C_0 = (1+r)^{-2}\Big[\tilde p^2\,C_2(HH) + 2\tilde p\tilde q\,C_2(HT) + \tilde q^2\,C_2(TT)\Big]}$$

This mirrors the binomial expansion $(\tilde p+\tilde q)^2 = \tilde p^2 + 2\tilde p\tilde q + \tilde q^2$.
The coefficients $(1,2,1)$ are the number of paths reaching each terminal
state: 1 path to $HH$, 2 paths to one head and one tail, 1 path to $TT$.

## 6. Generalization: the n-period BOPM

Repeatedly applying backward induction from the final period $n$ back to
time 0 gives the general binomial option pricing formula:

$$\boxed{C_0 = (1+r)^{-n}\sum_{j=0}^{n}\binom{n}{j}\,\tilde p^{\,j}\,\tilde q^{\,n-j}\,C_n(j)}$$

Reading each piece:

- $(1+r)^{-n}$ — discounts the period-$n$ payoff back to today.
- $\sum_{j=0}^n$ — sums over every terminal state; $j$ is the number of
  up-moves.
- $\binom{n}{j}$ — the number of distinct paths through the tree with
  exactly $j$ up-moves and $n-j$ down-moves.
- $\tilde p^{\,j}\tilde q^{\,n-j}$ — the risk-neutral probability of any one
  specific path with $j$ up-moves and $n-j$ down-moves.

This is precisely $C_0 = (1+r)^{-n}\,\tilde{\mathbb E}_{\mathbb Q}[C_n]$, the
risk-neutral pricing formula proved in general in
[[03 - FTAP, Risk-Neutral Measure and Martingales]].

## 7. The continuous-time limit (worked live, 2026-09-28)

**Setup (Cox-Ross-Rubinstein).** Split $[0,T]$ into $N$ steps of length
$\Delta t = T/N$ and set

$$u = e^{\sigma\sqrt{\Delta t}}, \qquad d = e^{-\sigma\sqrt{\Delta t}}, \qquad 1+r \to e^{r\Delta t}.$$

**Why $\sqrt{\Delta t}$.** Each log-step is $\pm\sigma\sqrt{\Delta t}$, with
variance $\sigma^2\Delta t$; over $N$ independent steps total variance is
$N\sigma^2\Delta t = \sigma^2 T$, fixed as $N\to\infty$. Steps of
$\pm\sigma\Delta t$ would give total variance $\sigma^2T\Delta t \to 0$ —
the randomness would vanish. This is the discrete root of Brownian motion
scaling like $\sqrt t$.

**Step 1 — $\tilde p$ to first order.** Using $e^x \approx 1+x+\tfrac12x^2$
and keeping terms to order $\Delta t$:

$$\text{num} = e^{r\Delta t} - e^{-\sigma\sqrt{\Delta t}} \approx \sigma\sqrt{\Delta t} + \left(r-\tfrac12\sigma^2\right)\Delta t,$$

$$\text{den} = e^{\sigma\sqrt{\Delta t}} - e^{-\sigma\sqrt{\Delta t}} \approx 2\sigma\sqrt{\Delta t}\quad(\text{the } \tfrac12\sigma^2\Delta t \text{ terms cancel}),$$

$$\tilde p \approx \frac12 + \frac{\left(r-\tfrac12\sigma^2\right)\sqrt{\Delta t}}{2\sigma}.$$

The coin becomes fair as $\Delta t\to0$; all the drift lives in a tilt of
order $\sqrt{\Delta t}$.

**Step 2 — mean and variance of one log-step.** $\xi_k = \ln(S_{k+1}/S_k) = \pm\sigma\sqrt{\Delta t}$:

$$\tilde{\mathbb E}[\xi_k] = \sigma\sqrt{\Delta t}\,(2\tilde p-1) = \left(r-\tfrac12\sigma^2\right)\Delta t,$$

$$\widetilde{\mathrm{Var}}[\xi_k] = \underbrace{\sigma^2\Delta t}_{\xi_k^2 \text{ in both states}} - \underbrace{\left(r-\tfrac12\sigma^2\right)^2\Delta t^2}_{\text{negligible}} \approx \sigma^2\Delta t.$$

> **Slip to watch:** the per-step mean carries a $\Delta t$ —
> $\sqrt{\Delta t}\cdot\sqrt{\Delta t}$. Only after multiplying by
> $N = T/\Delta t$ does it become $\left(r-\tfrac12\sigma^2\right)T$.

**Step 3 — central limit theorem.** Summing $N$ i.i.d. steps:

$$\boxed{\ln\frac{S_T}{S_0} \xrightarrow{\;d\;} \mathcal N\!\left(\left(r-\tfrac12\sigma^2\right)T,\ \sigma^2T\right)\quad\text{under }\mathbb Q}$$

$S_T$ is **lognormal** — the Black-Scholes stock model is not an
assumption bolted on; it is what the binomial tree becomes. The hedge ratio
$\Delta_n$ becomes $\partial V/\partial S$, and the $n$-period sum becomes
$S_0\Phi(d_1) - Ke^{-rT}\Phi(d_2)$ (see
[[06 - Feynman-Kac and the BSM Formula]]).

**Step 4 — why $-\tfrac12\sigma^2$.** With $\mathbb E[e^X] = e^{\mu+\frac12 v}$
for $X\sim\mathcal N(\mu,v)$:

$$\tilde{\mathbb E}[S_T] = S_0\exp\!\left(\left(r-\tfrac12\sigma^2\right)T + \tfrac12\sigma^2T\right) = S_0e^{rT},$$

exactly as Theorem 1 of [[03 - FTAP, Risk-Neutral Measure and Martingales]]
demands. Had the log-drift been $rT$, then
$\tilde{\mathbb E}[e^{-rT}S_T] = S_0e^{\frac12\sigma^2T} > S_0$: the
discounted stock would drift up, fail to be a martingale, and by the FTAP
the prices would admit arbitrage.

**Intuition — Jensen's inequality.** $e^x$ is convex, so
$\mathbb E[e^X] > e^{\mathbb E[X]}$. Volatility lifts the average *price*
above $e^{\text{average log-return}}$; for the price to grow at exactly $r$,
the log-return must drift $\tfrac12\sigma^2$ *below* $r$.

**Where it reappears.**

- *Volatility drag:* $+50\%$ then $-50\%$ has arithmetic mean $0\%$ but
  ends at $0.75$. Geometric return $\approx$ arithmetic $-\tfrac12\sigma^2$.
  Same reason leveraged ETFs decay in choppy markets, and why cutting
  variance raises compound growth for a trading strategy at the same
  average return.
- *Mean vs median:* under $\mathbb Q$ the median of $S_T$ is
  $S_0e^{(r-\frac12\sigma^2)T}$, below the mean $S_0e^{rT}$ — most paths
  finish below average; a few large winners pull the mean up.
- *Itô's lemma:* $d\ln S_t = \left(r-\tfrac12\sigma^2\right)dt + \sigma\,dW_t$ —
  the $-\tfrac12\sigma^2$ is Itô's second-order term, the continuous
  version of this calculation. See [[05 - The PDE in Log-Price]].

---

**Next:** [[03 - FTAP, Risk-Neutral Measure and Martingales]] — why this
recipe works: the Fundamental Theorem of Asset Pricing.
