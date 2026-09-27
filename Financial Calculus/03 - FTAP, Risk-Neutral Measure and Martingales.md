# 03 — FTAP, Risk-Neutral Measure and Martingales

> The Fundamental Theorem of Asset Pricing (FTAP) is the reason the
> risk-neutral recipe from Chapters 1–2 always works: no-arbitrage is
> *equivalent* to the existence of a martingale measure, and a unique such
> measure is equivalent to every claim being hedgeable (market
> completeness). This chapter proves both halves for the BOPM: that the
> discounted stock price is a $\tilde{\mathbb Q}$-martingale, that the
> discounted replicating portfolio is too, and that the replicating
> portfolio always matches the option's payoff.

---

## 1. Assumptions of the BOPM

Each assumption is listed with **where it does work** in the proofs of
Chapters 1–3, and **what breaks** if it fails.

### A. Market assumptions — what replication needs

| Assumption | Where it does the work | If it fails |
|---|---|---|
| **Frictionless trading** — no transaction costs, taxes, bid-ask | Rebalancing $\Delta_n$ at every node is free, so $X_{n+1} = \Delta_n S_{n+1} + (1+r)(X_n - \Delta_n S_n)$ holds exactly | Each rebalance costs; as steps $\to\infty$ hedging cost can explode (Leland). Price becomes a band |
| **Borrow and lend at the same $r$** | The bond position $C_0 - \Delta_0 S_0$ is usually *negative* for a call — you borrow at the same $r$ you'd lend at | Two rates give two $\tilde p$'s, hence a price interval |
| **Unlimited short-selling** | $\Delta_n<0$ for puts; the no-arbitrage argument for $u<1+r$ shorts the stock | Puts can't be hedged; one side of $d<1+r<u$ can't be enforced |
| **Perfect divisibility** | $\Delta_0 = \frac{C_1(H)-C_1(T)}{S_0(u-d)}$ is almost never an integer | Replication is only approximate |
| **Price-taker / perfect liquidity** | Hedge trades don't move $S$ | Large hedgers move the price they hedge against (feedback) |
| **No dividends** (or known ones) | The stock's return is only its price move, giving $\tilde p u + \tilde q d = 1+r$ | With yield $\delta$: $\tilde p = \frac{(1+r)/(1+\delta) - d}{u-d}$; early exercise of American calls can become optimal |
| **Deterministic, constant $r$** | $(1+r)^{-n}$ pulls out of every conditional expectation | Stochastic rates; discounting becomes random |

### B. Model assumptions — what the tree itself assumes

1. **Exactly two outcomes per step.** This is completeness: 2 states, 2
   assets $\Rightarrow$ 2 equations, 2 unknowns $\Rightarrow$ unique
   $\Delta_0$ and unique $\mathbb Q$. With three branches (trinomial),
   Theorem 3 fails and $\mathbb Q$ is not unique.
2. **$u, d$ constant across time and nodes.** Gives (i) a recombining tree
   ($n+1$ terminal nodes, not $2^n$); (ii) the *same* $\tilde p$ at every
   node — used in [[02 - The Two-Period and n-Period BOPM]]; (iii) the
   binomial coefficients in the $n$-period formula. Economically:
   **constant volatility** — the limit is BSM's constant $\sigma$, and the
   implied-volatility smile is the evidence it is false.
3. **No-arbitrage, $d < 1+r < u$.** Used exactly once: to guarantee
   $\tilde p,\tilde q \in (0,1)$, i.e. that $\mathbb Q$ is a valid
   probability measure.
4. **Discrete trading dates.** Rebalancing happens only at nodes. Chapter 4
   replaces this with continuous trading.
5. **Every path has positive real-world probability**, $0<p<1$. This is
   the *only* requirement on $\mathbb P$. It makes $\mathbb P$ and
   $\mathbb Q$ **equivalent**: they agree on what is possible, disagreeing
   only on how likely. (If $p=1$ the stock is riskless, must earn $r$, and
   $d<1+r<u$ is impossible.)

