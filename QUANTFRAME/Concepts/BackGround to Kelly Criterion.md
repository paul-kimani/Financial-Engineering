This page is building the **engine behind the Kelly Criterion**… but instead of jumping straight to the answer, it carefully constructs _why_ that answer is optimal. Let’s unpack it like a story of wealth evolving over time.

## 1. What is being measured?

You repeatedly bet a fraction $( f )$ of your wealth.
- With probability $( p )$: you **win**, wealth multiplies by ( $(1+f)$ )
- With probability $( q = 1-p )$: you **lose**, wealth multiplies by ( $(1-f)$ )

After many rounds ( n ), your wealth becomes:

$$
X_n = X_0 (1+f)^S (1-f)^F  
$$

where:
- $( S )$ = number of wins
- $( F )$ = number of losses

## 2. Why logs appear (this is key)

Instead of tracking raw wealth (which explodes or collapses), we look at **growth rate**:
$$
\frac{1}{n} \log\left(\frac{X_n}{X_0}\right)  
$$

Think of this as:

> “average growth per bet” 📈

Using log rules, this becomes:

$$
\frac{S}{n} \log(1+f) + \frac{F}{n} \log(1-f)  
$$

Now here’s the clever move:

- As $( n )$ gets large,  
$$( \frac{S}{n} \to p ), ( \frac{F}{n} \to q )$$

So the **expected growth rate** becomes:

## 3. The function we want to maximize

$$G(f)=p\log(1+f)+q\log(1-f)$$

This is the heart of everything.

Interpretation:
- First term = growth from wins
- Second term = damage from losses
- Together = _long-term survival + growth balance_

---

## 4. Why maximize $( G(f) )$?

Because:  
$$
G(f) = \frac{1}{n} E[\log X_n]  
$$

So maximizing $( G(f) )$ is the same as:

> maximizing expected logarithmic wealth

This is **not** about getting rich fast.  
It’s about **not going broke while growing steadily**.

## 5. Finding the optimal bet size

Differentiate:
$$
G'(f) = \frac{p}{1+f} - \frac{q}{1-f}  
$$

Set to zero:
$$
\frac{p}{1+f} = \frac{q}{1-f}  
$$

Solve → magic result:
$$
f^* = p - q  
$$

## 6. What does $( f^* = p - q )$ mean?

Since $( q = 1-p )$, this becomes: 
$$
f^* = 2p - 1  
$$

So:

| Probability ( p ) | Optimal fraction ( f^* ) |
| ----------------- | ------------------------ |
| 0.5               | 0 (don’t bet)            |
| 0.6               | 0.2                      |
| 0.7               | 0.4                      |
| 0.9               | 0.8                      |

 You bet **more only when your edge is stronger**

---

## 7. Why this is a maximum (not minimum)

They compute:
$$
G''(f) < 0  
$$

Meaning:

> The curve bends downward like an upside-down bowl 

So the solution is a **true peak**, not a trap.

## 8. Important intuition (this part changes how you think)
- If you bet too small → slow growth
- If you bet too big → risk collapse
- Kelly finds the **knife-edge balance**

Also:

- $( G(0) = 0 )$ → no betting = no growth
- $( G(f) \to -\infty )$ as $( f \to 1 )$ → overbetting destroys you.

## 9. The hidden philosophy

This isn’t just math. It’s a survival law:

> Maximize growth **without risking extinction**

Or in trader language:

- Not “How much can I win?”
- But “How do I stay alive long enough to win repeatedly?”


Now we upgrade Kelly from a coin-flip toy into a proper trading beast with **asymmetric payoffs** 
## 1. The setup (now with odds $( b )$)
Each round:
- You bet fraction $( f )$ of wealth
- With probability $( p )$: you **win** → gain $( b \times f )$
- With probability ( q = 1 - p ): you **lose** → lose $( f )$

So wealth evolves like:
- Win → multiply by $( (1 + bf) )$
- Loss → multiply by $( (1 - f) )$

## 2. Build the growth function

Same philosophy: maximize **expected log growth**

$$G(f)=p\log(1+bf)+q\log(1-f)$$

This is your long-term growth engine.

---

## 3. Take the derivative

We hunt for the peak:
$$
G'(f) = \frac{pb}{1+bf} - \frac{q}{1-f}  
$$

Set it to zero:
$$
\frac{pb}{1+bf} = \frac{q}{1-f}  
$$

## 4. Solve it step-by-step

Cross-multiply:
$$
pb(1 - f) = q(1 + bf)  
$$

Expand both sides:
$$
pb - pbf = q + qbf  
$$

Group terms:
$$
pb - q = bf(p + q)  
$$

Now here’s the neat simplification:
- $( p + q = 1 )$
So:
$$
pb - q = bf  
$$

Solve for $( f )$:

---

## 5. The general Kelly formula

$$f^* = \frac{bp - q}{b}$$

## 6. Make it more intuitive

Since $( q = 1 - p )$, rewrite:
$$
f^* = \frac{bp - (1 - p)}{b}  
= \frac{bp + p - 1}{b}  
= \frac{p(b+1) - 1}{b}  
$$

This version is often easier to interpret.

---

## 7. What this formula is really saying

Think of it like a negotiation between:
- **Edge** → how likely you are to win (( p ))
- **Reward** → how much you win (( b ))
- **Risk** → full loss when wrong
