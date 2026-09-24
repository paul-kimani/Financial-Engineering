# 00 — Lecture Notes Transcript

> Faithful transcription of the handwritten Financial Calculus notebook (photos in `Photos/`, IMG_3619 → IMG_3650). Written as it is on the page — wording, order and working kept. Where the handwriting has an obvious slip, it is kept and flagged with **[sic: …]**; where something is unreadable it is marked **[illegible]**. The polished chapter files (01–05) are the cleaned-up versions; this file is the source of truth for what was actually written in class.

---

## IMG_3619 — 18 Aug 2026

**FINANCIAL CALCULUS.**
**The Binomial Option Pricing Model (BOPM)**

The BOPM provides a powerful tool for understanding arbitrage theory & probability theory.

We use the model to understand the fundamental theorem of Arbitrage Pricing (FTAP).

Under the BOPM framework, stock prices are modelled in discrete time.

If the initial stock price $S_0$ & we assume that there are 2 positive numbers $d$ & $u$ where:

$$0 < d < 1 < u$$

such that:

$$S_0 \;\to\; \begin{cases} uS_0 \\ dS_0 \end{cases}$$

---

## IMG_3620

**After 1st toss:**

$$S_0 \to \begin{cases} S_1(H) = uS_0 \\ S_1(T) = dS_0 \end{cases}$$

**[sic:** the down branch is written $S_2(T)$ — should be $S_1(T)$**]**

**After 2nd toss:**

$$S_0 \to \begin{cases} S_1(H) \to \begin{cases} S_2(HH) = u^2S_0 \\ S_2(HT) = udS_0 \end{cases} \\[4pt] S_1(T) \to \begin{cases} S_2(HT) = udS_0 \\ S_2(TT) = d^2S_0 \end{cases} \end{cases}$$

(The middle node is shared — recombining tree.)

If we have a money market with interest rate $r$, & when we invest one unit this becomes $(1+r)$ in the next period. This implies that this equation holds when:

$$d < 1+r < u \quad \to \text{No arbitrage condition.}$$

**① What happens when $u < 1+r$?**

Since $(1+r)$ is given, we invest everything in the money market. This is because there exists an arbitrage opportunity. Therefore you can short sell the stock & invest the proceeds at the risk free interest & earn a guaranteed profit.

**A. Single Period BOPM.**

Consider a European call with strike $K > 0$.

$$\therefore \text{Payoff} = \max(S_T - K,\,0) = (S_T - K)^+$$

Suppose: at $t = 0$, we short the call for $C_0$ & therefore we are obligated to deliver / pay $(uS_0 - K)^+$ at maturity if $H$ & $(dS_0 - K)^+$ if $T$.

---

## IMG_3621

But what is the fair price of $C_0$?

- We need to construct a portfolio that replicates these exact obligations, regardless of whether the stock goes up or down.
- We hold a certain quantity $\Delta_0$ of the stock.
- Cash received from selling the call $(C_0)$ & less the cost of buying the stock $(\Delta_0 S_0)$.
- The remaining cash balance $[C_0 - \Delta_0 S_0]$ is invested in the risk free money market to earn interest rate $r$.

**At time 1:**

If the stock price goes up: $(H)$, our portfolio value $(X)$:

$$X_1(H) = \Delta_0 S_1(H) + [C_0 - \Delta_0 S_0](1+r)$$

If it goes down: $(T)$

$$X_1(T) = \Delta_0 S_1(T) + (C_0 - \Delta_0 S_0)(1+r)$$

For this to be a replicating portfolio, we need to mathematically show the exact stock quantity $(\Delta_0)$ such that the portfolio values match the call option's value in both states simultaneously:

$$C_1(H) = X_1(H) = \Delta_0 S_1(H) + [C_0 - \Delta_0 S_0](1+r) \quad \cdots (1)$$
$$C_1(T) = X_1(T) = \Delta_0 S_1(T) + [C_0 - \Delta_0 S_0](1+r) \quad \cdots (2)$$

We have a system of 2 equations with 2 unknowns $(\Delta_0 \;\&\; C_0)$.

**\* Finding the Hedge Ratio $(\Delta_0)$**

To isolate $\Delta_0$, we can eliminate the cash/bond portion by subtracting eq (2) from eq (1).

$$\therefore C_1(H) - C_1(T) = \Delta_0 S_1(H) - \Delta_0 S_1(T)$$

$$\Delta_0 = \frac{C_1(H) - C_1(T)}{S_1(H) - S_1(T)}$$

---

## IMG_3622

**NB!!:** $\Delta_0$ (Delta) represents the change in the option value divided by the change in the stock's value. It tells you how many shares of a stock you need to hold to perfectly hedge the option.

We can solve for $C_0$ by substituting $\Delta_0$ back into eq (2).

$$C_1(T) = \left[\frac{C_1(H) - C_1(T)}{S_1(H) - S_1(T)}\right] S_1(T) + \left(C_0 - \left[\frac{C_1(H) - C_1(T)}{S_1(H) - S_1(T)}\right] S_0\right)(1+r)$$

But: $S_1(H) = uS_0$ & $S_1(T) = dS_0$

$$C_1(T) = \left[\frac{C_1(H) - C_1(T)}{uS_0 - dS_0}\right] dS_0 + \left(C_0 - \left[\frac{C_1(H) - C_1(T)}{uS_0 - dS_0}\right] S_0\right)(1+r)$$

$$C_1(T) = \left[\frac{C_1(H) - C_1(T)}{u - d}\right] d + \left(C_0 - \left[\frac{C_1(H) - C_1(T)}{u - d}\right]\right)(1+r)$$

**[sic:** a couple of lines write $C_2(T)$ in the numerator — should be $C_1(T)$ throughout**]**

Making $C_0$ the subject:

$$C_1(T) + \left[\frac{C_1(H) - C_1(T)}{u-d}\right](1+r) - \left[\frac{C_1(H) - C_1(T)}{u-d}\right] d = C_0(1+r)$$

$$C_1(T) + \left[\frac{C_1(H) - C_1(T)}{u-d}\right]\big((1+r) - d\big) = C_0(1+r)$$

$$\left[C_1(T) - \frac{C_1(T)[(1+r)-d]}{u-d}\right] + \frac{C_1(H)[(1+r)-d]}{u-d} = C_0(1+r)$$

$$C_1(T)\left[\frac{u-d}{u-d} - \frac{(1+r)-d}{u-d}\right] + \cdots$$

$$C_1(T)\left[\frac{u-(1+r)}{u-d}\right] + \frac{C_1(H)[(1+r)-d]}{u-d}$$

$$\therefore\; C_0 = \frac{1}{(1+r)}\left[C_1(H)\left(\frac{(1+r)-d}{u-d}\right) + C_1(T)\left(\frac{u-(1+r)}{u-d}\right)\right] \;\;/\!/$$

---

## IMG_3623

Instead of dealing with real world probabilities, the model substitutes the fractional algebraic terms with artificial probabilities denoted by $\tilde p$ & $\tilde q$:

