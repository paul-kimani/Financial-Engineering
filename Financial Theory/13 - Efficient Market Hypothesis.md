# 13 — Efficient Market Hypothesis

> Part III asks when market prices fully reflect available information.
> The **Efficient Market Hypothesis (EMH)** answers: under the right
> conditions, prices adjust to new information so fast that exploiting
> any predictable pattern is futile. Three forms — **weak**,
> **semi-strong**, **strong** — differ only in *which information set*
> the prices reflect.

---

## 1. The core claim

In an efficient market, **the current prices of securities reflect all
relevant information**. Two corollaries follow immediately:

- Only **new** information moves prices.
- New information is, by definition, **unpredictable** — otherwise
  the prediction would already be in today's prices.

Hence price changes in response to new information must also be
unpredictable. Empirically this shows up as the **random walk
hypothesis** — successive price changes in individual securities
appear independent in an efficient market.

A formal definition:

> **Efficient market.** A market in which security prices adjust
> rapidly to new information, so that the current price reflects all
> information available about the security.

Costs of information acquisition and trading must be zero (or
negligible). When those costs are positive, prices reflect all
information until the **marginal cost** of obtaining and trading on
new information just equals its **marginal benefit**.

> Competition among investors is the mechanism, not the source: it
> drives the full effects of new information on intrinsic values to be
> reflected nearly instantaneously in actual prices.

### Over- vs under-adjustment

Because *new* information is itself uncertain, instantaneous
adjustment has two consequences in practice:

- Actual prices will initially **over-adjust** to intrinsic-value
  changes as often as they **under-adjust**.
- The lag in complete adjustment is itself an independent random
  variable — sometimes the adjustment *precedes* the event (when the
  event is anticipated), sometimes it *follows*.

Net: **successive price changes are independent in an efficient
market** — the random walk hypothesis.

---

## 2. Three forms of the EMH

The forms differ in which information set $\Omega_t$ the prices are
assumed to reflect.

### 2.1 Weak-form EMH

Prices reflect **any information contained in the history of the
security itself**, including:

- Past market trading data — historical sequence of prices, returns,
  trading volume.
- Other market-generated data — e.g. odd-lot transactions.

**Implication.** Trend analysis on historical prices is futile.
Past prices are public and virtually costless to obtain; if they ever
reliably signalled future performance, all investors would already
have learned to exploit them. The weak-form EMH says trading rules
based on past market data should not deliver excess return.

### 2.2 Semi-strong-form EMH

Prices reflect **all publicly available information** about the firm's
prospects. This includes the weak-form set (past prices) **plus**
non-market public information: earnings and dividend announcements,
dividend yields, $P/E$ ratios, stock splits, news about the economy
and political news.

**Implication.** Traders cannot earn above-average risk-adjusted
returns using important new public information — prices have already
incorporated it.

### 2.3 Strong-form EMH

Prices reflect **all** information, both **public and private**. No
group of investors — including company insiders — has monopolistic
access to price-forming information.

**Implication.** No subset of investors, insiders included, can
consistently derive above-average risk-adjusted returns.

| Form         | Information set $\Omega_t$ reflected | Empirical evidence                                       |
| :----------- | :----------------------------------- | :------------------------------------------------------- |
| Weak         | Past market data                     | Mostly supported (filter rules unprofitable after costs).|
| Semi-strong  | All public information               | Mostly supported; some anomalies (see chapter [[15 - EMH Limitations and Anomalies]]). |
| Strong       | Public **and** private information   | Generally **rejected** — insiders do earn excess return. |

---

## 3. Implications for security analysis

### Technical analysis

Technical analysis assumes predictably recurring price patterns. The
EMH says if any such pattern were exploitable, investors would chase
it until prices moved and the pattern dissolved. So **technical
analysis has no merit** under the weak-form EMH — abnormal return
cannot be systematically achieved from chart patterns alone.

### Fundamental analysis

Fundamental analysts hope to extract insight about a firm's future
that the rest of the market has missed. Under the semi-strong form
this is doomed: many well-resourced analysts are already trawling the
same public data, and the consensus view is already reflected in the
price.

The trick, then, is not to identify good firms but to **find firms
better than everyone else's estimate**. Only **superior** analysis —
not merely accurate analysis — produces abnormal return when prices
already reflect commonly available information.

---

## 4. Implications for portfolio management

Two strategy archetypes:

- **Active management.** Continuously revising the portfolio in line
  with forecasts to beat the market by identifying mispriced
  securities. Under EMH this is futile *and* costly (brokerage fees,
  transaction costs).
- **Passive management.** Holding a well-diversified portfolio with no
  attempt to outguess the market. Typically a **buy-and-hold**
  strategy.

