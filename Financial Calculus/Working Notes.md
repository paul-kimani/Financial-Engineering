# Working Notes — Financial Calculus

Scratch space for retrieval practice: after reading a reference chapter,
close it and write what you remember here in your own words *before*
checking back against it. Keep entries dated so you can see gaps re-emerge
(or not) across sessions.

---

## 2026-09-24 — No-arbitrage, the other direction (ref: [[01 - The One-Period BOPM]])

The notebook only worked out what happens when $u < 1+r$. Try the mirror
case: $1+r \le d$. What trade gives a guaranteed non-negative profit at
zero cost, and what is the portfolio worth at time 1 in states $H$ and $T$?



## 2026-09-24 — The down-state exercise (ref: [[03 - FTAP, Risk-Neutral Measure and Martingales]])

Show $X_{n+1}(T) = C_{n+1}(T)$, mirroring the up-state proof of
Theorem 3. Left open in the original notebook (attempted in pencil, not
completed).

**Done 2026-09-28** — worked live; now in Chapter 3 §6. Slip: wrote the
coefficient as $-\tilde q$; it is $-\tilde p$, since
$d-(1+r) = -[(1+r)-d]$ and $(1+r)-d$ is $\tilde p$'s numerator. Check: it
must cancel the $+\tilde p\,V_{n+1}(H)$ from the bond term.



## 2026-09-24 — The $F(T,x)=x^4$ exercise (ref: [[06 - Feynman-Kac and the BSM Formula]])

Solve $\dfrac{\partial F}{\partial t} + \tfrac14 x\dfrac{\partial F}{\partial x} + \tfrac12 x^2\dfrac{\partial^2F}{\partial x^2} + F = 0$, $F(T,x)=x^4$.
Not attempted in the notebook. Note the state-dependent $\sigma^2(x,t)=x^2$
and the $r(x,t)=-1$ read off the $+F$ term.

## 2026-09-27 — Live Socratic session, Chapters 1–3

Worked through live: one-period replicating portfolio → $\Delta_0$ → $C_0$;
meaning of $\tilde p,\tilde q$ (sum to 1, positive $\iff$ no-arbitrage,
$\tilde p u + \tilde q d = 1+r$); two-period by treating each $t=1$ node as
a one-period tree; $n$-period binomial sum; Theorem 1; Theorem 2;
pricing by the martingale property; Theorem 3 up-state.

**Slips to watch:**

- Dropped the discount factor twice — in $C_1(H)$ from the two-period step
  and in the $n$-period sum. Every step back in time is one factor of
  $\tfrac{1}{1+r}$.
- Theorem 3: stopped at the bond term
  ($\tilde p\,V_{n+1}(H) + \tilde q\,V_{n+1}(T)$) and forgot to add the
  stock term $\tilde q\,[V_{n+1}(H) - V_{n+1}(T)]$.
- Notation: wrote $S_{n+1}(H)$ for $V_{n+1}(H)$ — keep stock price and
  derivative value apart.

**Down-state:** closed 2026-09-28 (see entry above).

## 2026-09-28 — Binomial to lognormal (ref: [[02 - The Two-Period and n-Period BOPM]] §7)

Worked live: CRR $u,d$; $\tilde p \approx \tfrac12 + \frac{(r-\frac12\sigma^2)\sqrt{\Delta t}}{2\sigma}$;
per-step mean/variance; CLT to lognormal; why $-\tfrac12\sigma^2$.

**Slips to watch:**

- Gave the per-step mean as $r-\tfrac12\sigma^2$ — missing $\Delta t$. Units
  check: the variance had a $\Delta t$, so the mean must too.
- Knew the "drift $= rT$" case leaves an $e^{\frac12\sigma^2T}$ but not what it
  implies: discounted stock drifts up $\Rightarrow$ not a $\mathbb Q$-martingale
  $\Rightarrow$ Theorem 1 fails $\Rightarrow$ arbitrage (FTAP).

## 2026-09-28 — Binomial to Black-Scholes, Piece 2 (ref: [[02 - The Two-Period and n-Period BOPM]] §8)

Worked live: the threshold $a$; splitting $C_0$ into two sums; Piece 2 as
$\mathbb Q(S_T>K)$; standardising; symmetry; defining $d_2$.

**Slips / sticking points:**