### C. Investor assumptions

- **Non-satiation** (more is preferred to less) — the only preference
  assumption. It is why an arbitrage would be exploited, and hence why
  prices must exclude it.
- **Agreement on the states, not the probabilities** — everyone agrees on
  $u, d, r$; nobody needs to agree on $p$.

### What is *not* assumed

The price does **not** depend on the real-world probability $p$, the
stock's expected return, or investors' risk aversion. The model does not
assume investors are risk-neutral — it shows their preferences **cancel**,
because the option is priced *relative to* the stock, whose price $S_0$
already embeds whatever risk premium the market demands. "Risk-neutral
pricing" is a computational device, not a behavioural claim.

### Link forward

The BSM assumptions in [[04 - The Black-Scholes PDE (Hedging, Replication, CAPM)]]
are this list taken to the limit: binomial steps $\to$ geometric Brownian
motion; discrete $\to$ continuous rebalancing; constant $u,d \to$ constant
$\sigma$; constant $r$ stays constant $r$.

## 2. The Fundamental Theorem of Asset Pricing

The FTAP is two equivalent statements:

1. **No arbitrage.** The market admits no arbitrage if and only if it
   admits a martingale measure $\mathbb Q$ (a probability measure under
   which discounted asset prices are martingales).
2. **Market completeness.** That martingale measure is unique if and only
   if every contingent claim can be hedged — i.e. for every claim there is
   a replicating portfolio whose value $X_n$ matches the claim's value
   $C_n$ at every node.

In a complete market, any contingent claim can be perfectly replicated by
a portfolio of the underlying assets. Because that replication is exact,
there is a unique, unambiguous way to *compute* the fair value of any
asset using risk-neutral valuation — the recipe used throughout
Chapters 1–2.

## 3. The risk-neutral measure

Construct an artificial probability measure $\mathbb Q$ on the set of
coin-toss outcomes, where $\tilde p$ is the probability of a head (stock
up) and $\tilde q$ the probability of a tail. The expected value under
$\mathbb Q$ is written $\tilde{\mathbb E}_{\mathbb Q}$, giving the
simplified pricing formula

$$C_0 = \tilde{\mathbb E}_{\mathbb Q}\!\left(\frac{1}{1+r}C_1\right):$$

the price today is the discounted expected payoff tomorrow, under
$\mathbb Q$ — not under the real-world probability of the stock moving up
or down.

## 4. Theorem 1 — the discounted stock price is a martingale

**Claim.**

$$\tilde{\mathbb E}_{\mathbb Q}\Big[(1+r)^{-(n+1)}S_{n+1} \,\Big|\, \mathcal F_n\Big] = (1+r)^{-n}S_n.$$

**Proof.** At time $n+1$ the stock is $uS_n$ with probability $\tilde p$ or
$dS_n$ with probability $\tilde q$. Pull the constant $(1+r)^{-(n+1)}$ out
and apply the probabilities:

$$(1+r)^{-(n+1)}\big[\tilde p\,uS_n + \tilde q\,dS_n\big] = (1+r)^{-(n+1)}\,S_n\big[\tilde p\,u + \tilde q\,d\big].$$

Substitute $\tilde p = \frac{(1+r)-d}{u-d}$, $\tilde q = \frac{u-(1+r)}{u-d}$:

$$= (1+r)^{-(n+1)}\cdot\frac{S_n}{u-d}\Big[u(1+r) - ud + ud - d(1+r)\Big] = (1+r)^{-(n+1)}\cdot\frac{S_n}{u-d}\cdot(1+r)(u-d)$$

$$= (1+r)^{-(n+1)}\cdot S_n\,(1+r) = S_n(1+r)^{-n}. \qquad\blacksquare$$

