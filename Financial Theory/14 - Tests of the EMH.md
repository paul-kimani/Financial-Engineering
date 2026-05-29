# 14 — Tests of the EMH

> Empirical tools for checking whether market prices behave the way
> the EMH (chapter [[13 - Efficient Market Hypothesis]]) predicts.
> Short-term predictability tests target the **weak form**; event
> studies target the **semi-strong form**; long-term tests reveal
> mean-reversion patterns that complicate the picture.

---

## 1. Short-term predictability tests (weak-form)

The weak-form claim is that no rule based on past prices can deliver
abnormal returns. Three classical tests target it.

### 1.1 Serial-correlation tests

Test whether price changes (or proportional price changes) are related
over time:

$$
r_t \;=\; a \;+\; b\,r_{t - 1 - T} \;+\; \varepsilon_t,
$$

where:

- $a$ — expected return, unrelated to the previous return;
- $b$ — sensitivity of today's return to a previous return;
- $T = 0$ means yesterday's return is the regressor; $T = 1$ means
  two days ago; and so on;
- $\varepsilon_t$ — random error capturing variability not related to
  past returns.

**Interpretation.** Under weak-form EMH, $b$ should be statistically
indistinguishable from zero — past returns carry no information about
future returns. The **coefficient of determination** $R^2$ (square of
the correlation between $r_t$ and $r_{t - 1 - T}$) is the fraction of
the variation in today's return explained by the past return; under
EMH it should be near zero.

This is the **orthogonality property** of chapter
[[13 - Efficient Market Hypothesis]] specialised to a linear test.

### 1.2 Runs tests

Runs tests **ignore magnitudes** and look only at the **sign** of price
changes. Designate:

- price increase $\Rightarrow$ $+$
- price decrease $\Rightarrow$ $-$

A **run** is a consecutive sequence of identical signs. For example,
the sequence

$$
- \; - \; - \; + \; + \; + \; - \; -
$$

contains **four** runs.

Under weak-form EMH, the number of runs in a price-change series
should match the distribution expected under independent signs. A
statistically *low* number of runs (long blocks of same-direction
changes) signals positive serial correlation; a statistically *high*
number signals negative serial correlation.

### 1.3 Filter tests

A more sophisticated form of trend analysis. A **filter rule**
provides a mechanical buy/sell trigger:

> "Purchase the security if it rises by at least $X\%$ from the
> previous low, and hold until it declines by at least $Y\%$ from the
> subsequent high — at this point, sell short or hold cash."

It is a **timing strategy**. Performance is compared against a passive
**buy-and-hold** benchmark.

**Result.** Filter rules can outperform buy-and-hold in the long run
in *gross* terms, but transaction costs typically eliminate the
advantage. Net of costs, they fail to beat buy-and-hold — consistent
with the weak-form EMH.

---

## 2. Long-term predictability tests

Long-horizon studies (Fama–French; Poterba–Summers) reveal
**prolonged negative serial correlation** at multi-year horizons —
violating naive weak-form predictions.

The interpretation is the **fad hypothesis**: security prices may
overreact to relevant news. Overreaction creates negative serial
correlation in the long run:

- A subsequent correction generates **poor performance after good
  performance** and **vice versa**.
- A run of positive returns tends eventually to be followed by
  negative returns.

This is **mean reversion** — central to value-investing strategies and
itself the subject of chapter [[15 - EMH Limitations and Anomalies]].

---

## 3. Event studies (semi-strong form)

Event studies measure the **speed** with which security prices react
to the release of information, and whether the returns following the
announcement are **normal**, **abnormal**, or **supernormal**.

They are **joint tests** — testing market efficiency *and* the
asset-pricing model used to define "expected return" (CAPM, APT, or a
market index model).

### 3.1 The single-index expected-return model

$$
r_t \;=\; a \;+\; b\,r_{Mt} \;+\; \varepsilon_t,
$$

where $r_{Mt}$ is the market return. The firm-specific (abnormal)
return is the residual