- Isolating $j$: wrote $j < \dots$. $\ln(u/d)>0$ so the inequality does not
  flip — and a call pays for *many* ups, so it must be $j > \dots$.
- Tried to substitute $a$'s formula into the split sum and got stuck with
  logs. $a$ is only the lower summation limit; leave it as $a$.
- Standardising: thought the constant right-hand side "has no mean or sd".
  The mean and sd are $X$'s; they are applied to both sides only to keep
  the inequality balanced.
- $\mathbb P(Z>c) = \Phi(-c)$: follows from $\varphi(-z)=\varphi(z)$ — reflect the
  bell curve; right tail beyond $c$ = left tail beyond $-c$.

## 2026-09-29 — Binomial to Black-Scholes, Piece 1 (ref: [[02 - The Two-Period and n-Period BOPM]] §8.4–8.5)

Worked live: $p'+q'=1$ (recalled from Ch 1 §6(c)); the stock measure
$\mathbb Q'$; $p'$ to order $\sqrt{\Delta t}$; $d_1$; $d_1-d_2=\sigma\sqrt T$ (got
this one straight away).

**Sticking points:**

- The idea of a second measure $\mathbb Q'$ didn't click at first. Key
  picture: $\frac{u}{1+r}>1$ boosts up-paths, $\frac{d}{1+r}<1$ shrinks
  down-paths — $\mathbb Q'$ weights paths by how much the stock grew,
  because Piece 1 *pays the stock*.
- Why drop order $\Delta t$ in $p'$ but keep it in §7's numerator: keep
  what survives after multiplying by $N=T/\Delta t$. §7 divided by
  $\sqrt{\Delta t}$ (promotes terms); $p'$ is a product (doesn't).

## 2026-09-29 — Chapter 4: three derivations of the PDE (ref: [[04 - The Black-Scholes PDE (Hedging, Replication, CAPM)]] §5)

Worked live: all three routes. Got $\Delta = a_t = \partial V/\partial S$ and
the PDE each time without slips.

**Needed explaining:**

- The option's elasticity $\Omega = \frac{S}{V}\frac{\partial V}{\partial S}$ and
  why $\beta_V = \Omega\beta_S$ — didn't have it in mind. Remember the ATM
  example: $S=100, V=5, \Delta=0.5 \Rightarrow \Omega = 10$.
- Point to remember: in all three routes $\mu$ cancels (and $\mu_M$ too in
  CAPM) — that's *why* the PDE has $r$, not $\mu$.

## 2026-09-29 — Chapter 5: log-price PDE (ref: [[05 - The PDE in Log-Price]] §5)

Worked live. First derivative and $f'=-1/S^2$ done unaided; identified
the $u_x$ coefficient as $r-\tfrac12\sigma^2$ straight away.

**Sticking point:** $\frac{\partial}{\partial S}\left(\frac{\partial u}{\partial x}\right)$.
Fix: call $w = u_x$ — it's just another function of $x$, so the same
chain rule applies: $\partial_S w = \frac1S\,\partial_x w = \frac1S u_{xx}$.

## 2026-09-29 — Chapter 6: Feynman-Kac proof (ref: [[06 - Feynman-Kac and the BSM Formula]] §5)

Worked live. Recognised the $ds$ bracket vanishes and that the Itô
integral has zero expectation.

**Sticking points / slips:**

- Confused by what to differentiate with respect to: $t$ is fixed (today),
  $s$ is the running clock — all derivatives are in $s$.
- Called the $ds$ bracket "the Feynman-Kac"; it is the **PDE** (the
  hypothesis). Feynman-Kac is the conclusion.
- Dropped $e^{-r(s-t)}$ from the $dW$ term.

## 2026-09-29 — Feynman-Kac redone, notebook version (ref: [[06 - Feynman-Kac and the BSM Formula]] §5)

Redid the undiscounted proof in four moves after the discounted version
didn't land. Did moves 1, 3 unaided.

**Slips:**

- Itô: dropped the $\tfrac12$ on $\sigma^2 F_{xx}$. It's Taylor's $\tfrac1{2!}$.
- Called the surviving $dW$ term "the drift". The $dt$ term is the drift
  (it vanishes); the $dW$ term is the diffusion.
- Final answer written $\mathbb E[\Phi(x)]$; must be $\mathbb E[\Phi(X_T)\mid X_t=x]$.