$$\tilde p = \frac{(1+r) - d}{u - d}$$

$$\tilde q = \frac{u - (1+r)}{u - d} = (1 - \tilde p)$$

The final formula becomes:

$$C_0 = (1+r)^{-1}\left[\tilde p\, C_1(H) + \tilde q\, C_1(T)\right]$$

This means that the price of an option today $(C_0)$ is the expected future payoff of the option under a risk-neutral measure, discounted back to the present at the risk free rate.

You don't need to know investors' risk appetites or real-world probabilities of the stock going up or down; you only need the up/down factors & the interest rate.

**2-Period Binomial Model.**

In a single period model, you set your hedge at time 0 & hold it to time 1. In a 2-period model, you have the opportunity to adjust your hedge after the first period (toss 1) based on whether the stock went up/down.

- At time 1, investor has wealth equal to $X_1$ \$.
- They decide to adjust their portfolio by holding a new amount of shares $\Delta_1$.
- Just like in period 1, the remainder of the wealth, $(X_1 - \Delta_1 S_1)$, is invested in the money market.

$\therefore$ in the next period (time 2), the investor's wealth $(C_2)$ will be calculated as:

$$C_2 = \Delta_1 S_2 + (1+r)\{X_1 - \Delta_1 S_1\}$$

---

## IMG_3624

**Expanding the tree…**

$$S_0 \to \begin{cases} S_1(H) \to \begin{cases} S_2(HH) = u^2S_0 \\ S_2(HT) = udS_0 \end{cases} \\[4pt] S_1(T) \to \begin{cases} S_2(HT) \\ S_2(TT) = d^2S_0 \end{cases} \end{cases}$$

**\* Setting up the Equations.**

To price the option, we must ensure our replicating portfolio matches the option's payoff at time 2 for every possible path. Because the new hedge ratio $(\Delta_1)$ depends entirely on what happened at time 1, we get 2 distinct sets of equations:

If the first toss was heads $(H)$:

$$(1)\quad C_2(HH) = \Delta_1(H) S_2(HH) + (1+r)\{X_1(H) - \Delta_1(H) S_1(H)\}$$
$$(2)\quad C_2(HT) = \Delta_1(H) S_2(HT) + (1+r)\{X_1(H) - \Delta_1(H) S_1(H)\}$$

If the first toss was tails $(T)$:

$$(3)\quad C_2(TH) = \Delta_1(T) S_2(TH) + (1+r)\{X_1(T) - \Delta_1(T) S_1(T)\}$$
$$(4)\quad C_2(TT) = \Delta_1(T) S_2(TT) + (1+r)\{X_1(T) - \Delta_1(T) S_1(T)\}$$

The immediate goal established is to solve for the adjusted hedge ratio $(\Delta_1(H) \;\&\; \Delta_1(T))$ & the portfolio values at time 1 $(X_1(H) \;\&\; X_1(T))$.

**\* Solving for the Hedge Ratios $(\Delta_1)$**

The method here is identical to the one used for the single period model:

$$C_2(TH) - C_2(TT) = \Delta_1(T) S_2(TH) - \Delta_1(T) S_2(TT)$$

Isolating $\Delta_1(T)$:

---

## IMG_3625

$$\Delta_1(T) = \frac{C_2(TH) - C_2(TT)}{S_2(TH) - S_2(TT)}$$

Subtracting equation (2) from (1):

$$C_2(HH) - C_2(HT) = \Delta_1(H) S_2(HH) - \Delta_1(H) S_2(HT)$$

$$\Delta_1(H) = \frac{C_2(HH) - C_2(HT)}{S_2(HH) - S_2(HT)}$$

**[sic:** the denominator is written $S_1(HH)$ — should be $S_2(HH)$**]**

**\* Valuing the portfolio at time 1 using Risk neutral probabilities.**

To find the value of the portfolio at time 1 $(X_1(H) \;\&\; X_1(T))$, we use the risk neutral valuation approach which simplifies the process.

The core principle stated here is that the value of the ~~value of the~~ portfolio:

$$C_1(H) = X_1(H)$$
$$C_1(T) = X_1(T)$$

Therefore, we can simply jump straight to the risk-neutral formulas for time 1:

$$X_1(T) = C_1(T) = (1+r)^{-1}\{\tilde p\, C_2(TH) + \tilde q\, C_2(TT)\}$$
$$X_1(H) = C_1(H) = (1+r)^{-1}\{\tilde p\, C_2(HH) + \tilde q\, C_2(HT)\}$$

To get the full 2-period BOPM formula, we simply take the formulas for $C_1(T)$ & $C_1(H)$ & substitute directly into:

$$C_0 = (1+r)^{-1}\{\tilde p\, C_1(H) + \tilde q\, C_1(T)\}$$

$$C_0 = (1+r)^{-1}\Big[\tilde p\big\{(1+r)^{-1}[\tilde p\, C_2(HH) + \tilde q\, C_2(HT)]\big\} + \tilde q\big\{(1+r)^{-1}[\tilde p\, C_2(TH) + \tilde q\, C_2(TT)]\big\}\Big]$$

$$= (1+r)^{-2}\left[\tilde p^2 C_2(HH) + \tilde p\tilde q\, C_2(HT) + \tilde q\tilde p\, C_2(TH) + \tilde q^2 C_2(TT)\right]$$

$$= (1+r)^{-2}\left[\tilde p^2 C_2(HH) + 2\tilde p\tilde q\, C_2(HT) + \tilde q^2 C_2(TT)\right]$$

**[sic:** the last term is written $\tilde q^2(TT)$ — missing the $C_2$**]**

---

## IMG_3626

The equation directly mirrors the binomial expansion $(p+q)^2 = p^2 + 2pq + q^2$. The coefficients $(1, 2, 1)$ represent the number of possible paths to reach each state. There is 1 path to 2 heads, 2 paths to get one head & one tail, & 1 path to get 2 tails.

**Generalization for the n period BOPM.**

The core principle remains identical regardless of the number of periods: the value of a derivative at any node is simply the discounted expected value of its future payoff at the next time step, calculated using risk neutral probabilities.

By repeatedly applying the backward induction from the final period $(n)$ all the way back to time 0, we get the general n-period Binomial Option Pricing Formula:

$$C_0 = (1+r)^{-n}\sum_{j=0}^{n}\binom{n}{j}\tilde p^{\,j}\,\tilde q^{\,n-j}\,C_n(j)$$

Let's decipher what each piece means:

- $(1+r)^{-n}$: This discounts the final payoff from period $n$ back to period 0.
- $\sum_{j=0}^{n}$: This represents summing up the values across all possible terminal states. $j$ represents the number of times the stock goes up.
- $\binom{n}{j}$: This is the binomial coefficient ($n$ choose $j$). It calculates the exact number of paths through the tree that can result in exactly $j$ up-moves & $n-j$ down moves.
- $\tilde p^{\,j}\tilde q^{\,n-j}$: This calculates the risk neutral probability of traveling along one specific path that has $j$ up-moves & $n-j$ down-moves.

---

