# What Makes Data a Panel?

## 1. The defining test

The idea is to observe the **same cross-sectional units repeatedly**:

- A **unit** may be a firm, bank, stock, household, country or loan.
- **Time** may be measured in days, months, quarters, years — whatever the repeated observation interval is.
- The defining feature is **not** simply that the dataset contains a date column. The same unit must be **linked across dates**. A dataset with a time variable but a *different* set of units each period is not a panel.

> [!tip] The one question that settles it
> Does the same unit reappear? If yes — panel. If no — it's something else, however much it superficially resembles one (see the comparison table below).

## 2. The four data structures, compared

| Structure | Units | Time | Same unit repeated? | Example |
|---|---|---|---|---|
| **Cross-section** | many | one point in time | — (only one observation per unit anyway) | All Grunfeld firms in 1954 only |
| **Time series** | one | many points in time | trivially, yes (it's the same one unit throughout) | Company 1's investment, 1935–1954 |
| **Pooled cross-section** | many, but a *different* sample each period | many points in time | **No** — units are not linked across periods | Independent household surveys taken in different years, with new households sampled each wave |
| **Panel (longitudinal) data** | many | many points in time | **Yes** — the same units are tracked over time | Grunfeld firms observed every year, 1935–1954 |

The distinction between pooled cross-section and true panel data is easy to miss because both stack many units across many time periods into one dataset that *looks* identical in shape. The difference is entirely about whether row $i$ in period $t$ and row $i$ in period $t+1$ refer to the same real-world entity.

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

## 4. Why bother with panel data? Advantages

**More observations is not the main value.** $N\times T$ observations is a bigger sample than $N$ alone, but the *real* payoff of panel data is structural, not just statistical power:

1. **Repeated observations let you compare each unit with itself.** Because the same firm, country or household is tracked over time, you can ask "how did *this* unit's outcome change when *its own* $X$ changed?" — a comparison that is simply unavailable in a single cross-section, where all you can ever do is compare *different* units to each other.
2. **This unlocks a way to handle persistent unobserved heterogeneity.** If a unit has some fixed, unmeasured trait $\alpha_i$ (managerial quality, institutional quality, innate ability) that never changes over the sample period, comparing the unit with itself across time automatically differences $\alpha_i$ away — you never need to measure it or assume it's uncorrelated with $X$. This is the entire motivation behind fixed effects and first differences (previewed in [[01 Estimation Foundations#4. Omitted-variable bias|ch.1 §4]], developed properly once this unit reaches those estimators).
3. It lets you separately study **between-unit** and **within-unit** relationships, which can differ — sometimes even in sign (see [[03 Sources of Variation - Between, Within and Overall]]).
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