The conditional expectation of tomorrow's discounted price exactly equals
today's discounted price — for a single step this is
$S_0 = \frac{1}{1+r}\mathbb E_{\mathbb Q}[S_1]$, which can be checked
directly: $\mathbb E_{\mathbb Q}[S_1] = \tilde p\,uS_0 + \tilde q\,dS_0$,
and substituting $\tilde p, \tilde q$ and simplifying (exactly as above)
returns $(1+r)S_0$.

## 5. Theorem 2 — the discounted self-financing portfolio is a martingale

**Claim.** Under $\mathbb Q$, $\big\{(1+r)^{-(n+1)}X_{n+1}\,\big|\,\mathcal F_n\big\}_{n=0}^{N}$ is a martingale, where

$$X_{n+1} = \Delta_n S_{n+1} + (1+r)\big[X_n - \Delta_n S_n\big]$$

is the value of a portfolio holding $\Delta_n$ shares and a risk-free bond,
the remainder $(X_n - \Delta_n S_n)$ compounding at $r$.

**Proof.**

$$\mathbb E_{\mathbb Q}\Big[(1+r)^{-(n+1)}X_{n+1}\,\Big|\,\mathcal F_n\Big] = \mathbb E_{\mathbb Q}\Big[(1+r)^{-(n+1)}\big\{\Delta_n S_{n+1} + (1+r)(X_n-\Delta_n S_n)\big\}\,\Big|\,\mathcal F_n\Big]$$

$X_n$ and $\Delta_n S_n$ are $\mathcal F_n$-measurable (known at time $n$),
so they pull out as constants:

$$= \Delta_n\,\mathbb E_{\mathbb Q}\Big[(1+r)^{-(n+1)}S_{n+1}\,\Big|\,\mathcal F_n\Big] + (1+r)^{-n}(X_n-\Delta_n S_n).$$

By Theorem 1 the expectation equals $(1+r)^{-n}S_n$:

$$= \Delta_n(1+r)^{-n}S_n + (1+r)^{-n}(X_n - \Delta_n S_n) = (1+r)^{-n}\big[\Delta_n S_n + X_n - \Delta_n S_n\big] = (1+r)^{-n}X_n. \qquad\blacksquare$$

**What the cancellation means.** The two $\Delta_n S_n$ terms cancel, so
the result holds for *any* strategy $\Delta_n$. However you trade, you
cannot change the discounted portfolio's expected growth under
$\mathbb Q$: it is zero. If some self-financing strategy started at
$X_0=0$ and ended with $X_N \ge 0$ and $X_N>0$ in some state, its
discounted expectation would be strictly positive — contradicting the
martingale property. That is the "no-arbitrage" half of the FTAP, seen
from the inside.

## 5a. From martingale to pricing formula

A martingale has constant expectation over time. Applying Theorem 2
repeatedly (the tower property — conditioning back one step at a time):

$$\frac{X_0}{(1+r)^0} = \tilde{\mathbb E}\!\left[\frac{X_1}{1+r}\right] = \tilde{\mathbb E}\!\left[\frac{X_2}{(1+r)^2}\right] = \dots = \tilde{\mathbb E}\!\left[\frac{X_N}{(1+r)^N}\right].$$

If $X$ replicates a derivative with payoff $V_N$ (so $X_N = V_N$ in every
state), no-arbitrage forces its price to be $X_0$:

$$\boxed{V_0 = \tilde{\mathbb E}\!\left[\frac{V_N}{(1+r)^N}\right]}$$

This is the $n$-period formula of [[02 - The Two-Period and n-Period BOPM]]
in one line, and it also covers path-dependent payoffs. The gap: it
assumes a replicating portfolio *exists*. Theorem 3 closes that gap.

## 6. Theorem 3 — the BOPM is a complete market

A market is complete if every derivative can be hedged. Define, **backward
in time**, the sequence $C_N, \dots, C_0$ by

