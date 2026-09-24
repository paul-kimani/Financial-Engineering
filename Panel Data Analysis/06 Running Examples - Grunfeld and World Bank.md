# Running Examples: Grunfeld and World Bank

Two datasets run throughout this unit, one in each software package taught alongside the theory. Keeping both side by side is deliberate — the concepts (between/within variation, pooled OLS, poolability, clustering) are identical regardless of tool; only the syntax changes.

## 1. Firm investment: the Grunfeld panel (Stata)

**Economic question:** how are firm investment, market value and the existing capital stock related?

$$\text{Investment}_{it} = \beta_0 + \beta_1\,\text{Value}_{it} + \beta_2\,\text{Capital}_{it} + v_{it}$$

The unit is the **firm**, the time index the **year**. $\beta_1$ and $\beta_2$, estimated pooled, use *all* firm-year comparisons at once — mixing between-firm and within-firm variation together (see [[03 Sources of Variation - Between, Within and Overall]]).

### Setup and description

```stata
* Load and inspect
use grunfeld, clear
describe
list in 1/12

* Declare the panel structure
xtset company year
xtdescribe
```

`xtdescribe` reports the panel's shape directly — number of units, the time range, and whether it is balanced — the same question raised conceptually in [[02 What Makes Data a Panel?#5. Balanced vs. unbalanced panels|ch.2 §5]].

### Between/within/overall variation

```stata
* Overall, between and within summary statistics
xtsum invest mvalue kstock

* Each firm's mean market value, and its within-firm deviation
bysort company: egen mean_mvalue = mean(mvalue)
gen dev_mvalue = mvalue - mean_mvalue
```

`xtsum`'s three-row output (overall / between / within) is the numerical counterpart of the decomposition identity in [[03 Sources of Variation - Between, Within and Overall#2. The decomposition identity|ch.3 §2]] — read it as answering "how much of the total spread in this variable is cross-firm, and how much is within-firm over time?", not as three numbers that should sum to anything in particular.

### Pooled regression, plain and clustered

```stata
* Pooled OLS, conventional standard errors
reg invest mvalue kstock

* Same coefficients, standard errors clustered by firm
reg invest mvalue kstock, vce(cluster company)
```

Only the standard errors change between the two — see [[05 Clustered and Robust Standard Errors|ch.5]] for exactly what clustering does and does not fix.

### Poolability (Chow/F test)

```stata
reg invest mvalue kstock i.company
testparm i.company
* H0: all company effects are jointly zero
* Reject H0: simple pooling is inadequate
```

## 2. Country investment: the World Bank panel (R)

**Economic question:** across a panel of (illustratively) African countries, how are credit depth, institutional measures, and capital formation related? The World Bank Development Indicators (WDI) data plays the same structural role here that Grunfeld's firms play above — countries instead of firms, years instead of... years.

### Setup and description

```r
library(plm)
library(WDI)

pg <- pdata.frame(wdi_panel, index = c("country", "year"))

pdim(pg)              # N, T, balanced/unbalanced
is.pbalanced(pg)
```

### Between/within/overall variation

```r
# Country means and within-country deviations
country_means <- ave(pg$credit_depth, pg$country)
within_dev    <- pg$credit_depth - country_means
```

### Pooled benchmark, with clustered standard errors

```r
m_pool <- plm(investment ~ credit_depth + institutions,
              data = pg, model = "pooling")

library(lmtest); library(sandwich)
coeftest(m_pool, vcov = vcovHC(m_pool, cluster = "group"))
```

### Poolability (Chow/F test) via plm

```r
m_fe_preview <- plm(investment ~ credit_depth + institutions,
                     data = pg, model = "within")
pFtest(m_fe_preview, m_pool)
# H0: common intercept / pooled OLS is adequate
# Reject H0: country-specific intercepts are jointly relevant
```

## 3. Reading a panel description, in either package

Five things to check whenever you first look at a new panel dataset, in either Stata or R:

