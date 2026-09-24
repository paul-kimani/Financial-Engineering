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

---

**Next:** [[06 - Feynman-Kac and the BSM Formula]] — solving this PDE as a
conditional expectation, and deriving the closed-form call price.