## IMG_3627

**The Fundamental Theorem of Asset Pricing (FTAP)**

**Core Market Assumptions.**

- **Unlimited short-selling:** You can short-sell the stock without any restrictions or borrowing limits.
- **Unlimited Borrowing:** You have infinite access to capital to fund your positions.
- **Frictionless Trading:** There are absolutely no transaction costs.
- **Price taker Status:** The agent is categorized as a small investor. This means your individual trades are too small to influence or move the overall market price.

The FTAP can be explained by these 2 equivalent statements.

1. **No Arbitrage:** The market admits no arbitrage if and only if the market has a martingale measure (Probability measure $\mathbb{Q}$).
2. **Market Completeness:** The martingale measure is unique if & only if ~~the market has~~ every contingent claim can be hedged. This is mathematically expressed as $X_n = C_n$ (the value of the replicating portfolio equals the value of the contingent claim).

**The concept of a complete market.**

In a complete market, any contingent claim can be completely replicated by a portfolio of the underlying assets.

Because this perfect replication is possible, it means there is a unique, unambiguous way to complete the fair value of any asset using risk-neutral valuation. **[sic:** "complete" — probably meant "compute"**]**

---

## IMG_3628

**Defining the Risk-Neutral Measure.**

Imagine a set of possible outcomes from $K$ coin tosses:

We construct an artificial probability measure denoted by $\mathbb{Q}$, where $\tilde p$ is the probability of tossing a head (stock goes up) & $\tilde q$ is the probability of tossing a tail.

These are called your risk neutral probabilities, & the expected value calculated using them is denoted by $\tilde{\mathbb{E}}_{\mathbb{Q}}$.

This leads to the simplified pricing formula:

$$C_0 = \tilde{\mathbb{E}}_{\mathbb{Q}}\left(\frac{1}{1+r}\,C_1\right)$$

… the price today is the discounted expected payoff tomorrow.

**Theorem 1: The Martingale Property.**

This is a very critical theorem. It states that under these risk-neutral probabilities, the discounted stock price process is a martingale.

**Notes:**

For a martingale, we must prove that the conditional expectation of the future discounted price, given all current information, is exactly equal to the current discounted price.

Mathematically, if you discount tomorrow's expected stock price back to today, it should perfectly equal today's actual stock price.

The goal is to prove that the discounted stock price is a martingale under the risk neutral measure. Mathematically, we want to show that:

$$\tilde{\mathbb{E}}_{\mathbb{Q}}\left((1+r)^{-(n+1)} S_{n+1} \,\middle|\, \mathcal{F}_n\right) = (1+r)^{-n} S_n.$$

**Step 1: Setup the Conditional Expectation.**

We start by writing the expected value of the discounted stock price at time $n+1$, given the information set $\mathcal{F}_n$ available at time $n$:

---

## IMG_3629

$$\tilde{\mathbb{E}}_{\mathbb{Q}}\left((1+r)^{-(n+1)} S_{n+1} \,\middle|\, \mathcal{F}_n\right)$$

**Step 2: Expand the future Price $(S_{n+1})$**

At time $(n+1)$ the stock price will either be $uS_n$ (with probability $\tilde p$) or $dS_n$ (with probability $\tilde q$). Because $(1+r)^{-(n+1)}$ is a constant, we can pull it out & apply the probabilities directly to the future states:

$$(1+r)^{-(n+1)}\{\tilde p\, uS_n + \tilde q\, dS_n\}$$

**Step 3: Factor out the Current Price.**

$$(1+r)^{-(n+1)} \cdot S_n\{\tilde p u + \tilde q d\}$$

**Step 4: Substitute the Risk Neutral Probabilities.**

$$\tilde p = \frac{(1+r) - d}{u - d} \qquad \tilde q = \frac{u - (1+r)}{u - d}$$

$$= (1+r)^{-(n+1)} \cdot S_n\left(\frac{(1+r) - d}{u - d}\cdot u + \frac{u - (1+r)}{u - d}\cdot d\right)$$

**Step 5: Expand the numerators.**

$$= (1+r)^{-(n+1)} \cdot \frac{S_n}{u - d}\left(u(1+r) - ud + ud - d(1+r)\right)$$

**Step 6: Cancel Terms & Factor.**

$$= (1+r)^{-(n+1)} \cdot \frac{S_n}{u - d}\left((1+r)(u - d)\right)$$

**Step 7: Final Reduction.**

$$= (1+r)^{-(n+1)} \cdot S_n (1+r)^1$$

$$= S_n (1+r)^{-n}$$

Hence this proves that the conditional expectation of tomorrow's discounted price exactly equals today's discounted price.

---

## IMG_3630

Similarly one can show that:

$$C_0 = X_0 = S_0 = \frac{1}{(1+r)}\,\mathbb{E}_{\mathbb{Q}}(S_1)$$

**[sic:** "$C_0 = X_0 =$" is carried in front of every line here — the claim being proved is only $S_0 = \frac{1}{1+r}\mathbb{E}_{\mathbb{Q}}(S_1)$; the stock is being priced like a claim that pays $S_1$**]**

**Proof:**

$$\mathbb{E}_{\mathbb{Q}}[S_1] = \tilde p\, S_1(H) + \tilde q\, S_1(T) = \tilde p\, uS_0 + \tilde q\, dS_0$$

$$C_0 = S_0 = \frac{1}{(1+r)}\{\tilde p\, uS_0 + \tilde q\, dS_0\}$$

but $\tilde p = \dfrac{(1+r) - d}{u - d}$ & $\dfrac{u - (1+r)}{u - d} = \tilde q$

$$\therefore C_0 = S_0 = \frac{1}{(1+r)}\left(\frac{(1+r) - d}{u - d}\cdot uS_0 + \frac{u - (1+r)}{u - d}\cdot dS_0\right)$$

$$C_0 = S_0 = \frac{S_0}{(1+r)}\left(\frac{(1+r) - d}{u - d}\cdot u + \frac{u - (1+r)}{u - d}\cdot d\right)$$

$$C_0 = S_0 = \frac{S_0}{(1+r)}\left(\frac{(u - d)(1+r)}{u - d}\right) = S_0$$

**Theorem 2:** Under $\tilde{\mathbb{Q}}$ the discounted self-financing process value:

$$\left((1+r)^{-(n+1)} X_{n+1} \,\middle|\, \mathcal{F}_n\right)_{n=0}^{N} \text{ is a martingale.}$$

The value of the portfolio at time $n+1$, denoted as $X_{n+1}$, is equal to:

$$X_{n+1} = \Delta_n S_{n+1} + (1+r)\{X_n - \Delta_n S_n\}$$

This equation represents the value of the portfolio holding $\Delta_n$ shares of stock & a risk-free bond. The value of the bond is the remaining wealth $(X_n - \Delta_n S_n)$ compounded at the risk free rate.

$$\mathbb{E}_{\mathbb{Q}}\left((1+r)^{-(n+1)} X_{n+1} \,\middle|\, \mathcal{F}_n\right)$$

---

