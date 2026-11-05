# 01 — The Data Life Cycle

> Raw data is never model-ready. This chapter is the pipeline that turns it
> into something a regression or a backtest can be trusted on — and, more
> importantly, a catalogue of the specific ways that pipeline silently
> manufactures results that cannot survive contact with live markets.

---

## 1. The pipeline

For quantitative analysis the data does not arrive prepared. It moves through
stages:

$$
\text{Raw} \;\to\; \text{Cleaned} \;\to\; \text{Prepared} \;\to\; \text{Data + Results} \;\to\; \text{Archived}
$$

- **Raw data.** Where we start — unprocessed, unfiltered, as delivered by the
  vendor or exchange.
- **Cleaned data.** Immediate errors fixed (missing values, duplicates,
  corrupt ticks, unit changes).
- **Prepared data.** Restructured and transformed for a particular model
  (scaled, encoded, pivoted, lagged).
- **Data + Results.** Outputs stored alongside the exact inputs that produced
  them.
- **Archived data.** Frozen, documented, reproducible.

**The point of the last two stages.** A result you cannot reproduce from
archived inputs is not a result, it is an anecdote. Archiving is what makes the
difference.

---

## 2. Population vs. sample

The **population** is the entire domain under study; a **sample** is a subset
of it. For a sample to be *representative*, selection must not be correlated
with the outcome being measured. The textbook shorthand is "every element has
an equal chance of selection", but in finance the binding constraint is
subtler than equal probability — it is *what the selection rule knows*.

### 2.1 Survivorship bias

Suppose we test a momentum strategy on US equities by downloading daily prices
for **the stocks in the S&P 500 today**, back to 2000.

- **Population:** every US stock that traded at any point from 2000 to today —
  *including the ones that no longer exist*.
- **Sample:** the stocks in the index **today**, with history followed
  backwards.

That single word *today* is the whole problem. Membership in the sample is
determined by **what happened after 2000**:

- **Systematically excluded:** firms that went bankrupt, were acquired, or
  shrank out of the index — Enron (2001), WorldCom (2002), Lehman (2008), and
  hundreds of quieter cases.
- **Systematically included:** firms that grew *into* the index, carrying
  their full early history — Nvidia appears with its 2000-era prices, when no
  large-cap screen would have contained it.

So the selection rule uses **information from the future**, and that
information is correlated with returns. This is **survivorship bias**, a
special case of **selection on the outcome variable**.

**Not a size story.** The bias is not "large caps perform best" — historically
the size premium runs the other way (the SMB factor of
[[06 CAPM and Multifactor Models]]). The bias is that we kept only the names
we *now know* survived. Momentum is especially exposed, since it buys recent
winners, and every winner whose run ended in a delisting has been deleted.

### 2.2 Sizing the bias

Ten stocks, each \$100 in 2000. By 2025: three delisted at \$0, four at \$150,
three at \$300.

True equal-weighted population return:

$$
\bar r_{\text{pop}}=\frac{3(-100\%)+4(+50\%)+3(+200\%)}{10}=\frac{-300+200+600}{10}\%=+50\%
$$

Cross-check on prices: mean terminal price
$=\tfrac{3(0)+4(150)+3(300)}{10}=\tfrac{1500}{10}=\$150$, i.e. \$100 → \$150,
$+50\%$.

Survivors-only, reweighting over the seven that remain:

$$
\bar r_{\text{surv}}=\tfrac{4}{7}(50\%)+\tfrac{3}{7}(200\%)=\frac{200+600}{7}\%\approx+114.3\%
$$

**Bias $\approx 64.3$ percentage points** — the reported figure is more than
double the truth, purely from how the dataset was assembled, before a single
model is fitted. Note the mechanism: deleting the three $-100\%$ outcomes
removes the entire left tail, so the mean is pushed up *and* the dispersion is
understated. Risk estimates are corrupted along with return estimates.

### 2.3 The fix — two ingredients

1. **Point-in-time universe.** At each date $t$, the strategy may only select
   from the stocks that were *actually in the index at $t$*. This requires
   storing index membership **with effective dates**, not today's constituent
   list.
2. **Keep dead stocks, record the delisting return.** A stock stays in the
   panel for its whole life, and its final observation records what a holder
   *actually received*. That is not always $-100\%$:
   - **Bankruptcy** → close to $-100\%$.
   - **Cash acquisition** → frequently *positive*, since acquirers pay a
     premium.

   CRSP stores this as a dedicated **delisting return** field.

