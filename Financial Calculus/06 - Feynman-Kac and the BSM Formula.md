# 06 — Feynman-Kac and the BSM Formula

> Feynman-Kac is the bridge that turns "solve this PDE" into "compute this
> expectation": the solution to a linear parabolic PDE with terminal
> condition $\Phi$ can be written as $\mathbb E[\Phi(X_T)]$ for the
> stochastic process whose generator matches the PDE's coefficients.
> Applied to the log-price Black-Scholes PDE, it reproduces the closed-form
> European call price directly — no PDE-solving machinery required, just
> the log-normal distribution of $S_T$ under $\mathbb Q$.

---

## 1. The theorem

Let $X_t$ follow the SDE

$$dX_t = \mu(X_t,t)\,dt + \sigma(X_t,t)\,dW_t,$$

and define $F(t,x) = \mathbb E_{t,x}\big[\Phi(X_T)\big]$. Then $F$ solves the PDE

$$\frac{\partial F}{\partial t} + \mu(t,x)\frac{\partial F}{\partial x} + \frac12\sigma^2(t,x)\frac{\partial^2F}{\partial x^2} = 0, \qquad F(T,x) = \Phi(x).$$

## 2. Proof

Apply Itô's Lemma to $F(t,X_t)$:

$$dF(t,X_t) = \left[\frac{\partial F}{\partial t} + \mu(X_t,t)\frac{\partial F}{\partial x} + \frac12\sigma^2(X_t,t)\frac{\partial^2F}{\partial x^2}\right]dt + \sigma(X_t,t)\frac{\partial F}{\partial x}\,dW_t.$$

The bracketed $dt$-term is exactly the PDE, which is defined to equal
zero, so

$$dF(t,X_t) = \sigma(X_t,t)\frac{\partial F}{\partial x}\,dW_t.$$

Integrate both sides from $t$ to $T$:

$$\int_t^T dF(u,X_u) = \int_t^T \sigma(X_u,u)\frac{\partial F}{\partial x}\,dW_u \;\Longrightarrow\; F(T,X_T) - F(t,X_t) = \int_t^T \sigma(X_u,u)\frac{\partial F}{\partial x}\,dW_u.$$

Take $\mathbb E_{t,x}$ of both sides. The right side is an Itô integral, and
an Itô integral has expectation zero (it is built from non-anticipating
increments of $W$, each with mean zero), so

$$\mathbb E\big[F(T,X_T)\big] - \mathbb E\big[F(t,X_t)\big] = 0.$$

$F(t,X_t)$ is not itself random — it is the value of a deterministic
function at the *known*, already-realised pair $(t,X_t)$ — so its
expectation is just itself:

$$\mathbb E\big[F(T,X_T)\big] = F(t,X_t).$$

But the terminal condition says $F(T,X_T) = \Phi(X_T)$, so

$$\boxed{F(t,X_t) = \mathbb E\big[\Phi(X_T)\big]} \qquad\text{— the Feynman-Kac stochastic representation formula.}$$

## 3. A PDE with a source term and a discount rate

The version needed for option pricing has two extra pieces: a discount
rate $r(x,t)$ and a source term $g(x,t)$:

$$\frac{\partial F}{\partial t} + \mu(x,t)\frac{\partial F}{\partial x} + \frac12\sigma^2(x,t)\frac{\partial^2F}{\partial x^2} - r(x,t)F + g(x,t) = 0, \qquad F(T,x)=\Phi(x),$$

with solution

$$F(t,x) = \mathbb E\left[\int_t^T g(X_\tau,\tau)\,d\tau + \Phi(X_T)\ \Big|\ X_t = x\right].$$

**Worked example.** Solve $\dfrac{\partial F}{\partial t} + \dfrac{\partial F}{\partial x} + \dfrac{\partial^2F}{\partial x^2} - x^2+1 = 0$, $F(T,x)=2x$.

Comparing term by term: $\mu(x,t)=1$; $\tfrac12\sigma^2 = 1 \Rightarrow \sigma=\sqrt2$; discount rate $r(x,t)=0$; source term $g(x,t)=1-x^2$; terminal condition $\Phi(x)=2x$. The underlying SDE is $dX_s = ds + \sqrt2\,dW_s$, so integrating from $t$ to $T$,

$$X_T = x + (T-t) + \sqrt2(W_T-W_t).$$

By linearity, $F(x,t) = \underbrace{2\,\mathbb E[X_T]}_{A} + \underbrace{\int_t^T \mathbb E[1-X_\tau^2]\,d\tau}_{B}$.