## IMG_3631 — 08 Sept 2026

**THE BLACK-SCHOLES PDE**

The Black Scholes PDE for an option $V$ is given by:

$$\underbrace{\frac{\partial V}{\partial t}}_{\Theta} + \underbrace{\frac12\frac{\partial^2V}{\partial S^2}\sigma^2S_t^2}_{\Gamma} + \underbrace{\frac{\partial V}{\partial S}}_{\Delta}rS - rV = 0$$

**$\dfrac{\partial V}{\partial t}$ = Theta $(\Theta)$.** It represents the time decay of the option value. It captures how the option's value erodes as it gets closer to its expiration date.

**$rS\dfrac{\partial V}{\partial S}$.** It represents the expected growth in the option's value due to the stock movement at the risk free rate $r$. The term combines the option's sensitivity to the stock price, known as the delta $\Delta = \dfrac{\partial V}{\partial S}$, [with the] risk free rate $r$ to reflect the potential for an increase in the option's value.

**$\frac12\sigma^2S^2\dfrac{\partial^2V}{\partial S^2}$.** It contains the stock volatility $(\sigma)$ & the option's gamma $\left(\partial^2V/\partial S^2 = \Gamma\right)$ to capture the effect of the stock's random price movements on the option's value. It represents the value created by the convexity of the option.

**$rV$ = opportunity cost term.** It represents the opportunity cost of holding the option. By holding the option, you forgo the opportunity to invest its value, $V$, at the risk free rate $r$.

---

## IMG_3632

It can be derived:

1. By hedging — original derivation of the BSE
2. The replicating portfolio.
3. By the Capital Asset Pricing model (CAPM)
4. As a limiting case of Cox-Ross-Rubinstein (CRR) BOPM.

**① BSM PDE by Hedging.**

The Black-Scholes PDE can be derived by constructing a riskless portfolio that is hedged against changes in the underlying stock price.

The portfolio $\Pi$ is composed of one contingent claim (an option) & a short position of $\Delta$ units of the underlying stock.

The value of the portfolio is given by:

$$\Pi = V - \Delta S$$

For any small change in time, the change in the portfolio's value $d\Pi$ can be expressed as:

$$d\Pi = dV - \Delta\, dS$$

The change in the value of the option $dV$ is given by Itô's Lemma:

$$dV = \left(\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}\right)dt + \sigma S\frac{\partial V}{\partial S}\,dW$$

Similarly, the change in the stock price $dS$ follows a GBM:

$$dS = \mu S\,dt + \sigma S\,dW$$

Substituting the values of $dV$ & $dS$ in the $d\Pi$:

$$d\Pi = \left(\left[\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}\right]dt + \sigma S\frac{\partial V}{\partial S}\,dW\right) - \Delta\{\mu S\,dt + \sigma S\,dW\}$$

---

## IMG_3633

$$d\Pi = \left[\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} - \Delta\mu S\right]dt + \left[\sigma S\frac{\partial V}{\partial S} - \Delta\sigma S\right]dW$$

To make the option riskless the change in its value must be independent of the random wiener process $dW$. This means the coefficient of $dW$ terms must be zero:

$$\sigma S\frac{\partial V}{\partial S} - \Delta\sigma S = 0$$

$$\sigma S\left[\frac{\partial V}{\partial S} - \Delta\right] = 0$$

$$\Rightarrow \Delta = \frac{\partial V}{\partial S} \quad \text{(delta hedge ratio)}$$

Substituting $\Delta$ back into the equation $d\Pi$:

$$d\Pi = \left[\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} - \mu S\frac{\partial V}{\partial S}\right]dt + 0$$

$$d\Pi = \left(\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}\right)dt$$

Because the portfolio is now riskless, its return must equal the risk-free rate $r$. This is based on the principle of no arbitrage, which states that a riskless portfolio should not generate a return higher or lower than the risk free rate.

Therefore the change in the portfolio's value, $d\Pi$, must be equal to the risk-free return on the portfolio's current value $\Pi$:

$$d\Pi = r\Pi\,dt$$
$$d\Pi = r(V - \Delta S)\,dt$$

---

## IMG_3634

Equating the 2 equations of $d\Pi$:

$$\left[\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}\right]dt = r(V - \Delta S)\,dt$$

$$\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = rV - r\Delta S$$

But $\Delta = \dfrac{\partial V}{\partial S}$

$$\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = rV - rS\frac{\partial V}{\partial S}$$

Rearranging:

$$\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} + rS\frac{\partial V}{\partial S} - rV = 0$$

**BSM PDE By ~~Hedging~~ Replicating Portfolio.**

The BSM PDE can be derived using the concepts of pricing by arbitrage.

This method assumes that in a complete market, any derivative can be priced by constructing a self-financing replicating strategy that produces the same ~~portfolio~~ payoff as the derivative.

The value of the derivative must be equal to the value of its replicating portfolio at all times to prevent arbitrage opportunities.

**① Setting up the replicating portfolio & Self financing strategy.**

A self-financing trading strategy is one where the change in the portfolio's value is due only to changes in the value of the assets & not to external fund flows.

---

## IMG_3635

To replicate the derivative $V$, we form a self financing portfolio comprising of a stock $(S)$ & a Bond $(B)$. The value of the portfolio at time $t$ is:

$$\Pi(t) = a(t)S(t) + b(t)B(t)$$

- $a(t)$ — No. of units of stock.
- $b(t)$ — No. of units of bonds.

The self-financing assumption implies that the change in the portfolio value $d\Pi$ is given by:

$$d\Pi = a\,dS + b\,dB$$

This implies that the value of the derivative $V$ is equal to the value of the replicating portfolio:

$$dV = d\Pi$$
$$dV = a\,dS + b\,dB.$$

But the stochastic processes for the stock & the Bond are:

- $dS = \mu S\,dt + \sigma S\,dW_t$
- $dB = rB\,dt$

By Itô's Lemma, the change in the option's value $dV$ is given by:

$$dV = \left(\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}\right)dt + \left(\sigma S\frac{\partial V}{\partial S}\right)dW_t$$

**② Deriving the PDE by equating the stochastic & deterministic parts:**

Substitute the expressions for $dV$, $dS$ & $dB$ into the self financing equation:

---

## IMG_3636

$$dV = a\,dS + b\,dB$$

$$\left(\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}\right)dt + \left(\sigma S\frac{\partial V}{\partial S}\right)dW_t = a(\mu S\,dt + \sigma S\,dW_t) + b(rB\,dt)$$

$$= (a\mu S + rbB)\,dt + (a\sigma S)\,dW_t$$

**[sic:** the first $\partial V/\partial t$ is written $\partial V/\partial S$ on both lines of the left-hand side, and $rbB$ is written $rdB$ — the next page uses the correct forms**]**

For a portfolio to be riskless, the stochastic term must be eliminated, which is achieved by setting the coefficients of $dW$ on both sides equal to each other:

$$\sigma S\frac{\partial V}{\partial S}(dW) = a\sigma S(dW)$$

**[sic:** written with a "+" between the two sides**]**

