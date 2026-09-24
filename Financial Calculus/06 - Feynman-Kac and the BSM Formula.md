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
