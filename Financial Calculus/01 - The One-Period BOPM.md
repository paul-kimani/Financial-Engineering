# 01 — The One-Period BOPM

> The binomial option pricing model (BOPM) prices a derivative without ever
> needing the real-world probability of the stock going up. A replicating
> portfolio of stock and cash is built to match the option's payoff in both
> states; because two portfolios with the same payoff must have the same
> price (no-arbitrage), solving for that portfolio *is* solving for the
> option price. The result can be rewritten as a discounted expectation
> under artificial **risk-neutral probabilities** $\tilde p, \tilde q$ —
> the seed of every pricing formula that follows in this course.

---

## 1. Setting up the model

Stock prices are modelled in discrete time. Starting from a known price $S_0$, one "toss" of a coin decides the next price: heads (H) multiplies it by $u$, tails (T) multiplies it by $d$, with

$$0 < d < 1 < u.$$

**After the 1st toss:**

$$S_1(H) = uS_0, \qquad S_1(T) = dS_0.$$

**After the 2nd toss:**

$$S_2(HH) = u^2S_0, \qquad S_2(HT) = S_2(TH) = udS_0, \qquad S_2(TT) = d^2S_0.$$

An up-move followed by a down-move lands on the same price as a down-move
followed by an up-move, because $u \cdot d = d \cdot u$ — multiplication
commutes. This is why the tree **recombines**: after $n$ tosses there are
$n+1$ distinct prices, not $2^n$.

## 2. The no-arbitrage condition

Introduce a money market account with interest rate $r$: one unit invested
today grows to $(1+r)$ next period. For the model to admit no arbitrage,

$$d < 1+r < u.$$

**Why both inequalities are needed.**

*If $1+r \le u$ fails, i.e. $u < 1+r$:* the risk-free asset beats the stock
in every state. Short the stock (borrow it, sell it for $S_0$), invest the
proceeds at $r$. Next period you owe at most $uS_0$ to close the short, but
your cash has grown to $(1+r)S_0 > uS_0$. Guaranteed profit, at zero net
cost — an arbitrage.

*If $d \ge 1+r$ fails, i.e. $1+r \le d$:* the stock beats the risk-free
asset in every state. Borrow $S_0$ at rate $r$, buy the stock. Next period
you owe $(1+r)S_0$, but the stock is worth at least $dS_0 \ge (1+r)S_0$.
Again a guaranteed non-negative profit at zero net cost.

Only when $d < 1+r < u$ does neither asset dominate the other in every
state — which is exactly the condition needed for a replicating portfolio
to price the option, rather than for one asset to arbitrage the other.

## 3. The single-period replicating portfolio

Consider a European call with strike $K>0$, payoff $(S_T-K)^+$ at maturity.
Short the call at $t=0$ for price $C_0$; you now owe $(uS_0-K)^+$ if $H$ and
$(dS_0-K)^+$ if $T$.

To find the fair price $C_0$, build a portfolio that replicates these exact
obligations in both states:

- Hold $\Delta_0$ units of stock.
- Invest the remaining cash, $C_0 - \Delta_0 S_0$, at the risk-free rate $r$.

The portfolio's value at time 1 is

$$X_1(H) = \Delta_0 S_1(H) + [C_0 - \Delta_0 S_0](1+r),$$
$$X_1(T) = \Delta_0 S_1(T) + [C_0 - \Delta_0 S_0](1+r).$$

For replication, $X_1$ must equal the call's value $C_1$ in both states:

$$C_1(H) = \Delta_0 S_1(H) + [C_0 - \Delta_0 S_0](1+r) \qquad (1)$$
$$C_1(T) = \Delta_0 S_1(T) + [C_0 - \Delta_0 S_0](1+r) \qquad (2)$$

two equations, two unknowns ($\Delta_0$ and $C_0$).

## 4. Solving for the hedge ratio and the option price

**Hedge ratio.** Subtracting (2) from (1) cancels the identical cash/bond
term:

$$C_1(H) - C_1(T) = \Delta_0\big[S_1(H) - S_1(T)\big]$$

$$\Delta_0 = \frac{C_1(H) - C_1(T)}{S_1(H) - S_1(T)}.$$

$\Delta_0$ (**delta**) is the change in the option's value divided by the
change in the stock's value — the number of shares needed to perfectly
hedge the option.

**Option price.** Substitute $\Delta_0$ back into (2), and $S_1(H)=uS_0$,
$S_1(T)=dS_0$:

$$C_1(T) = \left[\frac{C_1(H) - C_1(T)}{u-d}\right] d + \left(C_0 - \left[\frac{C_1(H) - C_1(T)}{u-d}\right]\right)(1+r).$$

Making $C_0$ the subject (multiply through, group the $C_1(H)$ and
$C_1(T)$ terms, use $\tfrac{u-d}{u-d}=1$ to combine the $C_1(T)$
coefficients):

$$C_0 = \frac{1}{1+r}\left[C_1(H)\cdot\frac{(1+r)-d}{u-d} + C_1(T)\cdot\frac{u-(1+r)}{u-d}\right].$$

## 5. Risk-neutral probabilities

The two fractions above are both in $[0,1]$ and sum to 1 — they behave like
probabilities, even though they came from pure algebra, not from anyone's
belief about which way the stock will move. Name them:

$$\tilde p = \frac{(1+r)-d}{u-d}, \qquad \tilde q = \frac{u-(1+r)}{u-d} = 1-\tilde p.$$

These are the **risk-neutral probabilities**. The pricing formula becomes

$$\boxed{C_0 = \frac{1}{1+r}\Big[\tilde p\,C_1(H) + \tilde q\,C_1(T)\Big]}$$

The price today is the *expected* future payoff under $\tilde p,\tilde q$,
discounted at the risk-free rate. Real-world probabilities of the stock
going up or down never enter — only the up/down factors $u,d$ and the
interest rate $r$ are needed. This is the seed of risk-neutral pricing,
developed fully in [[03 - FTAP, Risk-Neutral Measure and Martingales]].

---

**Next:** [[02 - The Two-Period and n-Period BOPM]] — extending the hedge to
multiple periods, and the general $n$-period formula.