The EMH points toward passive management as the rational default.
**But** efficiency does not abolish portfolio management — the basic
principle of **diversification** still applies. Each security has its
own systematic risk exposure, and rational selection of a
well-diversified portfolio is required even in a perfectly efficient
market to achieve the investor's desired systematic-risk level.

Other practical considerations on top of diversification:

- **Tax.** High-tax-bracket investors lean toward tax-exempt securities
  or capital-gains-heavy portfolios.
- **Risk profile.** Aged investors avoid long-duration fixed income;
  younger investors take more equity. The portfolio manager tailors
  the investment policy to each investor's age, tax bracket, risk
  profile, employment status, and goals.

---

## 5. Efficient-markets observations

A short list of practical takeaways:

- Investors should expect a **fair** return on their investment, not
  more.
- Fundamental and technical analysis will not be reliably fruitful.
- **Markets are efficient only if investors believe they are
  inefficient.** It is precisely the constant search for mispricing
  that drives prices to efficiency.
- Publicly known investment strategies cannot be expected to generate
  abnormal returns.
- Past performance is **not** an indicator of future performance.
- Professional investors should fare no better in picking securities
  than ordinary investors.
- Since market prices always reflect intrinsic values, the search for
  mispriced securities is futile.

---

## 6. Mathematical formulation

Under the EMH, the current price $P_t$ already incorporates all
relevant information. The only reason for prices to change between $t$
and $t + 1$ is the **arrival of news** $\varepsilon_{t+1}$:

$$
P_{t+1} \;=\; \mathbb{E}_t\bigl[P_{t+1}\bigr] \;+\; \varepsilon_{t+1},
$$

where $\mathbb{E}_t[\cdot]$ is the expectation conditional on the
information available at $t$. Two properties follow.

### Unbiasedness

$$
\mathbb{E}_t\bigl[\varepsilon_{t+1}\bigr] \;=\; 0
\quad\Longleftrightarrow\quad
\mathbb{E}_t\bigl[P_{t+1}\bigr]
\;=\; P_{t+1} \text{ on average}.
$$

The forecast is **unbiased**.

### Orthogonality

The forecast error $\varepsilon_{t+1}$ must be **independent of any
information** $\Omega_t$ available at time $t$ or earlier:

$$
\boxed{\;\mathbb{E}\bigl[\varepsilon_{t+1} \cdot f(\Omega_t)\bigr] \;=\; 0 \quad \forall \text{ measurable } f.\;}
$$

This is the **orthogonality property**. If $\varepsilon_t$ is serially
correlated — for instance an AR(1)

$$
\varepsilon_{t+1} \;=\; \rho\,\varepsilon_t + v_{t+1}, \quad \rho \neq 0 \;-
$$

then $\varepsilon_t \in \Omega_t$ and orthogonality is violated, the
EMH fails. Tests for serial correlation (chapter
[[14 - Tests of the EMH]]) are exactly tests of this property.

### Returns version

Translating from prices to returns,

$$
\varepsilon_{t+1} \;=\; R_{t+1} - \mathbb{E}_t\bigl[R_{t+1}\bigr],
$$

where $\mathbb{E}_t[R_{t+1}]$ is the **required return** at $t$. Most
empirical work uses an asset-pricing model to pin down
$\mathbb{E}_t[R_{t+1}]$:

- **CAPM** — single-factor mean-variance model;
- **APT** — multi-factor arbitrage-pricing model.

EMH tests therefore become **joint tests** of market efficiency *and*
the chosen asset-pricing model — a source of ambiguity discussed in
chapter [[15 - EMH Limitations and Anomalies]].

---

## Cross‑references

- [[14 - Tests of the EMH]] — empirical tests of the orthogonality
  property.
- [[15 - EMH Limitations and Anomalies]] — observed deviations from
  the theory.
- [[00 - Introduction]] — the research-cycle phase where EMH is
  empirically validated or challenged.
- [[01 - Models in Financial Economics]] — CAPM and APT, the joint-test
  companions to any EMH test.
- [[../Stochastics/09 - Martingales]] — the discounted-price martingale
  is the formal sibling of unbiasedness here.
- [[../Stochastics/01 - Introduction to Stochastic Processes]] —
  random-walk vocabulary.
- [[../Time Series/10 April 2026]] — stochastic vs deterministic series;
  stationarity issues feed into EMH testing.
- [[../Derivatives/Lehman Brothers 2007]] — efficient markets did not
  prevent the 2008 blow-up; informational efficiency and *equilibrium*
  efficiency are not the same thing.
- [[../Stochastics/31.1 - Law of One Price]] — no-arbitrage as the foundation of market efficiency.
- [[../Stochastics/21 - Martingale Pricing Of European Contingent Claims]] — efficient, arbitrage-free pricing in action.