$$a = \frac{\partial V}{\partial S} \quad \text{(delta) No. of stocks needed to replicate the derivative value.}$$

Equating the coefficients of the deterministic term $(dt)$:

$$\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = a\mu S + rbB.$$

But $a = \dfrac{\partial V}{\partial S}$.

$$\frac{\partial V}{\partial t} + \underline{\mu S\frac{\partial V}{\partial S}} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = \underline{\mu S\frac{\partial V}{\partial S}} + rbB.$$

$$\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = rbB$$

From the portfolio value equation: $\Pi = V = aS + bB$, therefore we can solve for $bB$:

$$V = aS + bB$$
$$\Rightarrow bB = V - aS; \quad \text{But } a = \partial V/\partial S$$
$$\Rightarrow bB = V - S\,\partial V/\partial S$$

---

## IMG_3637

Substitute the value of $bB$ [into the] equation of the deterministic terms:

$$\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = r\left(V - S\frac{\partial V}{\partial S}\right)$$

$$\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = rV - rS\frac{\partial V}{\partial S}$$

To get the PDE, rearrange the terms & equate to zero:

$$\frac{\partial V}{\partial t} + rS\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} - rV = 0 \quad \text{— PDE.}$$

**BSM PDE FROM CAPM.**

**① CAPM & Expected Return**

The CAPM states that the expected return on any risky asset is equal to the risk free rate plus a risk premium which is proportional to the asset's systematic risk $(\beta)$.

$$\mathbb{E}(R_i) = r_f + \beta_i\big(\mathbb{E}(R_m) - r_f\big) \qquad (R_m = \text{market})$$

$$\underbrace{\mathbb{E}(R_i) - r_f}_{\text{asset } i} = \underbrace{\beta_i}_{\text{systematic risk}}\underbrace{\big(\mathbb{E}(R_m) - r_f\big)}_{\text{risk premium}}$$

**② Applying CAPM to the stock $(S)$ & the Option $(V)$**

Assume the value of the option is a function of the stock price & time:

---

## IMG_3638

$$V = V(S, t)$$

**⇒ For stock $(S)$:** The expected return on the stock is the drift value $\mu$. The Beta of the stock is $\beta_S$:

$$\mathbb{E}(R_S) = \mu$$
$$\mu = r + \beta_S\{\mathbb{E}[R_m] - r\}$$

**⇒ For the option $(V)$:** The expected return is the expected change in its value, $\mathbb{E}[dV]$, divided by its current value $V$. The beta of the option is $\beta_V$:

$$\mathbb{E}[R_V] = \frac{\mathbb{E}[dV]}{V}$$
$$\frac{\mathbb{E}[dV]}{V} = r + \beta_V\{\mathbb{E}(R_m) - r\}$$

**③ Finding the relationship between the Betas $(\beta_V \;\&\; \beta_S)$:**

The relationship comes from the fact the price movements of the option are directly tied to the price movements of the stock.

The Beta of any asset $i$ is:

$$\beta_i = \frac{\operatorname{Cov}(R_i, R_m)}{\operatorname{Var}(R_m)}$$

The $\beta$ of the stock & option is given by:

$$\beta_S = \frac{\operatorname{Cov}(R_S, R_m)}{\operatorname{Var}(R_m)} \qquad \beta_V = \frac{\operatorname{Cov}(R_V, R_m)}{\operatorname{Var}(R_m)}$$

---

## IMG_3639

For infinitesimal returns (continuous time) we have:

$$dR_S = \frac{dS}{S}, \qquad dR_V = \frac{dV}{V}$$

Expressing $dV$ using Itô's lemma, which describes how a function of a stochastic process evolves, given the stock price follows GBM:

$$dS = \mu S\,dt + \sigma S\,dW_t$$

For the change in share price $V(S_t)$ is:

$$dV = \left(\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}\right)dt + \left(\sigma S\frac{\partial V}{\partial S}\right)dW_t$$

**[sic:** "change in share price" — this is the change in the option value $V(S_t)$; the $dW_t$ coefficient is written "$\partial S\,\partial V/\partial S$", should be $\sigma S\,\partial V/\partial S$**]**

The covariance of a change in 2 variables $\operatorname{Cov}(dx, dy)$ is only dependent on their stochastic component. The deterministic term $(dt)$ doesn't contribute to the covariance with a stochastic process.

$$\operatorname{Cov}(dV, dS) = \operatorname{Cov}\left(\sigma S\frac{\partial V}{\partial S}dW_t,\; (\sigma S)\,dW_t\right)$$

$$\therefore \operatorname{Cov}(dV, dS) = \left(\sigma S\frac{\partial V}{\partial S}(\sigma S)\operatorname{Cov}(dW_t, dW_t)\right)$$

$$\operatorname{Cov}(dV, dS) = \left(\sigma S\frac{\partial V}{\partial S}(\sigma S)\,dt\right)$$

For the betas:

$$\beta_V = \frac{\operatorname{Cov}(dR_V, dR_M)}{\operatorname{Var}(dR_M)} \quad \& \quad \beta_S = \frac{\operatorname{Cov}(dR_S, dR_M)}{\operatorname{Var}(dR_M)}$$

Rewriting the covariance terms using the infinitesimal change:

$$\operatorname{Cov}(dR_V, dR_M) = \operatorname{Cov}\left(\frac{dV}{V}, \frac{dM}{M}\right)$$
$$\operatorname{Cov}(dR_S, dR_M) = \operatorname{Cov}\left(\frac{dS}{S}, \frac{dM}{M}\right)$$

---

## IMG_3640

Since the market portfolio is a function of the underlying assets, its stochastic part is also driven by $dW_t$:

$$\operatorname{Cov}(dR_V, dR_M) = \operatorname{Cov}\left(\frac{\sigma S\frac{\partial V}{\partial S}\,dW_t}{V},\; \frac{\sigma_M M\,dW_t}{M}\right)$$

**[sic:** the $\sigma$ in the first slot is written as $\partial$**]**

$$\operatorname{Cov}(dR_V, dR_M) = \frac{\sigma S}{V}\cdot\frac{\partial V}{\partial S}\cdot\sigma_M\operatorname{Cov}(dW_t, dW_t) = \frac{\sigma S}{V}\cdot\frac{\partial V}{\partial S}\,\sigma_M\,dt.$$

$$\operatorname{Cov}(dR_S, dR_M) = \frac{\sigma S}{S}\cdot\sigma_M\operatorname{Cov}(dW_t, dW_t) = \frac{\sigma S}{S}\cdot\sigma_M\,dt = (\sigma\sigma_M\,dt)$$

Taking the ratios:

$$\frac{\beta_V}{\beta_S} = \frac{\operatorname{Cov}(dR_V, dR_M)}{\operatorname{Var}(dR_M)} \div \frac{\operatorname{Cov}(dR_S, dR_M)}{\operatorname{Var}(dR_M)}$$

