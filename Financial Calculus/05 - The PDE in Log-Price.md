# 05 — The PDE in Log-Price

> A short but useful change of variable: rewriting the Black-Scholes PDE
> in $x = \ln S$ instead of $S$ turns the $S$- and $S^2$-dependent
> coefficients into plain constants. This is the same trick that makes
> $\ln S_t$ (rather than $S_t$) follow a driftless-in-space, constant-
> coefficient process, and it is what makes the PDE tractable for the
> Feynman-Kac route to the Black-Scholes formula in the next chapter.

---

## 1. The transformation

Start from the Black-Scholes PDE:

$$\frac{\partial V}{\partial t} + rS\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} - rV = 0,$$

and set $x = \ln S$, so $S = e^x$.

## 2. The first derivative

By the chain rule, $\dfrac{\partial V}{\partial S} = \dfrac{\partial V}{\partial x}\cdot\dfrac{\partial x}{\partial S}$. Since $x=\ln S$, $\dfrac{\partial x}{\partial S} = \dfrac1S$, so

$$\boxed{\frac{\partial V}{\partial S} = \frac{\partial V}{\partial x}\cdot\frac1S}$$

## 3. The second derivative

$$\frac{\partial^2V}{\partial S^2} = \frac{\partial}{\partial S}\left[\frac{\partial V}{\partial x}\cdot\frac1S\right].$$

This is a product $uw$ with $u=1/S$, $w = \partial V/\partial x$, so by the
product rule $\partial(uw)/\partial S = u\,\partial w/\partial S + w\,\partial u/\partial S$:

$$\frac{\partial u}{\partial S} = \frac{\partial}{\partial S}(S^{-1}) = -\frac{1}{S^2}, \qquad \frac{\partial w}{\partial S} = \frac{\partial}{\partial x}\left[\frac{\partial V}{\partial x}\right]\cdot\frac{\partial x}{\partial S} = \frac{\partial^2V}{\partial x^2}\cdot\frac1S.$$

Combining:

$$\frac{\partial^2V}{\partial S^2} = \frac1S\left(\frac{\partial^2V}{\partial x^2}\cdot\frac1S\right) - \frac1{S^2}\cdot\frac{\partial V}{\partial x} = \frac1{S^2}\left[\frac{\partial^2V}{\partial x^2} - \frac{\partial V}{\partial x}\right].$$

## 4. Substituting back into the PDE

$$rS\frac{\partial V}{\partial S} = rS\left(\frac1S\frac{\partial V}{\partial x}\right) = r\frac{\partial V}{\partial x},$$

$$\frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = \frac12\sigma^2S^2\cdot\frac1{S^2}\left[\frac{\partial^2V}{\partial x^2} - \frac{\partial V}{\partial x}\right] = \frac12\sigma^2\left[\frac{\partial^2V}{\partial x^2} - \frac{\partial V}{\partial x}\right].$$

The $S$-dependence has cancelled completely. Substituting both terms:

$$\boxed{\frac{\partial V}{\partial t} + r\frac{\partial V}{\partial x} + \frac12\sigma^2\left[\frac{\partial^2V}{\partial x^2} - \frac{\partial V}{\partial x}\right] - rV = 0}$$

Grouping the two $\partial V/\partial x$ terms gives the equivalent form

$$\frac{\partial V}{\partial t} + \left(r - \frac12\sigma^2\right)\frac{\partial V}{\partial x} + \frac12\sigma^2\frac{\partial^2V}{\partial x^2} - rV = 0,$$

which is exactly a constant-coefficient PDE for a process with drift
$r-\tfrac12\sigma^2$ and volatility $\sigma$ — matching the log-price GBM
$\ln S_T \sim N\!\left(\ln S_0 + (r-\tfrac12\sigma^2)\tau,\ \sigma^2\tau\right)$
used in [[06 - Feynman-Kac and the BSM Formula]].

## 5. Notes from the live session (2026-09-29)

**Why change variables.** The $S$-form PDE has *variable* coefficients
($rS$, $\tfrac12\sigma^2S^2$). $\ln S$ is already known to be well-behaved
(normal with constant drift, [[02 - The Two-Period and n-Period BOPM]] §7),
so $x=\ln S$ is the natural coordinate.

**The chain rule, stated once.** Differentiating *anything* with respect to
$S$ means: differentiate with respect to $x$, then multiply by
$\frac{dx}{dS} = \frac1S$.

- On $u$: $\dfrac{\partial V}{\partial S} = \dfrac1S\dfrac{\partial u}{\partial x}$.
  The delta term becomes $rS\cdot\frac1S u_x = r\,u_x$ — the $S$ cancels.
- On $w := \dfrac{\partial u}{\partial x}$ (give it a name so it looks like
  any other function of $x$): $\dfrac{\partial w}{\partial S} = \dfrac1S\dfrac{\partial w}{\partial x} = \dfrac1S\dfrac{\partial^2u}{\partial x^2}$.

**Second derivative via the product rule**, $f = \frac1S$, $g = u_x$:

$$\frac{\partial^2V}{\partial S^2} = \underbrace{-\frac1{S^2}\frac{\partial u}{\partial x}}_{f'g} + \underbrace{\frac1S\cdot\frac1S\frac{\partial^2u}{\partial x^2}}_{fg'} = \frac1{S^2}\left(\frac{\partial^2u}{\partial x^2} - \frac{\partial u}{\partial x}\right),$$

so the gamma term becomes
$\tfrac12\sigma^2\left(u_{xx} - u_x\right)$ — the $S$ cancels again.

**Result.** Collecting the two $u_x$ terms:

$$\frac{\partial u}{\partial t} + \left(r-\tfrac12\sigma^2\right)\frac{\partial u}{\partial x} + \frac12\sigma^2\frac{\partial^2u}{\partial x^2} - ru = 0,$$

constant coefficients throughout.

**Reading the coefficients.** $r-\tfrac12\sigma^2$ is the $\mathbb Q$-drift of
$\ln S$ per unit time; $\tfrac12\sigma^2$ is half its variance per unit time.
The PDE encodes the dynamics

$$dX_t = \left(r-\tfrac12\sigma^2\right)dt + \sigma\,dW_t \quad\text{under }\mathbb Q.$$

General pattern: **drift in front of the first derivative, half the
variance in front of the second.** That correspondence is exactly what
Feynman-Kac makes precise ([[06 - Feynman-Kac and the BSM Formula]]).

**Third sighting of $-\tfrac12\sigma^2$.** Here it comes from the
$-u_x$ produced by the product rule inside the gamma term. The same
convexity correction as Jensen (Chapter 2 §7) and Itô's lemma — reached by
plain calculus.

---

**Next:** [[06 - Feynman-Kac and the BSM Formula]] — solving this PDE as a
conditional expectation, and deriving the closed-form call price.