**Wald's bombers.** Engineers in WWII wanted to armour the parts of returning
aircraft that showed bullet holes. Abraham Wald pointed out that the planes hit
elsewhere *did not return*. Whenever entry into the dataset depends on the
outcome, the data lies in a predictable direction. The same structure recurs
as: mutual-fund databases dropping dead funds (inflating average fund
performance), hedge-fund indices where funds report only while the numbers
flatter them, and — in econometrics — truncated samples and the **Heckman
selection correction**.

---

## 3. Metadata

Data about data; the documentation layer. It tracks the dataset and every
transformation applied, prevents nonsensical operations (averaging a
categorical code, adding prices denominated in different currencies), and is
what makes research understandable and reproducible.

Its single most important job in finance is recorded in §7: storing **when a
value became known**, not merely what period it describes.

---

## 4. Exploratory Data Analysis (EDA)

The first pass at profiling a dataset: classify it, validate it against the
metadata, and inspect its distribution using visual tools (histograms, KDE —
see [[Kernel Density Function]]) and descriptive statistics (measures of
centrality and dispersion).

**EDA is diagnosis, not treatment.** Its purpose is to tell you *which* of the
problems below you have, before you choose a fix.

---

## 5. Data cleaning (wrangling)

Fixing immediate errors. Typical financial faults: stale or outdated values,
gaps in time series, manual-entry mistakes, and shifts in measurement scale.

### 5.1 Missing values — the three mechanisms

Whether a fix is safe depends entirely on *why* the value is missing. Let $Y$
be the variable with missing entries, $X$ the variables observed, and $M$ an
indicator equal to 1 when $Y$ is missing.

| Mechanism | Formal condition | In words |
| :--- | :--- | :--- |
| **MCAR** (missing completely at random) | $P(M\mid Y,X)=P(M)$ | Missingness is pure noise |
| **MAR** (missing at random) | $P(M\mid Y,X)=P(M\mid X)$ | Missingness depends only on *observed* variables |
| **MNAR** (missing not at random) | $P(M\mid Y,X)$ depends on $Y$ | The missing value causes its own missingness |

The corresponding rules:

- **Row deletion** (listwise deletion) is unbiased **only under MCAR**.
- **Imputation conditioning on $X$** handles **MAR**.
- **MNAR** cannot be fixed mechanically — it requires external information or
  an explicit model of the missingness process.

**Worked case — the missing delisting return.** Suppose delisted stocks are in
the panel but 30% have no final return. Two quick fixes, and what each
assumes:

- **Drop them** → assumes the missing names resemble the retained ones, i.e.
  MCAR.
- **Fill with $0\%$** → assumes a clean exit at the last traded price with no
  loss.

**Careful: a $0\%$ return is not a \$0 price.** A \$0 price is $-100\%$; a
$0\%$ return means the price did not move. Filling with zero is the *opposite*
of assuming bankruptcy.

Is the missingness random? No:

- **Mergers** settle at a known cash or share price → final value cleanly
  recorded.
- **Bankruptcies and listing-standard failures** get pushed to OTC markets and
  fade out → no clean final price.

So the observations most likely to be *missing* are those with the *worst*
values: this is **MNAR**. Dropping them deletes the left tail (strong upward
bias, the §2.2 mechanism); filling with $0\%$ replaces a large loss with none
(smaller upward bias, same direction).

**Empirically:** Shumway (1997) recovered the missing performance-related
delisting returns on NYSE/AMEX and found they averaged roughly $-30\%$;
follow-up work found around $-55\%$ for Nasdaq. Standard practice is therefore
to impute **by delisting reason** — merger price for mergers, $\approx-30\%$
for performance delistings.

**Why that works, mechanically.** Conditioning on the **delisting code**,
which *is* observed, converts an MNAR problem into approximately MAR *within
each group*. This is the practical payoff of metadata: the right auxiliary
variable, recorded at collection time, turns an unfixable problem into a
fixable one.

### 5.2 Three worked classifications

- **Random ticks dropped during a server outage unrelated to market
  conditions** → **MCAR**. Deletion is unbiased. **But** in a time series,
  deleting a row destroys equal spacing: a return computed across the gap is a
  two-period return masquerading as one. Either compute returns only between
  adjacent *observed* timestamps, or resample onto a fixed grid and mark the
  gap explicitly.
