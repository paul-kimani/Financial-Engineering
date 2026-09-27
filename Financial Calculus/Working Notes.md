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

**Still open:** the down-state (entry above). Watch the sign of
$d-(1+r)$ — that's where no-arbitrage enters.

