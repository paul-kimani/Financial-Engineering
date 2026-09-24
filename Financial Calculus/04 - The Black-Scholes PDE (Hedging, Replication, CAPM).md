# 04 — The Black-Scholes PDE (Hedging, Replication, CAPM)

> The BOPM's replicating portfolio has a continuous-time twin: build a
> portfolio of the option and $-\Delta$ shares, choose $\Delta$ to kill the
> random $dW$ term, then invoke no-arbitrage to force the surviving
> deterministic part to earn the risk-free rate. The same PDE falls out
> three different ways — by direct hedging, by a self-financing
> stock-and-bond replication, and by applying the CAPM to both the stock
> and the option — which is reassuring: three independent arguments, one
> equation.

---

## 1. Reading the PDE

The Black-Scholes PDE for an option value $V(S,t)$ is

$$\underbrace{\frac{\partial V}{\partial t}}_{\Theta} + \underbrace{rS\frac{\partial V}{\partial S}}_{\Delta \cdot rS} + \underbrace{\frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}}_{\Gamma} - rV = 0.$$

- $\dfrac{\partial V}{\partial t} = \Theta$ (theta) — time decay: how the
  option's value erodes as expiration approaches.
- $rS\dfrac{\partial V}{\partial S}$ — the expected growth in the option's
  value from the stock moving at the risk-free rate, combining the
  option's sensitivity to price ($\Delta = \partial V/\partial S$) with
  $r$.
- $\tfrac12\sigma^2S^2\dfrac{\partial^2V}{\partial S^2}$ — the value
  created by convexity in the option's payoff (gamma, $\Gamma =
  \partial^2V/\partial S^2$), driven by the stock's volatility $\sigma$.
- $rV$ — the opportunity cost of holding the option instead of investing
  its value $V$ at the risk-free rate.

It can be derived four ways: (1) hedging, the original route; (2) a
self-financing replicating portfolio; (3) the CAPM; (4) as a limiting case
of the Cox-Ross-Rubinstein BOPM as the number of periods $\to\infty$. The
first three are worked out below.

## 2. Route 1 — BSM PDE by hedging

Build a portfolio of one option minus $\Delta$ shares of stock:

$$\Pi = V - \Delta S, \qquad d\Pi = dV - \Delta\,dS.$$

Under $\mathbb P$, $dS = \mu S\,dt + \sigma S\,dW$, and by Itô's Lemma

$$dV = \left(\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}\right)dt + \sigma S\frac{\partial V}{\partial S}\,dW.$$

Substituting,

$$d\Pi = \left[\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} - \Delta\mu S\right]dt + \left[\sigma S\frac{\partial V}{\partial S} - \Delta\sigma S\right]dW.$$

For the portfolio to be riskless, the $dW$ coefficient must vanish:

$$\sigma S\left[\frac{\partial V}{\partial S} - \Delta\right] = 0 \;\Rightarrow\; \Delta = \frac{\partial V}{\partial S} \quad\text{(the delta hedge ratio).}$$

Substituting this $\Delta$ back (the $\mu S\,\partial V/\partial S$ terms
cancel):

$$d\Pi = \left(\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}\right)dt.$$

No-arbitrage says a riskless portfolio must earn exactly the risk-free
rate: $d\Pi = r\Pi\,dt = r(V-\Delta S)\,dt$. Equating the two expressions
for $d\Pi$, then substituting $\Delta = \partial V/\partial S$ and
rearranging:

$$\boxed{\frac{\partial V}{\partial t} + rS\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} - rV = 0}$$

## 3. Route 2 — BSM PDE by a self-financing replicating portfolio

Assume a complete market: any derivative can be priced by a
self-financing replicating strategy with the same payoff, so the
derivative's value equals its replicating portfolio's value at all times.
A self-financing strategy is one whose value changes only from asset-value
changes, never from outside cash flows.

Form a portfolio of stock $S$ and bond $B$:

$$\Pi(t) = a(t)S(t) + b(t)B(t), \qquad d\Pi = a\,dS + b\,dB,$$

with $a(t), b(t)$ the units of stock and bond held. Since $V=\Pi$,
$dV = a\,dS + b\,dB$. The asset dynamics are $dS = \mu S\,dt + \sigma S\,dW_t$
and $dB = rB\,dt$; by Itô's Lemma, $dV$ is as in §2. Substituting all three
into $dV = a\,dS + b\,dB$:

$$\left(\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}\right)dt + \sigma S\frac{\partial V}{\partial S}\,dW_t = \big(a\mu S + rbB\big)\,dt + a\sigma S\,dW_t.$$

Equate $dW_t$ coefficients: $a = \partial V/\partial S$ (again, the number
of shares needed to replicate). Equate $dt$ coefficients, and the
$\mu S\,\partial V/\partial S$ terms cancel:

$$\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = rbB.$$

From $V = aS+bB$: $bB = V - aS = V - S\,\partial V/\partial S$. Substitute:

$$\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = r\left(V - S\frac{\partial V}{\partial S}\right),$$

which rearranges to the same PDE as §2.

## 4. Route 3 — BSM PDE from the CAPM

The CAPM says the expected return on any risky asset equals the risk-free
rate plus a risk premium proportional to systematic risk $\beta$:

$$\mathbb E(R_i) - r_f = \beta_i\big[\mathbb E(R_m) - r_f\big], \qquad \beta_i = \frac{\operatorname{Cov}(R_i,R_m)}{\operatorname{Var}(R_m)}.$$

Apply it to the stock ($\mathbb E(R_S)=\mu$) and to the option, whose
value is a function $V(S,t)$ of the stock price and time:

$$\mu = r + \beta_S\{\mathbb E[R_m]-r\}, \qquad \frac{\mathbb E[dV]}{V} = r + \beta_V\{\mathbb E[R_m]-r\}.$$

**Relating $\beta_V$ to $\beta_S$.** For infinitesimal returns,
$dR_S = dS/S$, $dR_V = dV/V$. Since the covariance of a change with a
stochastic process depends only on the stochastic ($dW_t$) part,

$$\operatorname{Cov}(dR_V,dR_M) = \frac{\sigma S}{V}\cdot\frac{\partial V}{\partial S}\cdot\sigma_M\,dt, \qquad \operatorname{Cov}(dR_S,dR_M) = \sigma\sigma_M\,dt.$$

Taking the ratio of betas (both share the $\operatorname{Var}(dR_M)$
denominator):

$$\frac{\beta_V}{\beta_S} = \frac{\operatorname{Cov}(dR_V,dR_M)}{\operatorname{Cov}(dR_S,dR_M)} = \frac{S}{V}\cdot\frac{\partial V}{\partial S} \;\Rightarrow\; \beta_V = \frac{S}{V}\frac{\partial V}{\partial S}\,\beta_S.$$

Substitute into the option's CAPM equation, and use
$\beta_S\{\mathbb E(R_m)-r\} = \mu - r$ from the stock's CAPM equation:

$$\frac{\mathbb E[dV]}{V} = r + \frac{\partial V}{\partial S}\cdot\frac{S}{V}(\mu-r) \;\Rightarrow\; \mathbb E[dV] = rV + \mu S\frac{\partial V}{\partial S} - rS\frac{\partial V}{\partial S}.$$

From Itô's Lemma, $\mathbb E[dW_t]=0$ so the deterministic part of $dV$ is
$\mathbb E[dV] = \left(\partial V/\partial t + \mu S\,\partial V/\partial S + \tfrac12\sigma^2S^2\,\partial^2V/\partial S^2\right)dt$.
Equating the two expressions for $\mathbb E[dV]$, the $\mu S\,\partial
V/\partial S$ terms cancel, leaving — once more — the same PDE.

---

**Next:** [[05 - The PDE in Log-Price]] — the same equation, transformed
into the log-stock-price variable $x=\ln S$.