- **Missing NSE prices on zero-volume days, with volume recorded** → **MAR**
  given volume. Fixable, but the usual fix — **forward-filling** the last
  price — has side effects. A forward-filled day has return exactly $0\%$ and
  the price then catches up in one jump, so for thinly traded counters:
  daily volatility is **understated**, returns appear **autocorrelated**, and
  estimated **betas are biased toward zero** because the stock appears to
  react late. This is why **Scholes–Williams** and **Dimson** betas regress on
  lagged as well as contemporaneous market returns — directly relevant to NSE
  work, where many counters trade thinly.
- **Hedge funds reporting voluntarily and ceasing after bad months** →
  **MNAR**. No mechanical fix. Worse, the bias runs both ways: funds *join* a
  database after a good run and import their track record
  (**backfill / instant-history bias**). Bad endings vanish, good beginnings
  are added; published hedge-fund index returns are widely held to be
  overstated by several percentage points a year on these two effects
  together.

### 5.3 Outliers

The governing rule: **judge an outlier by its cause, not its size.**

- **Gold falls $\approx9\%$ in a day, present on every vendor, huge volume,
  wall-to-wall news** → **real event**. Keep it. Cross-vendor confirmation
  plus volume plus news *is* the evidence.
- **A 1-minute XAUUSD bar spikes $3\%$ and fully reverses, on one broker's
  feed only** → **data error** for research; cross-check against other
  vendors and remove or replace with the consolidated price. **Nuance:** if
  that feed is the one a bot trades on, the spike can trigger real stops — so
  it is an error for *strategy research* and real data for *execution risk on
  that broker*. Whether an outlier is an error depends on the question being
  asked.
- **A $-50\%$ daily return on the day of a 2-for-1 split, flat thereafter** →
  **error in the return**, not an event. The price move happened; the return
  did not. Holding one share at $P$ becomes two shares at $P/2$:

  $$
  \text{wealth}_{\text{after}}=2\times\tfrac{P}{2}=P=\text{wealth}_{\text{before}}\;\Rightarrow\; r=0\%
  $$

  Divide pre-split prices by the split factor, or use the vendor's adjusted
  close. Dividends need the same treatment: the ex-date price drop is not a
  loss to shareholders. These are the **shifts in measurement scale** named
  above — a split is a change of units, like switching from shillings to cents
  mid-series.

**Why "delete anything beyond $3\sigma$" is dangerous.** Under normality,

$$
P(|Z|>3)=2\big(1-\Phi(3)\big)\approx2(0.00135)=0.0027\approx0.27\%
$$

so with $\approx252$ trading days, $252\times0.0027\approx0.68$ — **fewer than
one such day per year**. Three problems:

1. **Fat tails.** Real returns have excess kurtosis (see
   [[03 Asset Returns.]]); $3\sigma$ days occur far more often than $0.27\%$,
   and far larger moves occur at all. Black Monday 1987 was $\approx-20\%$ in
   a day — a $20\sigma$-plus event, effectively impossible under a Normal. The
   rule deletes precisely the observations that constitute drawdowns and
   blow-ups.
2. **Downward-biased risk.** Remove the tails and volatility, VaR and maximum
   drawdown all look calmer than reality. Any position sizing built on those
   numbers — ATR-based risk in a cBot, for instance — is then too aggressive.
3. **Masking.** $\sigma$ is estimated *from data containing the outliers*. One
   large spike inflates $\sigma$, pushing smaller genuine errors inside the
   $3\sigma$ fence. Robust scale estimators (median absolute deviation) are
   barely moved by a few extreme points and avoid this.

**Better practice:** flag, investigate, decide by cause. Where a model is
over-sensitive to extremes (OLS, with its squared loss), **winsorise** (cap at
the 1st and 99th percentiles) or use robust regression — and report that you
did. Do not silently delete.

### 5.4 Duplicates

Identify and remove duplicate records; repeated rows reweight observations and
can bias both estimation and backtest fills.

---

## 6. Data preprocessing

Preparing clean data for modelling: normalising or scaling numerical values,
changing data types (binarisation), or altering structure by pivoting.

### 6.1 Data leakage in the scaling step

Consider predicting tomorrow's gold return from 10 years of daily features
(RSI, ATR, lagged returns), with this workflow: **(1)** standardise every
feature using the full-sample mean and standard deviation, **(2)** split
8 years train / 2 years test, **(3)** train and report test accuracy.

