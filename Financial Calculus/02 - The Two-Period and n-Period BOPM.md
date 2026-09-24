# 02 — The Two-Period and n-Period BOPM

> A one-period hedge is set once and held. A two-period hedge is **adjusted**
> after the first toss, using whatever new information the toss revealed.
> Pricing the option means solving the replicating-portfolio equations
> backward through the tree, one period at a time — which turns out to
> collapse into the same risk-neutral expectation from Chapter 1, applied
> recursively. The reward for seeing this is the general $n$-period formula,
> a direct binomial-expansion sum.

---

## 1. Dynamic hedging

In a two-period model you set your hedge at time 0, observe the first toss,
then **re-hedge**: hold a new number of shares $\Delta_1$, with the
remaining wealth $(X_1 - \Delta_1 S_1)$ invested at the risk-free rate.
Wealth evolves as

$$X_2 = \Delta_1 S_2 + (1+r)\big[X_1 - \Delta_1 S_1\big].$$

The tree now has four terminal states:

$$S_2(HH) = u^2S_0, \quad S_2(HT) = S_2(TH) = udS_0, \quad S_2(TT) = d^2S_0.$$

## 2. Setting up the equations

Because the adjusted hedge $\Delta_1$ depends on what happened at time 1,
there are two *separate* systems of equations — one branch for each first
toss.

**If the first toss was heads:**

$$C_2(HH) = \Delta_1(H)\,S_2(HH) + (1+r)\big[X_1(H) - \Delta_1(H)S_1(H)\big] \qquad (1)$$
$$C_2(HT) = \Delta_1(H)\,S_2(HT) + (1+r)\big[X_1(H) - \Delta_1(H)S_1(H)\big] \qquad (2)$$

**If the first toss was tails:**

$$C_2(TH) = \Delta_1(T)\,S_2(TH) + (1+r)\big[X_1(T) - \Delta_1(T)S_1(T)\big] \qquad (3)$$
$$C_2(TT) = \Delta_1(T)\,S_2(TT) + (1+r)\big[X_1(T) - \Delta_1(T)S_1(T)\big] \qquad (4)$$

## 3. Solving for the adjusted hedge ratios

Exactly as in Chapter 1, subtract each pair to eliminate the cash/bond term:

$$\Delta_1(H) = \frac{C_2(HH) - C_2(HT)}{S_2(HH) - S_2(HT)}, \qquad \Delta_1(T) = \frac{C_2(TH) - C_2(TT)}{S_2(TH) - S_2(TT)}.$$

## 4. Valuing the portfolio at time 1

Because the portfolio replicates the option, its value equals the option's
value at every node: $C_1(H)=X_1(H)$ and $C_1(T)=X_1(T)$. There is no need
to redo the messy algebra of Chapter 1 — the risk-neutral pricing formula
applies directly at each node:

$$X_1(H) = C_1(H) = \frac{1}{1+r}\Big[\tilde p\,C_2(HH) + \tilde q\,C_2(HT)\Big],$$
$$X_1(T) = C_1(T) = \frac{1}{1+r}\Big[\tilde p\,C_2(TH) + \tilde q\,C_2(TT)\Big].$$

## 5. The full two-period formula

Substitute both into the single-period formula $C_0 = \frac{1}{1+r}[\tilde p\,C_1(H) + \tilde q\,C_1(T)]$:

$$C_0 = \frac{1}{1+r}\left[\tilde p\left(\frac{1}{1+r}\big[\tilde p\,C_2(HH)+\tilde q\,C_2(HT)\big]\right) + \tilde q\left(\frac{1}{1+r}\big[\tilde p\,C_2(TH)+\tilde q\,C_2(TT)\big]\right)\right]$$

$$= (1+r)^{-2}\Big[\tilde p^2 C_2(HH) + \tilde p\tilde q\,C_2(HT) + \tilde q\tilde p\,C_2(TH) + \tilde q^2 C_2(TT)\Big].$$

Since $C_2(HT) = C_2(TH)$ (the tree recombines), the two middle terms
combine:

$$\boxed{C_0 = (1+r)^{-2}\Big[\tilde p^2\,C_2(HH) + 2\tilde p\tilde q\,C_2(HT) + \tilde q^2\,C_2(TT)\Big]}$$

This mirrors the binomial expansion $(\tilde p+\tilde q)^2 = \tilde p^2 + 2\tilde p\tilde q + \tilde q^2$.
The coefficients $(1,2,1)$ are the number of paths reaching each terminal
state: 1 path to $HH$, 2 paths to one head and one tail, 1 path to $TT$.

## 6. Generalization: the n-period BOPM

Repeatedly applying backward induction from the final period $n$ back to
time 0 gives the general binomial option pricing formula:

$$\boxed{C_0 = (1+r)^{-n}\sum_{j=0}^{n}\binom{n}{j}\,\tilde p^{\,j}\,\tilde q^{\,n-j}\,C_n(j)}$$

Reading each piece:

- $(1+r)^{-n}$ — discounts the period-$n$ payoff back to today.
- $\sum_{j=0}^n$ — sums over every terminal state; $j$ is the number of
  up-moves.
- $\binom{n}{j}$ — the number of distinct paths through the tree with
  exactly $j$ up-moves and $n-j$ down-moves.
- $\tilde p^{\,j}\tilde q^{\,n-j}$ — the risk-neutral probability of any one
  specific path with $j$ up-moves and $n-j$ down-moves.

This is precisely $C_0 = (1+r)^{-n}\,\tilde{\mathbb E}_{\mathbb Q}[C_n]$, the
risk-neutral pricing formula proved in general in
[[03 - FTAP, Risk-Neutral Measure and Martingales]].

---

**Next:** [[03 - FTAP, Risk-Neutral Measure and Martingales]] — why this
recipe works: the Fundamental Theorem of Asset Pricing.
