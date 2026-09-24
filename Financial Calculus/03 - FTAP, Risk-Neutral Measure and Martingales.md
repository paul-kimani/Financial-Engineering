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

## 1. Core market assumptions

- **Unlimited short-selling** — the stock can be shorted without
  restriction or borrowing limits.
- **Unlimited borrowing** — infinite access to capital.
- **Frictionless trading** — no transaction costs.
- **Price-taker status** — the agent is small; individual trades never
  move the market price.

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

**Exercise — the down-state.** Show, by the mirror argument, that
$X_{n+1}(T) = C_{n+1}(T)$. Work this through in
[[Working Notes]] rather than reading a finished proof — it is a direct
substitution copy of the up-state case with $u \leftrightarrow d$ and
$\tilde p \leftrightarrow \tilde q$ swapped at the right places.

---

**Next:** [[04 - The Black-Scholes PDE (Hedging, Replication, CAPM)]] — the
continuous-time analogue of this chapter's replicating-portfolio argument.