Step 1 before step 2 is the error. A training day in 2017 is transformed as

$$
z_{2017}=\frac{x_{2017}-\bar x_{\text{10y}}}{s_{\text{10y}}}
$$

and $\bar x_{\text{10y}},s_{\text{10y}}$ are computed over a window that
**includes the test period**. Test-set statistics have leaked into the
*training inputs*.

**Concretely:** ATR in dollars rises with the gold price. If gold rallied hard
during the test years, the full-sample ATR mean is pulled upward by them, so
the training features were shaped by knowledge of a future price level. The
test score is no longer computed on unseen data, and it cannot be reproduced
live, where future means are unavailable.

**Principle.** The test set stands in for the future. *Anything* computed from
it prior to evaluation — means, variances, imputation values, feature choices —
contaminates the estimate. Scaling is a comparatively mild leak; performing
**feature selection** or **PCA** on the full sample is severe. Selecting "the
five indicators most correlated with returns" using all 10 years can make pure
noise look predictive.

### 6.2 Why a random train/test shuffle is worse

Replacing the chronological split with a random 80/20 shuffle breaks two
things:

1. **Target leakage.** If the target is $y_t=r_{t+1}$ and lagged return is a
   feature, then day $t+1$'s *features* contain $r_{t+1}$, which is day $t$'s
   *target*. Shuffle day $t$ into test and day $t+1$ into train and the model
   has been trained on the answer.
2. **Overlapping features.** A 14-day RSI at $t$ and at $t+1$ share 13 days of
   input, and volatility clusters. Every test day has near-duplicate
   neighbours in training, so the model **interpolates** among seen days
   rather than **forecasting** unseen ones.

This is exactly why $k$-fold cross-validation from
[[07 Model Assessment, Bias-Variance and Resampling]] must be modified for
time series: $k$-fold assumes i.i.d. observations, and financial series are
not.

### 6.3 Correct order of operations

1. Split **chronologically** into train and test.
2. **Fit** $\bar x_{\text{train}}$ and $s_{\text{train}}$ on the training set
   only.
3. **Apply those training parameters** to transform the test set — do *not*
   re-standardise the test set using its own mean and standard deviation.
4. Train, then evaluate **once** on the test set.

**Extension — walk-forward validation.** Train on years 1–8, test on year 9;
retrain on years 1–9, test on year 10; refit the preprocessing at each step.
Leave a **gap** between train and test at least as long as the feature
lookback plus the label horizon — López de Prado's **purging** and **embargo**.
This is also how a cBot backtest should be validated.

---

## 7. Look-ahead bias and point-in-time data

Three cases, and what each needs recorded:

**(i) Signal computed from the day-$t$ close, trade filled at that same
close.** Look-ahead: the closing price is unknown until the close has
happened, at which point it can no longer be traded. Fill at the **next bar's
open** with realistic spread and slippage, or compute the signal minutes
before the close from prices available then.

*cTrader note.* `OnBar()` fires when a *new* bar opens, and
`Bars.ClosePrices.Last(1)` is the bar that just closed — so a cBot reading
`Last(1)` and entering inside `OnBar()` is already filling at the next open.
The bias appears in hand-built backtests (Python, Excel) that permit a fill at
`Close[t]`.

**(ii) A 31 December backtest using Q4 earnings for the quarter ending
31 December, published in late February.** Look-ahead, and one of the largest
sources of fake alpha. Every accounting value needs **two dates**:

| Field | Meaning | Example |
| :--- | :--- | :--- |
| **Period end date** | What period the number describes | 31 Dec 2024 |
| **Report / announcement date** | When the market first learned it | 27 Feb 2025 |

The rule: a value may be used only on or after its **report date**, never its
period end date. Point-in-time databases (Compustat) store the report date for
this purpose. Absent report dates, apply a **conservative lag** — Fama–French
use fiscal-year accounting data only from the following June, an
$\approx6$-month lag, to guarantee it was public.

**(iii) A 2019 macro backtest using 2019 GDP growth as it appears in today's
database.** Also look-ahead, on two counts:

1. **Release lag.** Full-year 2019 GDP did not exist *during* 2019; the first
   estimate arrived weeks after year-end. Same issue as (ii).