$$
\varepsilon_t \;=\; r_t \;-\; (a + b\,r_{Mt}),
$$

which is the difference between actual return and the expected return
given the stock's sensitivity to the market.

### 3.2 Event-study methodology

The standard recipe.

1. **Sample.** Collect firms that had a surprise announcement (the
   event).
2. **Event day.** Identify the precise day of the announcement and
   designate it day $0$. Use the smallest feasible time intervals —
   daily or intraday data.
3. **Window.** Define the period to be studied. Days before the event
   are labelled $-T, -T+1, \dots, -1$; days after are $+1, +2, \dots, +T$;
   the event day itself is $0$.
4. **Returns.** Compute the return on each day for each firm.
5. **Abnormal returns.** For each day, subtract the expected return
   (from the single-index model, CAPM, or APT) to get the abnormal
   return $\varepsilon_t$.
6. **Average across firms.** Compute the **average abnormal return**
   (AAR) for the sample on each day in the window.
7. **Cumulate.** Sum daily AARs to get the **cumulative average
   abnormal return** (CAAR) from the start of the window.

### 3.3 Interpreting the pattern

Under semi-strong EMH, one expects a **large abnormal return on the
day of the announcement** and **no significant abnormal returns on
other days** in the window.

Reasons abnormal returns might appear *after* day $0$:

- Information taking time to be fully reflected in price (i.e. lazy
  semi-strong efficiency).
- The announcement happening very late on day $0$, after market
  close.

Reasons abnormal returns might appear *before* day $0$:

- News release that an announcement will take place.
- The announcement event itself being partly caused by prior abnormal
  returns (e.g. a stock split prompted by prior price runs).
- **Information leakage.**
- Concentrated trades, e.g. someone accumulating large blocks of
  shares before an impending takeover.

---

## 4. Market rationality

A subtle but distinct question from informational efficiency.

> **Market rationality.** Do market prices accurately reflect
> investors' expectations about the **present value of future cash
> flows**?

A market is **rational** if market prices are not significantly
different from intrinsic values. EMH and rationality are usually
discussed together but they are not the same statement: a market can
be informationally efficient (rapidly digesting news) yet
non-rational (overreacting to non-economic events like stock splits or
mergers).

Any systematic response to **non-economic variables** is evidence of
*irrationality*. Cataloguing those responses is the topic of chapter
[[15 - EMH Limitations and Anomalies]].

---

## 5. Returns and firm characteristics

A persistent embarrassment for the EMH: empirically, returns are
related to firm characteristics such as

- size,
- market value divided by book value,
- earnings divided by price.

These relationships go **against** strict EMH — they imply a public,
free piece of information (the firm's accounting figures) predicts
excess returns. Standard explanations:

- The characteristic is a **proxy for an omitted risk variable**; once
  the omitted variable is added to the model, the relationship
  disappears.
- The statistical techniques used by some researchers are
  inappropriate.
- **Model misspecification** — e.g. CAPM using a lower-than-normal
  beta.
- The market really is **inefficient** with respect to these
  characteristics.

Each of these is alive in the literature; none has settled the
question.

---

## Cross‑references

- [[13 - Efficient Market Hypothesis]] — the theory these tests check.
- [[15 - EMH Limitations and Anomalies]] — observed empirical
  deviations, including the size and book-to-market effects.
- [[01 - Models in Financial Economics]] — CAPM / APT, the joint-test
  asset-pricing models.
- [[../Time Series/10 April 2026]] — serial-correlation and
  stationarity vocabulary used in §1.1.
- [[../Time Series/24 April Assignment]] — the OLS apparatus behind
  the serial-correlation regression.
- [[../Stochastics/01 - Introduction to Stochastic Processes]] —
  random-walk / AR(1) vocabulary.
- [[../Stochastics/12 - GBM Parameter Estimation]] — annualised
  estimates of $\mu$ and $\sigma$ from log returns are the inputs to
  most return-model tests.