*Part A.* $\mathbb E[X_T] = x+(T-t)$ (the $\sqrt2(W_T-W_t)$ term has mean 0), so $A = 2x+2(T-t)$.

*Part B.* $\operatorname{Var}(X_T) = 2(T-t)$, so $\mathbb E[X_T^2] = 2(T-t) + \big(x+(T-t)\big)^2$, giving $\mathbb E[1-X_\tau^2] = 1-x^2-2u-2xu-u^2$ where $u=\tau-t$. Integrating $u$ from $0$ to $T-t$:

$$B = (1-x^2)(T-t) - (1+x)(T-t)^2 - \tfrac13(T-t)^3.$$

$$\boxed{F(t,x) = 2x + 2(T-t) + (1-x^2)(T-t) - (1+x)(T-t)^2 - \tfrac13(T-t)^3}$$

(Checked directly: this satisfies the PDE and $F(T,x)=2x$.)

**Open exercise.** Solve $\dfrac{\partial F}{\partial t} + \tfrac14 x\dfrac{\partial F}{\partial x} + \tfrac12 x^2\dfrac{\partial^2F}{\partial x^2} + F = 0$, $F(T,x)=x^4$ — note the $+F$ (i.e. $r(x,t)=-1$) and state-dependent $\sigma^2(x,t)=x^2$. Work this in [[Working Notes]].

## 4. Deriving the Black-Scholes formula

The European call $V(S,t)$ with strike $K$ and maturity $T$ solves the
Black-Scholes PDE with terminal condition $V(S_T,T)=(S_T-K)^+$. By
Feynman-Kac (with constant discount rate $r$, and $\tau=T-t$):

$$V(S,t) = e^{-r\tau}\,\mathbb E_{\mathbb Q}\big[(S_T-K)^+\ \big|\ \mathcal F_t\big] = e^{-r\tau}\int_K^\infty (S_T-K)\,dF(S_T) = e^{-r\tau}\int_K^\infty S_T\,dF(S_T) - Ke^{-r\tau}\int_K^\infty dF(S_T).$$

Under $\mathbb Q$, $\ln S_T \sim N\big(M,\hat\sigma^2\big)$ with

$$M = \ln S_0 + \left(r-\tfrac12\sigma^2\right)\tau, \qquad \hat\sigma = \sigma\sqrt\tau.$$

**First integral.** For a log-normal variable, the partial expectation
$\int_K^\infty S_T\,dF(S_T) = \mathbb E_{\mathbb Q}\big[S_T\,\mathbf 1_{\{S_T>K\}}\big]$ has the closed form

$$\int_K^\infty S_T\,dF(S_T) = e^{M+\frac12\hat\sigma^2}\,\Phi\!\left(\frac{-\ln K + M + \hat\sigma^2}{\hat\sigma}\right).$$

Since $M + \tfrac12\hat\sigma^2 = \ln S_0 + r\tau$, this is $S_0e^{r\tau}\,\Phi(d_1)$ with

$$d_1 = \frac{\ln(S_0/K) + \left(r+\tfrac12\sigma^2\right)\tau}{\sigma\sqrt\tau}.$$

So $e^{-r\tau}\int_K^\infty S_T\,dF(S_T) = e^{-r\tau}\cdot S_0e^{r\tau}\,\Phi(d_1) = S_0\,\Phi(d_1)$.

**Second integral.** $\int_K^\infty dF(S_T) = 1 - \Phi\!\left(\dfrac{\ln K - M}{\hat\sigma}\right) = 1-\Phi(-d_2) = \Phi(d_2)$, with $d_2 = d_1 - \sigma\sqrt\tau$ (by the symmetry $\Phi(x)=1-\Phi(-x)$ of the standard normal).

**Combining the two terms:**

$$\boxed{C(S,K,\tau) = S_0\,\Phi(d_1) - Ke^{-r\tau}\,\Phi(d_2)}$$

the Black-Scholes-Merton formula for a European call.

---

> **A note on this derivation.** The notebook version of this step carries a
> sign slip: it writes $e^{M+\frac12\hat\sigma^2} = S_0e^{-r\tau}$, which
> would make the final combination give $S_0e^{-2r\tau}\Phi(d_1)$ instead of
> $S_0\Phi(d_1)$. The correct exponent is $+r\tau$ (since
> $M+\tfrac12\hat\sigma^2 = \ln S_0 + r\tau$), which is what makes the last
> line work out to $S_0\,\Phi(d_1)$ exactly as it should. Fixed here; see
> [[00 - Lecture Notes Transcript]] for the page as written.