$$C_n(w_1,\dots,w_n) = (1+r)^{-1}\Big[\tilde p\,C_{n+1}(w_1,\dots,w_n;H) + \tilde q\,C_{n+1}(w_1,\dots,w_n;T)\Big],$$

and the portfolio process

$$\Delta_n(w_1,\dots,w_n) = \frac{C_{n+1}(w_1,\dots,w_n;H) - C_{n+1}(w_1,\dots,w_n;T)}{S_{n+1}(w_1,\dots,w_n;H) - S_{n+1}(w_1,\dots,w_n;T)}.$$

Setting $X_0 = C_0$ and defining, **forward in time**,

$$X_{n+1} = \Delta_n S_{n+1} + (1+r)(X_n - \Delta_n S_n),$$

the claim is $X_n = C_n$ for every $n$ and every path.

**Proof — up-state case.** $S_{n+1}(H) = uS_n$, so

$$X_{n+1}(H) = \Delta_n\,uS_n + (1+r)X_n - (1+r)\Delta_n S_n = (1+r)X_n + \Delta_n S_n\big[u-(1+r)\big].$$

Substitute the hedge ratio $\Delta_n = \dfrac{C_{n+1}(H)-C_{n+1}(T)}{uS_n-dS_n} = \dfrac{C_{n+1}(H)-C_{n+1}(T)}{(u-d)S_n}$:

$$X_{n+1}(H) = (1+r)X_n + \frac{C_{n+1}(H)-C_{n+1}(T)}{u-d}\big[u-(1+r)\big].$$

Since $\tilde q = \dfrac{u-(1+r)}{u-d}$,

$$X_{n+1}(H) = (1+r)X_n + \tilde q\big[C_{n+1}(H) - C_{n+1}(T)\big].$$

Using $X_n = C_n$ and the pricing recursion $C_n(1+r) = \tilde p\,C_{n+1}(H) + \tilde q\,C_{n+1}(T)$:

$$X_{n+1}(H) = \tilde p\,C_{n+1}(H) + \tilde q\,C_{n+1}(T) + \tilde q\,C_{n+1}(H) - \tilde q\,C_{n+1}(T) = (\tilde p+\tilde q)\,C_{n+1}(H) = C_{n+1}(H). \qquad\blacksquare$$

**Which assumption does which job.**

- **$X_n = C_n$** (induction hypothesis) + the pricing recursion turn the
  bond term $(1+r)X_n$ into the risk-neutral *average*
  $\tilde p\,C_{n+1}(H) + \tilde q\,C_{n+1}(T)$. Here the $(1+r)$ from
  the bond growing one period cancels the $\tfrac{1}{1+r}$ that defined
  $C_n$.
- **The choice of $\Delta_n$** (the Chapter 1 hedge ratio at every node)
  turns the stock term into the *correction*
  $\tilde q\,[C_{n+1}(H) - C_{n+1}(T)]$, which moves you from the average
  to the up-state value. Stopping at the bond term alone gives the
  average, not $C_{n+1}(H)$.
- **$\tilde q = \frac{u-(1+r)}{u-d}$** is what lets the correction match the
  average's coefficients so a pair of terms cancels.
- **$\tilde p + \tilde q = 1$** closes it.

Since $X_0 = C_0$ by construction, induction carries $X_n = C_n$ node by
node to $X_N = C_N$: every payoff is replicable, the market is complete,
and the risk-neutral measure $(\tilde p,\tilde q)$ is unique.

**Exercise — the down-state.** Show, by the mirror argument, that
$X_{n+1}(T) = C_{n+1}(T)$. Work this through in
[[Working Notes]] rather than reading a finished proof — it is a direct
substitution copy of the up-state case with $u \leftrightarrow d$ and
$\tilde p \leftrightarrow \tilde q$ swapped at the right places.

---

**Next:** [[04 - The Black-Scholes PDE (Hedging, Replication, CAPM)]] — the
continuous-time analogue of this chapter's replicating-portfolio argument.
