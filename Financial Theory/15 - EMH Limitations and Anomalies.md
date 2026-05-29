# 15 — EMH Limitations and Anomalies

> Empirical anomalies and theoretical defects of the EMH. Price
> overreactions, excess volatility, seasonal patterns, the failure of
> CAPM as a complete return model, and the explanatory power of
> non-CAPM factors all complicate the clean efficient-markets picture
> in chapter [[13 - Efficient Market Hypothesis]].

---

## 1. Empirical anomalies

### 1.1 Price overreactions

Prices of individual stocks **overreact** to information and then
undergo corrections.

> **Reversal effect.** Poorly performing stocks in one period tend to
> experience *good* performance in the subsequent period; best-performing
> stocks tend to follow with *poor* performance.

This is exactly the long-horizon negative serial correlation flagged
in chapter [[14 - Tests of the EMH]] (the "fad hypothesis"). Reversal
strategies (buy past losers, sell past winners) have produced positive
returns in long-run studies — a clear weak-form violation.

### 1.2 Excess volatility

> **Excess volatility.** Stock-market volatility is empirically much
> higher than the volatility of dividends.

If prices were rational present values of future dividends and
discount rates moved only with macro fundamentals, price volatility
should be of the same order as dividend volatility. It is not — by an
order of magnitude. The standard interpretation is that the market
**overreacts in aggregate** because investors chase fads and engage in
herd-like behaviour.

### 1.3 Seasonal patterns

Calendar regularities should not exist under EMH — but they do:

- Hourly, daily, monthly, and quarterly patterns.
- The **weekend effect** — stock returns tend to be **negative** from
  Friday close to Monday opening.

Each is a public, free signal whose persistence violates strict
semi-strong-form EMH.

---

## 2. Failure of CAPM

CAPM is the primary tool for measuring an investor's expected return.
Its single-factor structure leads to a clear empirical prediction:
high-$\beta$ stocks earn higher expected returns than low-$\beta$
stocks, proportional to $\beta$.

In reality this is **not always** the case. High-beta stocks have, in
several large samples, **failed to earn proportionally high returns**.
That breaks not just CAPM but every EMH test built on top of CAPM —
a CAPM mis-specification can manifest as an apparent EMH violation.

This is the **joint-test problem** in concrete form (see chapter
[[14 - Tests of the EMH]] §3): when an event study or a return-pattern
test fails, we cannot tell whether efficiency failed, the model
failed, or both.

---

## 3. Explanatory power of non-CAPM factors

Several characteristics that should be irrelevant under EMH + CAPM
empirically **outperform** their CAPM-implied returns:

- **Small-cap stocks.** Small market-capitalisation firms beat CAPM
  expectations on a risk-adjusted basis.
- **Low market-to-book** ("value") stocks.
- **High dividend yield** stocks.
- **Low $P/E$ ratio** stocks.

These regularities motivated investment strategies like **yield
tilting** and **small-cap value tilting**, both of which have produced
historical excess returns. They also motivated multi-factor models
(Fama–French three-factor and beyond) that augment CAPM with size and
value factors.

Each non-CAPM factor is, again, ambiguous between "the world is
inefficient" and "the CAPM was the wrong model in the first place" —
the joint-test problem in another guise.

---

## 4. Defects in "efficiency" as a model of stock markets

Three theoretical defects in the EMH framework itself.

### 4.1 Failure to incorporate information-acquisition and processing costs

EMH assumes the cost of gathering information is essentially zero.
For widely-followed large-cap stocks this is reasonable; for small-cap
stocks it is not. Information about small firms is costly to obtain
and process. **Rational investors then expect higher returns from
small-cap stocks to compensate for that cost** — providing a
non-anomalous explanation for the small-cap effect (§3) within an
extended-efficiency framework.

### 4.2 Heterogeneous information and beliefs

EMH typically treats investors as **homogeneous and objective**. In
reality investors differ in:

- the information they actually access (despite "public"
  availability);
- the beliefs they form from that information;
- their **confidence** and **sentiment**, which play documented roles
  in price formation (recall the fad / overreaction picture in §1).

Heterogeneity is fundamental to bubble formation, herd behaviour, and
the excess-volatility phenomenon — none of which fit cleanly into a
homogeneous-investor EMH.

### 4.3 Failure to consider transaction costs

The EMH assumes prices reflect all information until the **marginal
cost** of trading just balances the **marginal benefit**. In practice
transaction costs are positive and asymmetric (chapter
[[08 - Fisher's Separation Theorem]] §5 has the analogous breakdown
for Fisher separation). Strategies that look profitable gross — like
filter rules of chapter [[14 - Tests of the EMH]] §1.3 — become
unprofitable net of transaction costs.

That, somewhat paradoxically, *protects* the EMH at high-frequency
horizons: many apparent inefficiencies are not exploitable, hence not
truly inefficient in any operational sense.

---

## 5. Where this leaves the theory

The picture is nuanced rather than simple.

| Phenomenon                       | EMH compatible?                                                     |
| :------------------------------- | :------------------------------------------------------------------ |
| Weak-form short-horizon          | Mostly **yes** — after transaction costs.                           |
| Semi-strong daily event studies  | Mostly **yes** — most adjustment is fast.                           |
| Insider trading excess returns   | **No** — strong-form EMH is clearly rejected.                       |
| Long-horizon mean reversion      | **No** — reversal effects are real.                                 |
| Excess aggregate volatility      | **No** — prices are noisier than fundamentals justify.              |
| Size, value, dividend yield      | **Possibly** — depending on whether one accepts the joint-test ambiguity. |

Operationally, the EMH is a useful **first-order approximation** and
a sobering check on active strategies. It is not, however, the final
word — chapter
[[13 - Efficient Market Hypothesis]] §3 already pointed toward
diversification as the policy that survives even in an efficient
world; the anomalies in this chapter motivate the modern multi-factor
models that have replaced single-factor CAPM in practice.

The Lehman Brothers case study in
[[../Derivatives/Lehman Brothers 2007]] is the cautionary endgame:
informational efficiency in a *narrow* sense (OTC derivatives prices
reflected the going view of risk) did **not** prevent catastrophic
mispricing of systemic risk. The research cycle of
[[00 - Introduction]] §2 closes — the practical phase generates
questions that loop back into the theoretical phase.

---

## Cross‑references

- [[13 - Efficient Market Hypothesis]] — the theory whose limits are
  catalogued here.
- [[14 - Tests of the EMH]] — the empirical apparatus that uncovers
  the anomalies in §1.
- [[01 - Models in Financial Economics]] — CAPM and its joint-test
  partnership with EMH (§2, §3).
- [[08 - Fisher's Separation Theorem]] §5 — analogous breakdown of a
  clean theoretical result once frictions are added.
- [[00 - Introduction]] — the research cycle that the anomalies here
  re-enter.
- [[../Derivatives/Lehman Brothers 2007]] — informational efficiency
  ≠ systemic stability.
- [[../Time Series/10 April 2026]] — stationarity / non-stationarity
  vocabulary; long-horizon mean reversion is a stationarity statement.
- [[../Stochastics/09 - Martingales]] — the formal risk-neutral
  martingale property is the no-arbitrage analogue of EMH that does
  *not* obviously break under the anomalies above.
