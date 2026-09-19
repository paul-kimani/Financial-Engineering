# Sources of Variation: Between, Within and Overall

## 1. Why panel data has three kinds of variation

A cross-section has exactly one source of variation: how units differ from each other. A time series (for one unit) has exactly one source too: how that unit moves over time. A panel has **both at once**, plus their combination — and separating them is the key conceptual move behind everything from [[04 Pooled OLS and the Unobserved-Effects Model|pooled OLS's limitations]] to fixed effects later in the unit.

## 2. The decomposition identity

For any variable $X_{it}$, define:

- the **grand mean** $\bar X = \dfrac{1}{NT}\sum_i\sum_t X_{it}$ (pooling every unit and period together),
- the **unit mean** $\bar X_i = \dfrac{1}{T}\sum_t X_{it}$ (unit $i$'s own average across its observed periods).

Any single deviation from the grand mean splits exactly into two pieces:

$$X_{it} - \bar X = \underbrace{(\bar X_i - \bar X)}_{\text{between-unit component}} + \underbrace{(X_{it} - \bar X_i)}_{\text{within-unit component}}$$

> [!example] Reading the identity
> The first term, $\bar X_i-\bar X$, doesn't depend on $t$ at all — it only asks "how does this unit's *average* compare to the average unit?" The second term, $X_{it}-\bar X_i$, doesn't depend on the grand mean at all — it only asks "how does this particular observation compare to *this unit's own* average?" Add them back together and you recover exactly how far $X_{it}$ sits from the grand mean. Nothing is lost or double-counted; it's an algebraic identity, true for every single observation.

## 3. Between, within and overall variation, defined

| Variation | What varies | Formula (sum of squares, informally) | What it answers |
|---|---|---|---|
| **Between** | across units, using each unit's *own average* only — time dimension collapsed away | $\sum_i(\bar X_i-\bar X)^2$ (one term per unit) | Do units with persistently higher $X$ also have persistently higher $Y$? |
| **Within** | across time, for a *given* unit, around that unit's own mean | $\sum_i\sum_t(X_{it}-\bar X_i)^2$ | When *this* unit's $X$ rises above its own typical level, does its $Y$ rise too? |
| **Overall** | everything at once — every observation compared to the single grand mean | $\sum_i\sum_t(X_{it}-\bar X)^2$ | The plain, undifferentiated total variation a naive pooled analysis would use |

> [!warning] A common misreading of `xtsum`-style output
> It is tempting to expect the between and within standard deviations to add up to the overall standard deviation. They generally **do not** — the decomposition above is additive in *sums of squared deviations* under specific conditions, not simply additive in standard deviations. Reason from the decomposition identity itself, not from an expectation that the reported numbers should sum neatly.

**The exact result, derived.** Write $a_i=\bar X_i-\bar X$ and $b_{it}=X_{it}-\bar X_i$, so $X_{it}-\bar X=a_i+b_{it}$. Squaring and summing over every $(i,t)$:

$$\text{SS}_{\text{overall}} = \sum_i\sum_t(a_i+b_{it})^2 = \sum_i\sum_t a_i^2 + 2\sum_i\sum_t a_ib_{it} + \sum_i\sum_t b_{it}^2$$

Since $a_i$ doesn't depend on $t$, $\sum_i\sum_t a_i^2 = T\sum_i a_i^2 = T\cdot\text{SS}_{\text{between}}$, and $\sum_i\sum_t b_{it}^2=\text{SS}_{\text{within}}$ by definition. For the cross term, factor $a_i$ out of the inner sum:

$$2\sum_i\sum_t a_ib_{it} = 2\sum_i a_i\left(\sum_t(X_{it}-\bar X_i)\right)$$

and $\sum_t(X_{it}-\bar X_i)=0$ for every $i$ — pure algebra from the definition of an average: $\sum_t(X_{it}-\bar X_i)=\sum_t X_{it}-T\bar X_i=\sum_t X_{it}-\sum_t X_{it}=0$ (no optimization involved, though it's the same fact as $\sum_i\hat u_i=0$ from [[01 Estimation Foundations#Deriving the normal equations|ch.1]], since a plain average *is* the least-squares solution to an intercept-only regression, and this is that regression's own first normal equation). So the cross term vanishes exactly, giving:

$$\text{SS}_{\text{overall}} = T\cdot\text{SS}_{\text{between}} + \text{SS}_{\text{within}}$$

an **exact** identity for a balanced panel — sums of squares genuinely add (with the $T$ weight on the between piece). What breaks down is two further steps stacked on top: taking square roots of a sum doesn't distribute ($\sqrt{x+y}\neq\sqrt x+\sqrt y$), and software like `xtsum` normalizes the three reported standard deviations by different degrees of freedom (overall by $NT$, between by $N$, within by an adjusted $NT$) — so even before the square-root issue, the terms aren't on equal footing.


## 4. Worked toy example: two firms, two years

Take two firms and a leverage ratio $X$ observed in two years:

| Firm | Year 1 | Year 2 | Firm mean $\bar X_i$ |
|---|---|---|---|
| A | 0.3 | 0.5 | 0.4 |
| B | 0.6 | 0.7 | 0.65 |

Grand mean: $\bar X = (0.3+0.5+0.6+0.7)/4 = 0.525$.

**Between component** (how each firm's own average compares to the grand mean — the same number is used for both of that firm's observations):
$$\bar X_A - \bar X = 0.4 - 0.525 = -0.125, \qquad \bar X_B - \bar X = 0.65-0.525 = 0.125$$

**Within component** (how each observation compares to its *own* firm's average):
$$X_{A,1}-\bar X_A = 0.3-0.4=-0.1, \quad X_{A,2}-\bar X_A = 0.5-0.4=0.1$$
$$X_{B,1}-\bar X_B = 0.6-0.65=-0.05, \quad X_{B,2}-\bar X_B = 0.7-0.65=0.05$$

**Verifying the identity** for, say, Firm A, Year 2: $X_{A,2}-\bar X = 0.5-0.525=-0.025$, and indeed $(\bar X_A-\bar X)+(X_{A,2}-\bar X_A) = -0.125+0.1=-0.025$. ✓.

Notice the within deviations sum to (approximately) zero **within every firm** — $-0.1+0.1=0$ for A, $-0.05+0.05=0$ for B — a mechanical consequence of deviating each observation from its *own* unit mean, exactly parallel to $\sum_i\hat\epsilon_i=0$ for OLS residuals ([[01 Estimation Foundations#3. What OLS actually does vs. what identification requires|ch.1 §3]]).

## 5. Why between and within relationships can differ — even in sign

Because between variation compares *different* units to each other, while within variation compares a unit *to itself over time*, there is no requirement that the two relationships point the same way. This is not a technical curiosity — it is one of the central reasons naive pooling is dangerous, developed fully in [[04 Pooled OLS and the Unobserved-Effects Model]].

> [!example] A finance story where the signs flip
> Suppose more conservative, cash-rich firms persistently hold *both* higher market value **and** lower investment (a firm-level trait — call it conservatism — drives both). Across firms, you'd see a *negative* between relationship between value and investment. But within a given firm over time, a genuine *positive* year-to-year story could hold — when that firm's market value rises (say, due to a temporarily favorable outlook), it may invest more that year. Pooling both sources of variation together, as plain pooled OLS does, blends a negative between-relationship with a positive within-relationship into one number that may not resemble either one — and can even come out with the "wrong" sign relative to the relationship you actually care about.

**A verified example: police spending and crime.** Take police spending and crime rates observed across many cities over many years.

- **Between:** across cities, more police spending tends to go with *higher* crime. Not because police cause crime — a city's underlying, largely fixed crime propensity $\alpha_i$ (poverty, density, social conditions) drives both: it produces more crime directly, and it causes city governments to respond by funding more police. That fixed trait pulls both variables the same way across cities, so the raw cross-city correlation comes out **positive**.
- **Within:** now hold a single city fixed and watch it over time. In years where *that same city*, relative to its own usual baseline, spent more on policing, crime tends to move **down** — a genuine deterrent effect, isolated once $\alpha_i$ is differenced away.

The mechanism is exactly the ch.2 Firm A story: $\alpha_i$ is a fixed unit-level trait, it's positively correlated with the regressor across units (contaminating the between comparison), and differencing cancels it exactly — $\alpha_i-\alpha_i=0$ — leaving the within relationship free of that particular confound.

> [!warning] Not every correlated pair delivers a sign flip
> A genuine sign flip needs a *specific* structural setup — a fixed unit trait correlated with $X$ across units, but not what's driving $Y$'s response to $X$ within a unit. It's easy to reach for a pair of variables that *sounds* like it should flip and find both relationships actually point the same way. For instance, firm profitability and leverage: across firms, more profitable firms tend to carry less debt (the "pecking order" pattern — profitable firms prefer internal cash over borrowing). Within a firm, a year of above-typical profitability generates more internal cash, which *also* reduces the need to borrow that year. Both relationships are negative — no flip. The lesson: don't assume divergence by default; trace the actual mechanism through both comparisons before claiming the signs differ.

This is exactly why a pooled-OLS coefficient can be difficult to interpret when the two underlying relationships have very different magnitudes or opposite signs: it is a variance-weighted blend of both, not a clean estimate of either.


## 6. Cheat sheet

| Quantity | Compares | Time dimension | Typical concern it isolates |
|---|---|---|---|
| Between | unit averages to the grand mean | collapsed away | persistent cross-unit differences |
| Within | each observation to its own unit's average | preserved | unit-specific change over time |
| Overall | every observation to the grand mean | mixes both | what a naive pooled analysis sees |

Decomposition identity: $X_{it}-\bar X = (\bar X_i-\bar X) + (X_{it}-\bar X_i)$.

## 7. Open questions for revision

- [ ] Construct your own finance or economics example where the between relationship between $X$ and $Y$ is negative but the within relationship is positive. Explain both mechanisms in words.
- [ ] Explain "between variation" and "within variation" to someone without using the word "variance" at all.
- [ ] Why would you *not* expect the reported between standard deviation plus the within standard deviation to equal the overall standard deviation? Reason from the decomposition of deviations, not from adding standard deviations directly.
- [ ] Using Grunfeld-style data, calculate a firm's mean market value and its within-firm deviations by hand for two or three years, and verify the deviations sum to (approximately) zero for that firm.

---
*Source: BSE 3211/4211 Week 2 "Panel Data Foundations — Pooled Models" lecture (PDF, pp.21–40) and "Panel data FENG 3y" (pptx).*