$$\frac{\beta_V}{\beta_S} = \frac{\operatorname{Cov}(dR_V, dR_M)}{\operatorname{Cov}(dR_S, dR_M)} = \frac{\sigma\sigma_M\cdot\frac{S}{V}\frac{\partial V}{\partial S}\,dt}{\sigma\sigma_M\,dt}$$

$$\beta_V = \frac{S}{V}\cdot\frac{\partial V}{\partial S}\,\beta_S.$$

Substituting the relationship from step 2 back into the CAPM equation for the option:

$$\frac{\mathbb{E}[dV]}{V} = r + \beta_V\big(\mathbb{E}[R_m] - r\big)$$
$$= r + \left(\frac{S}{V}\cdot\frac{\partial V}{\partial S}\cdot\beta_S\right)\big(\mathbb{E}[R_m] - r\big)$$

From the CAPM equation of the stock we know that:

$$\beta_S\big(\mathbb{E}(R_m) - r\big) = \mu - r$$

---

## IMG_3641

$$\frac{\mathbb{E}[dV]}{V} = r + \frac{\partial V}{\partial S}\cdot\frac{S}{V}\{\mu - r\}$$

$$\mathbb{E}[dV] = rV + \mu S\frac{\partial V}{\partial S} - rS\frac{\partial V}{\partial S}$$

**[sic:** the left-hand side should carry "$\times\,dt$" on the right — $\mathbb{E}[dV]$ is a $dt$-sized quantity; it cancels in the next step so the result is unaffected**]**

Equating the expression for $\mathbb{E}[dV]$ from the Itô's lemma: the expected change in the option's value is the deterministic part of the $dV$:

$$\mathbb{E}[dW_t] = 0$$

$$\mathbb{E}[dV] = \frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2}$$

Equating $\mathbb{E}[dV]$ from the Itô's & CAPM, we have:

$$\frac{\partial V}{\partial t} + \mu S\frac{\partial V}{\partial S} + \frac12\sigma^2\frac{\partial^2V}{\partial S^2} = rV + \mu S\frac{\partial V}{\partial S} - rS\frac{\partial V}{\partial S}$$

**[sic:** the $S^2$ is dropped on this line; it's back on the next**]**

$$\frac{\partial V}{\partial t} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = rV - rS\frac{\partial V}{\partial S}$$

Rearranging:

$$\frac{\partial V}{\partial t} + rS\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} - rV = 0.$$

**The BSM PDE in terms of the log Stock Price.**

The BSM PDE is a fundamental tool for pricing options. The original form is expressed in terms of the stock price $(S)$.

*Derive the equivalent PDE in terms of the log-stock price, $x = \ln S$, & show all the necessary steps, including the transformation of the 1st & 2nd partial derivatives.*

---

## IMG_3642

**Soln.**

BSM PDE: $\dfrac{\partial V}{\partial t} + rS\dfrac{\partial V}{\partial S} + \dfrac12\sigma^2S^2\dfrac{\partial^2V}{\partial S^2} - rV = 0$

The transformation is $x = \ln S$

$$\Rightarrow S = e^x$$

Express $\dfrac{\partial V}{\partial S}$ in terms of $x$, using the chain rule for partial derivatives.

$$\frac{\partial V}{\partial S} = \frac{\partial V}{\partial x}\cdot\frac{\partial x}{\partial S}$$

but $x = \ln S$, $\dfrac{\partial x}{\partial S} = \dfrac1S$

$$\boxed{\frac{\partial V}{\partial S} = \frac{\partial V}{\partial x}\cdot\frac1S}$$

Express $\dfrac{\partial^2V}{\partial S^2}$ in terms of $x$ (chain rule for partial derivatives & product rule)

$$\frac{\partial^2V}{\partial S^2} = \frac{\partial}{\partial S}\left(\frac{\partial V}{\partial S}\right) = \frac{\partial}{\partial S}\left(\frac{\partial V}{\partial x}\cdot\frac1S\right)$$

By product rule: $\dfrac{\partial(uw)}{\partial S} = u\dfrac{\partial w}{\partial S} + w\dfrac{\partial u}{\partial S}$.

$$u = 1/S, \qquad w = \frac{\partial V}{\partial x}$$

$$\frac{\partial u}{\partial S} = \frac{\partial}{\partial S}\left(\frac1S\right) = \frac{\partial(S^{-1})}{\partial S} = -S^{-2} = \boxed{-\frac{1}{S^2}}$$

$$\frac{\partial w}{\partial S} = \frac{\partial}{\partial S}\left(\frac{\partial V}{\partial x}\right) \;\to\; \text{Apply chain rule as from the first derivative.}$$

$$= \frac{\partial}{\partial x}\left(\frac{\partial V}{\partial x}\right)\cdot\frac{\partial x}{\partial S} = \frac{\partial^2V}{\partial x^2}\cdot\frac{\partial x}{\partial S} = \boxed{\frac{\partial^2V}{\partial x^2}\cdot\frac1S}$$

---

## IMG_3643

$$\frac{\partial^2V}{\partial S^2} = u\frac{\partial w}{\partial S} + w\frac{\partial u}{\partial S} \quad \ldots\text{(product rule)}$$

$$= \frac1S\left(\frac{\partial^2V}{\partial x^2}\cdot\frac1S\right) - \frac1{S^2}\cdot\frac{\partial V}{\partial x}$$

From PDE:

$$rS\frac{\partial V}{\partial S} = rS\left(\frac1S\cdot\frac{\partial V}{\partial x}\right) = r\frac{\partial V}{\partial x}$$

$$\frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = \frac12\sigma^2S^2\left(\frac1{S^2}\cdot\frac{\partial^2V}{\partial x^2} - \frac1{S^2}\frac{\partial V}{\partial x}\right) = \frac12\sigma^2\left(\frac{\partial^2V}{\partial x^2} - \frac{\partial V}{\partial x}\right)$$

The PDE in terms of log-stock price is:

$$\frac{\partial V}{\partial t} + r\frac{\partial V}{\partial x} + \frac12\sigma^2\left(\frac{\partial^2V}{\partial x^2} - \frac{\partial V}{\partial x}\right) - rV = 0.$$

**FEYNMAN-KAC THEOREM**

The Feynman-Kac Theorem establishes a connection between a Stochastic Differential Equation (SDE) & a Partial Differential Equation (PDE).

It's a key result particularly for pricing options as it allows us to solve a PDE by calculating an expectation under a risk-neutral measure.

The theorem states that the solution to certain PDEs can be represented as a conditional expected value of a function of a stochastic process.

Consider a stochastic process $X_t$ that follows the SDE:

$$dX_t = \mu(X_t, t)\,dt + \sigma(X_t, t)\,dW_t$$

Also, define a function:

$$F(t, x) = \mathbb{E}_{t,x}\left[\Phi(X_T)\right]$$

---

## IMG_3644

Therefore the Feynman-Kac Theorem states that the function $F(t, x)$ is the solution to the PDE:

$$\frac{\partial F}{\partial t} + \mu(t,x)\frac{\partial F}{\partial x} + \frac12\sigma^2(t,x)\frac{\partial^2F}{\partial x^2} = 0$$