2. **Revisions.** The figure in today's database is a *later vintage* nobody
   held in 2019. GDP is published as a first estimate and then revised
   repeatedly — months later, a year later, and in large benchmark revisions
   years later. Kenya's 2014 rebasing raised KNBS's estimate of the size of
   the economy by roughly a quarter overnight.

Revisions are **largest around turning points** — precisely where a macro
signal matters. An early "growth is fine" reading is often later revised into
"a slowdown had already begun". Backtested on revised data the model looks
prescient at calling turns; live, it would have seen only the misleading first
print.

**Fix: vintage data.** Store each value together with the date that version
was published. ALFRED (the St. Louis Fed's archive of FRED) retains every
vintage, allowing a query for "2019 GDP as known on 15 March 2020".

---

## 8. The principle underneath the whole chapter

Every data point carries **two times**:

$$
\underbrace{\text{reference time}}_{\text{what it describes}}
\qquad\text{and}\qquad
\underbrace{\text{knowledge time}}_{\text{when it became available}}
$$

A valid backtest uses only values whose

$$
\text{knowledge time} \;\le\; \text{decision time}
$$

Survivorship bias, look-ahead bias, the missing delisting return and stale data
vintages are all violations of this single rule. Recording knowledge time is
the most important thing **metadata** does — which is why metadata sits at the
very front of the data life cycle rather than at the end.

Once all of this is done, the data is officially ready to be fed into the
algorithms.

---

## 9. Additions from the lecture notes (BSF 3216, §1.1)

> Points in the lecturer's notes not yet covered above, kept in his wording
> where the exam is likely to reuse it.

### 9.1 States vs. activities of the life cycle

The **data life cycle** refers both to the *transformations* applied to data and
to the *states* data passes through as a result.

- **States:** raw data → cleaned data → prepared data → data + results →
  archived data.
- **Activities:** data acquisition → data storage → data cleaning/wrangling →
  data analysis → data archival/preservation.

### 9.2 Definitions as the lecturer gives them

- **Raw data** — data before any processing has been applied. **Raw does not
  mean neutral or unfiltered**: how and what was collected already shaped it
  (the survivorship discussion in §2 is exactly this).
- **Population** — the domain of study.
- **Sample** — the data collected about the domain; always a subset of the
  population, of any size, but **never the population itself** however large.
- **Representative sample** — needed for analysis of the sample to say anything
  about the population; achieved by choosing at random from elements that are
  equally likely to be selected.
- **Metadata** — data about data. Reasons it matters, per the notes:
  1. Only operations that make sense for the data should be performed — it
     makes no sense to add a temperature and a height, though both are numbers.
  2. It guides decisions on how to clean and preprocess.
  3. If analysis shows a transformation was inappropriate, a record of the
     change makes it easy to undo and try something else.
  4. Shared or exported data must be explained for others to use it.
  5. It traces how the data came to be, which is what makes analysis
     repeatable and results reproducible.
- **Exploratory Data Analysis (EDA)**, also called **data profiling** — the
  first step: classify the dataset, get an idea of the attributes and, for
  each, its distribution (visualisation, histograms, measures of centrality and
  dispersion); validate the dataset against any metadata and augment it.

### 9.3 Sources of errors in datasets

1. Manual (unreliable) data entry.
2. Changes in layout (for records).
3. Variations in measurement, scale or format (for values).
4. Changes in how default or missing values are marked.
5. Outdated values — called **gaps** in time series.

### 9.4 Data cleaning — explicit vs. implicit missing values

Data cleaning is also called data cleansing, data wrangling or data munging.

- **Explicitly** missing values carry a marker such as `NULL` or `NA`.
- **Implicitly** missing values are marked by the *absence* of a value (e.g. a
  date simply absent from a daily series).

They can be handled by deleting incomplete records or by **imputing** a value
from other related values — with the mechanism caveats of §5.1.

**Duplicate data** — two pieces of the dataset referring to the same
real-world item (entity, fact, event or observation); removed because it can
bias the analysis.

### 9.5 The three kinds of preprocessing transformation

1. **Format / frame-of-reference** transformations: normalisation, scaling,
   standardisation.
2. **Type** transformations, changing a value from one type to another:
   **discretisation** (turning a continuous value into bins, e.g. returns into
   "large down / small / large up") and **binarisation** (into 0/1, e.g.
   Up/Down).
3. **Structural** transformations: pivoting, (de)normalisation.
