# What Makes Data a Panel?

## 1. The defining test

The idea is to observe the **same cross-sectional units repeatedly**:

- A **unit** may be a firm, bank, stock, household, country or loan.
- **Time** may be measured in days, months, quarters, years — whatever the repeated observation interval is.
- The defining feature is **not** simply that the dataset contains a date column. The same unit must be **linked across dates**. A dataset with a time variable but a *different* set of units each period is not a panel.

> [!tip] The one question that settles it
> Does the same unit reappear? If yes — panel. If no — it's something else, however much it superficially resembles one (see the comparison table below).

**Grounding "unit" and "time" concretely.** Take a table with three columns: City, Day, Temperature.

- **Unit** is whichever column identifies the thing being followed — here, City (Nairobi, Lagos, Accra, ...).
- **Time** is whichever column identifies *when* each reading was taken — here, Day (Mon, Tue, Wed, ...).

A single row is one *(unit, time)* pair: (Nairobi, Mon) → 24°C. Having panel data means you have (Nairobi, Mon), (Nairobi, Tue), (Nairobi, Wed) — the *same* city followed across days — not just three different cities each measured once. The two dimensions are never optional extras to identify; every panel observation is located by exactly these two coordinates, unit and time, together.

## 2. The four data structures, compared


| Structure | Units | Time | Same unit repeated? | Example |
|---|---|---|---|---|
| **Cross-section** | many | one point in time | — (only one observation per unit anyway) | All Grunfeld firms in 1954 only |
| **Time series** | one | many points in time | trivially, yes (it's the same one unit throughout) | Company 1's investment, 1935–1954 |
| **Pooled cross-section** | many, but a *different* sample each period | many points in time | **No** — units are not linked across periods | Independent household surveys taken in different years, with new households sampled each wave |
| **Panel (longitudinal) data** | many | many points in time | **Yes** — the same units are tracked over time | Grunfeld firms observed every year, 1935–1954 |

The distinction between pooled cross-section and true panel data is easy to miss because both stack many units across many time periods into one dataset that *looks* identical in shape. The difference is entirely about whether row $i$ in period $t$ and row $i$ in period $t+1$ refer to the same real-world entity.

**Concrete example: a household survey.** Suppose a government agency surveys 500 households every year on income and spending.

- If each year's 500 households are freshly and independently sampled — a different 500 families in 2023, a different 500 in 2024 — the data is a **pooled cross-section**. Household #217 in the 2023 file and household #217 in the 2024 file are not the same family; the ID number is just a row label, not a link across time.
- If the agency deliberately re-contacts the *same* 500 households every year, tracking them as they age, move, or change jobs, the data is a **true panel**. Household #217 in 2023 and #217 in 2024 are the same family both times.

> [!warning] Both look identical from a single wave alone
> Take just the 2023 wave on its own — 500 households, one row each. Nothing in that single cross-section reveals whether next year's wave will recontact these same 500 households or draw a fresh sample. The panel-vs-pooled distinction is not a property of any one period's data; it's a property of *how the sample was constructed across periods*. You need to know the sampling design (or observe at least two linked periods) to tell them apart — a single snapshot can never settle it.

**Working definitions (for note-taking):**
- **Pooled cross-section**: repeated cross-sectional samples over multiple periods, where each period's sample consists of different units drawn independently — units are not linked across time, even though the dataset stacks multiple periods together.
- **True panel (longitudinal) data**: repeated observations on the *same* set of cross-sectional units across multiple periods, where each unit can be tracked and compared with itself over time.

## 3. Panel notation


For unit $i=1,\dots,N$ observed at time $t=1,\dots,T$ (or $T_i$, if the panel is unbalanced — see §5), a typical panel regression is written:

$$Y_{it} = \beta_0 + X_{it}'\beta + v_{it}$$

| Symbol | Meaning |
|---|---|
| $i$ | cross-sectional unit index (firm, country, household, ...) |
| $t$ | time index |
| $N$ | number of cross-sectional units |
| $T$ | number of time periods (per unit, if balanced) |
| $Y_{it}$ | outcome for unit $i$ at time $t$ |
| $X_{it}$ | vector of regressors for unit $i$ at time $t$ |
| $v_{it}$ | the panel error term — decomposed further in [[04 Pooled OLS and the Unobserved-Effects Model]] |

**Concrete example: the Grunfeld data.** The classic Grunfeld investment dataset has $N=10$ firms observed over $T=20$ years (1935–1954), the same 10 firms every year. If the panel is balanced, the total row count is $N\times T = 10\times20=200$. Each row is indexed by a specific $(i,t)$ pair — e.g. $i=3,\,t=1940$ is firm 3's observation for the year 1940, distinct from $i=3,\,t=1941$ (same firm, next year) and from $i=4,\,t=1940$ (different firm, same year).

## 4. Why bother with panel data? Advantages


**More observations is not the main value.** $N\times T$ observations is a bigger sample than $N$ alone, but the *real* payoff of panel data is structural, not just statistical power:

1. **Repeated observations let you compare each unit with itself.** Because the same firm, country or household is tracked over time, you can ask "how did *this* unit's outcome change when *its own* $X$ changed?" — a comparison that is simply unavailable in a single cross-section, where all you can ever do is compare *different* units to each other.
2. **This unlocks a way to handle persistent unobserved heterogeneity.** If a unit has some fixed, unmeasured trait $\alpha_i$ (managerial quality, institutional quality, innate ability) that never changes over the sample period, comparing the unit with itself across time automatically differences $\alpha_i$ away — you never need to measure it or assume it's uncorrelated with $X$. This is the entire motivation behind fixed effects and first differences (previewed in [[01 Estimation Foundations#4. Omitted-variable bias|ch.1 §4]], developed properly once this unit reaches those estimators).

   **Worked example: why differencing eliminates $\alpha_i$ exactly.** Suppose Firm A's investment depends on its firm value and its (unmeasured, time-invariant) managerial quality $\alpha_A$:

   $$Y_{i,t} = \beta_0 + \beta_2 X_{i,t} + \alpha_i + u_{i,t}$$

   Write this out for Firm A in both years:

   $$Y_{A,2020} = \beta_0 + \beta_2 X_{A,2020} + \alpha_A + u_{A,2020}$$
   $$Y_{A,2021} = \beta_0 + \beta_2 X_{A,2021} + \alpha_A + u_{A,2021}$$

   Subtract the first equation from the second:

   $$Y_{A,2021} - Y_{A,2020} = \beta_2(X_{A,2021}-X_{A,2020}) + (\alpha_A - \alpha_A) + (u_{A,2021}-u_{A,2020})$$

   $\beta_0$ cancels because it's a constant common to both years. $\alpha_A - \alpha_A = 0$ — not because of any assumption about $\alpha_A$'s value or its relationship to $X$, but because it is literally the same number subtracted from itself. Whatever Firm A's managerial quality is — good, bad, ever measured or not — it drops out **exactly**.

   > [!tip] Contrast with a single cross-section
   > If you only had 2021 data across *many different* firms, $\alpha_i$ would vary firm-to-firm, and if it's correlated with $X_i$ (better-managed firms also tending to have higher firm value), it would contaminate $\hat\beta_2$ through precisely the omitted-variable-bias mechanism from [[01 Estimation Foundations#4. Omitted-variable bias|ch.1 §4]] — with $\alpha_i$ playing the role of the omitted $Z_i$. Within-unit differencing sidesteps that contamination entirely, without ever having to observe or make assumptions about $\alpha_i$.

3. It lets you separately study
 **between-unit** and **within-unit** relationships, which can differ — sometimes even in sign (see [[03 Sources of Variation - Between, Within and Overall]]).
4. It can improve the precision of estimates of parameters that are otherwise hard to pin down from a single cross-section or a short time series alone.

## 5. Balanced vs. unbalanced panels

- A **balanced panel** has an observation for every unit at every time period — the same $T$ periods for every $i$, so $N\times T$ equals the total observation count exactly.
- An **unbalanced panel** has gaps: some units are missing some periods. **Unbalanced panels are very common in real-world data** — balanced panels are closer to the exception than the rule.

### Why panels become unbalanced

Gaps arise for reasons that matter econometrically, not just administratively:

- **Random missingness** — a survey respondent skips one wave by chance, a data field is unrecorded for an unrelated reason. This is (relatively) benign: it doesn't by itself bias estimation.
- **Firms exiting through bankruptcy, acquisition, or delisting.** This is **not** random — it is likely to be systematically related to the very outcomes under study.
- **New units entering** the sample over time (an IPO, a new country beginning to report the relevant statistic).

> [!warning] Survival / attrition bias
> When exit from the panel is correlated with the outcome variable — e.g. financially distressed firms are more likely to disappear from an investment dataset — restricting analysis to the units that *survived* the full sample period selects on the outcome itself. The surviving sample is no longer representative of the original population, and estimates from it can be badly misleading. This is why the distinction between random missingness and *selective* attrition matters: unbalancedness by itself does not make pooled estimation impossible, but attrition tied to the outcome is a genuine sample-selection problem, not a mechanical nuisance to be shrugged off.

**Two distinct problems from selective attrition — not one:**

1. **Mechanical loss of comparison.** Once a firm exits, it can never again be compared with itself — the within-unit trick from §4 requires the *same* unit observed at multiple points, and an exited firm simply stops supplying those points going forward. This piece would exist even if exit were pure random noise; you'd still lose that unit's future within-comparisons.
2. **Sample selection.** Beyond losing that one firm's future comparisons, the set of firms that *remain* in the panel is no longer representative of the original population. If the firms most likely to exit are exactly the poorly-performing or financially distressed ones, any estimate computed only from survivors is systematically distorted toward survivors' characteristics — not the population's. This piece only arises because exit is *correlated* with the outcome under study, unlike problem 1.

Unbalancedness by itself does not make pooled OLS or most panel estimators inapplicable — the mechanics generally still go through with an unbalanced panel. What changes is whether you can still trust the sample as representative, which depends on *why* it's unbalanced.


## 6. Cheat sheet

| Question | Answer |
|---|---|
| What makes data a panel (not just "has a date")? | The same cross-sectional unit is linked and re-observed across time periods |
| Panel vs. pooled cross-section | Panel: same units repeated. Pooled cross-section: different sample each period |
| Main value of panel data | Comparing each unit with itself over time — not just more observations |
| Balanced panel | Every unit observed in every period |
| Unbalanced panel | Gaps — very common in practice |
| Random missingness | Benign on its own |
| Selective attrition | A sample-selection problem — correlated with the outcome |

## 7. Open questions for revision

- [ ] For a panel of 100 banks observed annually for 10 years: define the unit, the time dimension, $N$, $T$, and the maximum possible number of observations. Give three distinct reasons the realized panel could still be unbalanced.
- [ ] Explain, without using the word "sample size," why repeated observations of the *same* unit are more valuable than an equivalent number of observations on *different* units.
- [ ] Why is "our panel is unbalanced because firms went bankrupt" a more serious concern for identification than "our panel is unbalanced because of a data-entry gap"? Connect this to sample selection.
- [ ] Verify, for a specific dataset, whether $N\times T$ equals the total number of observations — what does a mismatch tell you immediately?

---
*Source: BSE 3211/4211 Week 1 introduction (pptx) and Week 2 "Panel Data Foundations — Pooled Models" lecture (PDF, pp.1–20).*