**Related:** [[../Stochastics/28 - Derivation of the Black Scholes PDE]] and
[[../Stochastics/29 - Derivation of The Black Scholes merton formula]] cover
the same PDE and formula from the Stochastics course's own route — useful
for cross-checking notation and seeing a second derivation of $d_1,d_2$.

---

## 5. The proof, worked live (2026-09-29)

**Two times.** $t$ is **fixed** — "today", the moment we want the price.
$s$ is the **running clock**, moving from $t$ to $T$. All derivatives are
with respect to $s$; $t$ is a constant in the exponent. The discount factor
$e^{-r(s-t)}$ equals $1$ at $s=t$ and $e^{-r(T-t)}$ at $s=T$.

**Strategy — the continuous Theorem 2.** Define the discounted value along
the path, $Y_s := e^{-r(s-t)}F(s,X_s)$. Its endpoints are

$$Y_t = F(t,x)\ \ (\text{today's price — wanted}), \qquad Y_T = e^{-r(T-t)}\Phi(X_T)\ \ (\text{discounted payoff — known}).$$

If $Y$ has no drift, $Y_t = \mathbb E[Y_T]$ — and that *is* the theorem.

**Step 1 — the deterministic factor.**
$d\big(e^{-r(s-t)}\big) = -r\,e^{-r(s-t)}\,ds$ (ordinary calculus in $s$).

**Step 2 — Itô on $F$.** Exactly Chapter 4's $dV$ with letters renamed:

$$dF = \left(\frac{\partial F}{\partial s} + \mu\frac{\partial F}{\partial x} + \frac12\sigma^2\frac{\partial^2F}{\partial x^2}\right)ds + \sigma\frac{\partial F}{\partial x}\,dW_s.$$

**Step 3 — product rule** (no cross term: the exponential has no $dW$):

$$dY_s = e^{-r(s-t)}\underbrace{\left(\frac{\partial F}{\partial s} + \mu\frac{\partial F}{\partial x} + \frac12\sigma^2\frac{\partial^2F}{\partial x^2} - rF\right)}_{=\,0\ \text{— the PDE}}ds + e^{-r(s-t)}\,\sigma\frac{\partial F}{\partial x}\,dW_s.$$

The $-rF$ comes from differentiating the discount factor — which is why
the discount rate in the formula must match the $-rF$ in the PDE. The
bracket is the **PDE** $F$ satisfies by hypothesis (Feynman-Kac is the
*conclusion*). So $Y$ has no drift.

> **Slip to watch:** keep the $e^{-r(s-t)}$ on the $dW$ term.

**Step 4 — integrate and take expectations.**

$$Y_T - Y_t = \int_t^T \underbrace{e^{-r(s-t)}\sigma\frac{\partial F}{\partial x}(s,X_s)}_{H_s}\,dW_s.$$

*Why the Itô integral has zero expectation.* Discretise:
$\sum_k H_{s_k}\Delta W_k$. $H_{s_k}$ is known at $s_k$; $\Delta W_k$ is
independent of the past with mean zero. By the tower property,

$$\mathbb E\big[H_{s_k}\Delta W_k\big] = \mathbb E\Big[H_{s_k}\,\underbrace{\mathbb E[\Delta W_k\mid\mathcal F_{s_k}]}_{=0}\Big] = 0.$$

Same logic as Theorem 2 in [[03 - FTAP, Risk-Neutral Measure and Martingales]]:
no position size ($H$ here, $\Delta_n$ there) extracts expected profit from
a fair game. Rigorously this needs $\mathbb E\int_t^T H_s^2\,ds<\infty$, so
the integral is a true martingale.

Hence $\mathbb E[Y_T\mid X_t=x] = Y_t$, i.e.

$$\boxed{F(t,x) = e^{-r(T-t)}\,\mathbb E\big[\Phi(X_T)\,\big|\,X_t = x\big]}$$

**Where it lands.** Applied to the log-price PDE of
[[05 - The PDE in Log-Price]] (drift $r-\tfrac12\sigma^2$, payoff
$(e^x-K)^+$), the process Feynman-Kac produces drifts at $r$, not $\mu$ —
**Feynman-Kac derives risk-neutral pricing**; the Chapter 4 PDE already
contained $\mathbb Q$. The resulting expectation splits into exactly the
$\mathbb Q(S_T>K)$ and $\mathbb Q'(S_T>K)$ of
[[02 - The Two-Period and n-Period BOPM]] §8. Tree route and PDE route meet
here.