With the terminal condition:

$$F(T, x) = \Phi(x)$$

**Proof:**

To find the stochastic representation formula, we apply the Itô's lemma formula to the function $F(t, X_t)$:

$$dF(t, X_t) = \left[\frac{\partial F}{\partial t} + \mu(X_t, t)\frac{\partial F}{\partial x} + \frac12\sigma^2(X_t, t)\frac{\partial^2F}{\partial x^2}\right]dt + \sigma(X_t, t)\frac{\partial F}{\partial x}\,dW_t.$$

But the deterministic term is the same as the PDE which equals to 0 ($dt$ term $= 0$):

$$\therefore dF(t, X_t) = \sigma(X_t, t)\frac{\partial F}{\partial x}\,dW_t.$$

Integrating both sides from $t \to T$, we have:

$$\int_t^T dF(u, X_u) = \int_t^T \sigma(X_u, u)\frac{\partial F}{\partial x}\,dW_u$$

$$\mathbb{E}\left[F(T, X_T) - F(t, X_t)\right] = 0$$

**[gap:** the step between these two lines — taking $\mathbb{E}_{t,x}$ of both sides and using that an Itô integral has expectation zero — isn't written; the second line appears directly**]**

$$\Rightarrow \mathbb{E}\left[F(T, X_T)\right] - \mathbb{E}\left[F(t, X_t)\right] = 0.$$

But $F(t, X_t)$ is not a R.V. (it's a value at a specific time $t$ & its expectation is itself).

$$\mathbb{E}\left[F(T, X_T)\right] = F(t, X_t)$$

---

## IMG_3645

but $F(T, X_T) = \Phi(X_T)$, the terminal condition of the PDE.

$$\therefore F(t, X_t) = \mathbb{E}\left[\Phi(X_T)\right] \;\to\; \text{Feynman-Kac Stochastic Representation Formula.}$$

**Applications**
**Derivation of the BSM formula.**

European call option price $V(S, t)$ with strike price $K$ & maturity $T$ satisfies the BSM PDE:

$$\frac{\partial V}{\partial t} + rS\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} - rV = 0$$

**[sic:** $rS$ is written $rV$ in the second term**]**

With the terminal condition:

$$V(S_T, T) = (S_T - K)^+$$

[According] to Feynman-Kac theorem the solution $V(S, t)$ to a PDE of the form

$$\frac{\partial V}{\partial t} + \mu(x,t)\frac{\partial V}{\partial x} + \frac12\sigma^2(x,t)\frac{\partial^2V}{\partial x^2} - r(x,t)V(x,t) = 0$$

**[sic:** the second-derivative denominator is written $\partial x$ — should be $\partial x^2$**]**

is given by the conditional Expectation:

$$V(S, t) = \mathbb{E}_{\mathbb{Q}}\left[e^{-\int_t^T r(S_u, u)\,du}\cdot V(S_T, T)\,\middle|\,\mathcal{F}_t\right]$$

but the risk-free rate $r$ is assumed to be constant until maturity. Also let $\tau = T - t$. The discount factor simplifies to $e^{-r\tau}$, giving:

$$V(S, t) = e^{-r\tau}\cdot\mathbb{E}_{\mathbb{Q}}\left((S_T - K)^+\,\middle|\,\mathcal{F}_t\right)$$

Because the payoff $(S_T - K)^+$ is non-zero only when $S_T > K$, the expectation can be written as an integral over the prob. density function $dF(S_T)$ under the risk neutral

---

## IMG_3646

measure $\mathbb{Q}$:

$$V(S, K, t) = e^{-r\tau}\int_K^\infty (S_T - K)\,dF(S_T)$$

$$= e^{-r\tau}\int_K^\infty S_T\,dF(S_T) - Ke^{-r\tau}\int_K^\infty dF(S_T)$$

Recall under the risk neutral measure $\mathbb{Q}$ the stock price follows a log-normal distribution:

$$\ln(S_T) \sim N\!\left(\ln S_0 + (r - \tfrac12\sigma^2)\tau,\; \sigma^2\tau\right)$$

- mean: $M = \ln S_0 + (r - \frac12\sigma^2)\tau$
- variance: $\hat\sigma^2 = \sigma^2\tau \Rightarrow \hat\sigma = \sigma\sqrt\tau$

**[note:** on the page the mean is written with the letter $\mu$ and the standard deviation $\sigma\sqrt\tau$ is also just called "$\sigma$". I've written them $M$ and $\hat\sigma$ here so they don't collide with the stock drift $\mu$ and volatility $\sigma$ — same content**]**

Using the conditional expected value property $L_{S_T}(K)$ for lognormal distributions:

$$\int_K^\infty S_T\,dF(S_T) = \mathbb{E}_{\mathbb{Q}}\left[S_T \,\middle|\, S_T > K\right]$$

**[sic:** this should be the *partial* expectation $\mathbb{E}_{\mathbb{Q}}[S_T\,\mathbf 1_{\{S_T > K\}}]$, not the conditional one — the conditional expectation would be this divided by $\mathbb{Q}(S_T > K)$. The formula used on the next line is the partial-expectation one, so the result is unaffected**]**

$$= e^{M + \frac12\hat\sigma^2}\,\Phi\!\left(\frac{-\ln K + M + \hat\sigma^2}{\hat\sigma}\right)$$

$$= S_0 e^{-r\tau}\,\Phi\!\left(\frac{-\ln K + \ln S_0 + (r - \frac12\sigma^2)\tau + \sigma^2\tau}{\sigma\sqrt\tau}\right)$$

$$= S_0 e^{-r\tau}\,\Phi(d_1)$$

$$\Rightarrow e^{-r\tau}\int_K^\infty S_T\,dF(S_T) = e^{-r\tau}\cdot S_0e^{-r\tau}\,\Phi(d_1) = S_0\,\Phi(d_1)$$

**[sic:** sign slip — $e^{M + \frac12\hat\sigma^2} = e^{\ln S_0 + r\tau} = S_0e^{+r\tau}$, not $S_0e^{-r\tau}$. With $+r\tau$ the last line works: $e^{-r\tau}\cdot S_0e^{r\tau}\Phi(d_1) = S_0\Phi(d_1)$. As written, $e^{-r\tau}\cdot S_0e^{-r\tau}$ would give $S_0e^{-2r\tau}$**]**

The integral $\int_K^\infty dF(S_T)$ evaluates the prob. $1 - F_{S_T}(K)$:

$$\int_K^\infty dF(S_T) = 1 - \Phi\!\left(\frac{\ln K - M}{\hat\sigma}\right)$$

---

## IMG_3647

$$= 1 - \Phi\!\left(\frac{\ln K - \{\ln S_0 + (r - \frac12\sigma^2)\tau\}}{\sigma\sqrt\tau}\right)$$

$$= 1 - \Phi(-d_2)$$

$$= \Phi(d_2)$$

(There's a red diagonal mark through this part of the page — it looks like a tick rather than a crossing-out, and the algebra is correct: $1 - \Phi(-d_2) = \Phi(d_2)$ by symmetry of the normal.)

$$\Rightarrow e^{-r\tau}K\int_K^\infty dF(S_T) = e^{-r\tau}K\,\Phi(d_2)$$

Combining the 2 terms we have:

$$V(S, K, \tau) = C(S, K, \tau) = S_0\,\Phi(d_1) - Ke^{-r\tau}\,\Phi(d_2)$$

**2. Solve**

$$\frac{\partial F}{\partial t} + \frac{\partial F}{\partial x} + \frac{\partial^2F}{\partial x^2} - x^2 + 1 = 0.$$
$$F(T, x) = 2x$$

**Soln.** State the general Feynman-Kac PDE for fxn $F(t, x)$:

$$\frac{\partial F}{\partial t} + \mu(x,t)\frac{\partial F}{\partial x} + \frac12\sigma^2(x,t)\frac{\partial^2F}{\partial x^2} - r(x,t)F + \underbrace{g(x,t)}_{\text{source term}} = 0$$

Comparing the terms:

- Drift coefficient $(\mu)$: $\dfrac{\partial F}{\partial x} \Rightarrow \mu(x,t)\dfrac{\partial F}{\partial x}$, $\;\therefore \mu(x,t) = 1$
- Diffusion coefficient $(\sigma)$: $\dfrac{\partial^2F}{\partial x^2} \Rightarrow \frac12\sigma^2(x,t)\dfrac{\partial^2F}{\partial x^2}$, $\;1 = \frac12\sigma^2 \Rightarrow \sigma = \sqrt2$, $\;\therefore \sigma^2 = \sqrt2$

**[sic:** last line — $\sigma = \sqrt2$, so $\sigma^2 = 2$. The $\sqrt2$ used below is correct**]**

---

## IMG_3648

- Discount rate (in coefficient): $0 \Rightarrow r(x,t) = 0$
- Source term $(g)$: $g(x,t) = -x^2 + 1 = 1 - x^2$
- Terminal Condition: $\Psi(x) / \Phi(x) = 2x$

**Construct & Solve the Associated SDE** — underlying SDE for $X_s$:

$$dX_s = \mu(X_s, s)\,ds + \sigma(X_s, s)\,dW_s$$
$$dX_s = 1\cdot ds + \sqrt2\,dW_s$$

Integrate from $t$ to $T$:

$$\int_t^T dX_s = \int_t^T 1\,ds + \int_t^T \sqrt2\,dW_s$$

$$X_T - X_t = (T - t) + \sqrt2(W_T - W_t)$$

Feynman-Kac $\Rightarrow F(t, X_t) = \mathbb{E}[\Phi(X_T)]$ — [arrow to the full formula below]

Hence,

$$X_T = X_t + (T - t) + \sqrt2(W_T - W_t)$$
$$X_t = x$$
$$X_T = x + (T - t) + \sqrt2(W_T - W_t)$$

**\* State the Feynman Kac Expectation Formula:**

$$F(t, x) = \mathbb{E}\left[\int_t^T g(X_\tau, \tau)\,d\tau + \Phi(X_T)\,\middle|\,X_t = x\right]$$

**[note:** on the page the integration variable is written as a capital $T$ (e.g. $g(X_T, T)\,dT$), the same letter as the maturity. I've used $\tau$ for the dummy variable here so the two don't collide — same content**]**

$$\therefore g(X_\tau, \tau) = 1 - X_\tau^2 \qquad \Phi(X_T) = 2X_T$$

$$F(x, t) = \mathbb{E}\left[\int_t^T (1 - X_\tau^2)\,d\tau + 2X_T\,\middle|\,X_t = x\right]$$

by linearity:

$$F(x, t) = \underbrace{\int_t^T \mathbb{E}(1 - X_\tau^2)\,d\tau}_{B} + \underbrace{2\,\mathbb{E}[X_T]}_{A}$$

---

## IMG_3649

**PART A:** $2\,\mathbb{E}[X_T]$

$$X_T = x + (T - t) + \sqrt2(W_T - W_t).$$

$$\text{mean}[X_T] = x + (T - t) + 0 = x + (T - t)$$

$$\operatorname{Var}(X_T) = 2(T - t)$$

$$\Rightarrow 2\,\mathbb{E}(X_T) = 2\{x + (T - t)\} = \underline{\underline{2x + 2(T - t)}}$$

**PART B:** $\displaystyle\int_t^T \mathbb{E}[1 - X_\tau^2]\,d\tau$

$$= \int_t^T \left(\mathbb{E}[1] - \mathbb{E}[X_\tau^2]\right)d\tau$$

Recall: $\operatorname{Var}(X_T) = \mathbb{E}(X_T^2) - [\mathbb{E}(X_T)]^2$

$$\mathbb{E}[X_T^2] = \operatorname{Var}(X_T) + (\mathbb{E}(X_T))^2$$
$$\Rightarrow 2(T - t) + (x + (T - t))^2$$
$$\Rightarrow 2(T - t) + x^2 + 2x(T - t) + (T - t)^2$$

(Margin: $T - t = u$, $du = \ldots$)

$$\mathbb{E}(1 - X_T^2) = 1 - \left(x^2 + 2(T - t) + 2x(T - t) + (T - t)^2\right)$$
$$= 1 - x^2 - 2(T - t) - 2x(T - t) - (T - t)^2$$

let $T - t = u$

$$\Rightarrow 1 - x^2 - 2u - 2xu - u^2$$

$$\therefore \int_t^T \mathbb{E}(1 - X^2)\,d\tau = \int_t^T (1 - x^2 - 2u - 2xu - u^2)\,du$$

$$= \int_t^T (1 - x^2)\,du - 2\int_t^T (1 + x)u\,du - \int_t^T u^2\,du.$$

$$= (1 - x^2)(T - t) - (1 + x)(T - t)^2 - \tfrac13(T - t)^3$$

**[note:** after substituting $u$ = (time elapsed since $t$), the limits should become $u: 0 \to T - t$, not $t \to T$. The final line is what you get with the correct limits, so the answer stands — only the limits on the middle lines are off**]**

---

## IMG_3650

$$\therefore F(t, x) = 2x + 2(T - t) + (1 - x^2)(T - t) - (1 + x)(T - t)^2 - \tfrac13(T - t)^3$$

(double-underlined)

$$= 2x - (T - t)x^2$$

**[sic:** this last simplification is wrong — the terms don't cancel down to that. Checked by substituting back: the double-underlined line above satisfies the PDE and $F(T, x) = 2x$ exactly; $2x - (T - t)x^2$ doesn't satisfy the PDE. Keep the double-underlined line as the answer**]**

**Solve:**

$$\frac{\partial F}{\partial t} + \frac14 x\frac{\partial F}{\partial x} + \frac12 x^2\frac{\partial^2F}{\partial x^2} + F = 0$$
$$F(T, x) = x^4$$

*(Not attempted in the notebook — the rest of the page and the facing page are blank.)*

---

*End of notebook photos (IMG_3619 → IMG_3650).*