1. **What is the cross-sectional unit, and what is the time variable?** (`xtset`'s two arguments; `pdata.frame`'s `index`.)
2. **$N$, $T$, and whether $N\times T$ equals the actual observation count** — a mismatch is your first, cheapest check for unbalancedness.
3. **Balanced or unbalanced** (`xtdescribe`; `pdim`/`is.pbalanced`) — and if unbalanced, *why*, per the random-missingness-vs-attrition distinction in [[02 What Makes Data a Panel?#5. Balanced vs. unbalanced panels|ch.2 §5]].
4. **Overall vs. between vs. within variation** for each key variable (`xtsum`; manual group means and deviations) — before running any regression at all, this tells you whether there's enough within-unit movement for a within estimator to even have something to work with later.
5. **Whether the pooled coefficients survive a poolability test** — and if they don't, that they still cannot tell you whether $\operatorname{Cov}(X,\alpha_i)=0$ (see [[04 Pooled OLS and the Unobserved-Effects Model#5. Is pooling adequate? The Chow/F poolability test|ch.4 §5]]).

## 4. The five-model progression this unit builds toward

Both running examples are designed to walk through the same five specifications, in the same order, as the unit progresses — this chapter and the ones before it cover the first three; the remaining two are previewed here and developed fully in later study sessions.

| # | Model | Comparison used | Covered in |
|--:|---|---|---|
| 1 | Cross-section (single year/period) | across units only | [[02 What Makes Data a Panel?]] |
| 2 | Time series (single unit) | across time only, for one unit | [[02 What Makes Data a Panel?]] |
| 3 | Pooled OLS | across units *and* time, blended | [[04 Pooled OLS and the Unobserved-Effects Model]] |
| 4 | Fixed effects (within estimator) | within-unit only, $\alpha_i$ eliminated | *later chapter* |
| 5 | First differences | period-to-period change within a unit | *later chapter* |

> [!tip] The closing question this unit is building toward
> We now know a common pooled intercept may be inadequate, and that if $\alpha_i$ is unobserved, constant through time, and potentially correlated with $X_{it}$, pooled OLS is biased. The natural next question: **can the repeated observations be transformed so that $\alpha_i$ disappears algebraically, without ever having to measure it?** That is exactly what the fixed-effects (within) transformation and first-differencing do — both exploit the fact that $\alpha_i$ has no $t$ subscript, so differencing any unit's data against itself (its own mean, or its own previous period) cancels $\alpha_i$ out identically. This is deliberately not yet built out here — it is the next natural chapter once this foundational material is solid.

A separate, more advanced lab (`BSE4211_Grunfeld_Pooled_LSDV_FE_FD_Lab.do`) already exists among the source materials and works through pooled OLS, the LSDV/`testparm` poolability test, `xtreg, fe` with a manual demeaning check, and first differences via the `D.` operator with and without clustering — useful to revisit once the fixed-effects and first-differences theory chapters exist to anchor it.

## 5. What the Week 3 labs deliberately do NOT cover

Both the Stata and R Week 3 labs are explicit about this boundary, and it's worth stating plainly rather than discovering it by surprise mid-lab: country/firm indicators are used **only** for the joint poolability test, and a descriptive regression of deviations from unit means is used only to *visualize* within-unit co-movement — **explicitly not introduced as a fixed-effects estimator**. "Do NOT estimate fixed-effects or random-effects models" is stated outright in the Grunfeld Week 3 lab. This is a deliberate pedagogical sequencing choice, not an oversight — it keeps this unit's current material (cross-section, time series, pooled OLS, poolability, clustering) cleanly separated from the FE/RE/FD estimators that follow.

## 6. Cheat sheet: Stata ↔ R panel commands

| Task | Stata | R (`plm`) |
|---|---|---|
| Declare panel structure | `xtset unit time` | `pdata.frame(df, index = c("unit","time"))` |
| Describe panel shape | `xtdescribe` | `pdim()` |
| Check balance | (from `xtdescribe`) | `is.pbalanced()` |
| Between/within/overall stats | `xtsum` | manual group means / deviations |
| Pooled OLS | `reg y x1 x2` | `plm(y ~ x1 + x2, model = "pooling")` |
| Clustered SEs | `reg ..., vce(cluster unit)` | `vcovHC(m, cluster = "group")` via `coeftest()` |
| Poolability (Chow/F) | `reg y x1 x2 i.unit` + `testparm i.unit` | `pFtest(m_within, m_pool)` |

## 7. Open questions for revision

- [ ] For the 1954 Grunfeld cross-section only: who is being compared with whom, and why is the resulting coefficient not a statement about what happens when *one* firm's market value changes through time?
- [ ] For the company-1-only time series: why should this specifically *not* be called a "within estimator," even though it only uses one firm's own variation?
- [ ] Write, in your own words, why pooled OLS's mvalue coefficient uses *both* cross-firm and within-firm comparisons, while the cross-section and time-series models each use only one.
- [ ] Using the pooled Grunfeld residuals, describe (without formally testing it yet) how you'd check whether some firms are persistently above or below the common regression line — and connect this informally to what the poolability test formalizes.
- [ ] Build the three-row comparison table from the labs — cross-section / time series / pooled OLS — reporting for each: unit of observation, source of identifying variation, sample size, the mvalue and kstock coefficients, what the coefficient can legitimately be called, and one major threat to its interpretation.

---
*Source: `Running_examples.docx`, `Grunfeld_WorldBank_Panel_Regression_Lab_1.Rmd`, `Week_3_Lab_Grunfeld_data.do`, `Week_3_Lab_World_Bank.Rmd`, `World_Bank_lab_read_me.txt`, `BSE4211_Grunfeld_Pooled_LSDV_FE_FD_Lab.do`.*
