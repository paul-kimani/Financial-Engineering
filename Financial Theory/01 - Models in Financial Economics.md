# 01 — Models in Financial Economics

> Quant models split into two camps: **economic** (built from first
> principles about why people behave the way they do) and **statistical**
> (built from data, agnostic about the underlying behaviour). This
> chapter sets out both, illustrates with CAPM and Black–Scholes, and
> closes with the *modeler's manifesto* — a short list of warnings
> about treating mathematical convenience as physical truth.

---

## 1. The two pillars of financial modelling

### A. Economic models — the *why*

Economic models are simplified versions of reality built from
**economic theory** upwards.

- **Core assumption.** They start with a fundamental belief about human
  behaviour. In finance the bedrock assumption is usually that
  investors **maximise expected utility** — they want the most reward
  for the least risk. See chapter
  [[09 - Expected Utility Theory]] for the formal version.
- **Purpose.** Generate equations describing *why* investors behave as
  they do and how markets *should* operate.

**Example — the CAPM.** The Capital Asset Pricing Model is the classic
economic model:

$$
E(R_i) = R_f + \beta_i \left( E(R_m) - R_f \right).
$$

It is founded entirely on **mean–variance portfolio theory** — investors
care only about mean (expected return) and variance (risk) and act
rationally to maximise utility given those two metrics.

### B. Statistical models — the *how*

Statistical models ignore economic theory and focus entirely on
**data**.

- **Calibration.** Parameters are chosen to fit historical data as
  closely as possible.
- **Pros and cons.** Because they are fitted to actual data they often
  feel more realistic and describe *how* a phenomenon occurs. They do
  not explain *why*. If the historical data is anomalous or
  unrepresentative, the model breaks.

**Example — Black–Scholes.** The Black–Scholes formula for a European
call option is a statistical model:

$$
C = N(d_1)\,S_t - N(d_2)\,K\,e^{-r(T-t)}.
$$

It assumes the underlying follows
[[../Stochastics/10 - Geometric Brownian Motion|Geometric Brownian Motion]] — a
specific random walk. That assumption is not derived from human
behaviour; it is chosen because it mathematically matches observed
returns reasonably well.

### C. Time-series models — the hybrid

Time-series models analyse data collected over time (e.g. daily stock
prices). Depending on how the formula is constrained, they can lean
economic or statistical:

- **As an economic model** — parameters are restricted so they do not
  violate economic theory.
- **As a statistical model** — the formula (e.g.
  $r_t = r_{t-1} + \delta^2 \, r_{t-2}$) relies purely on best fit to
  past data.

See [[../Time Series/10 April 2026]] for the broader time-series
vocabulary and [[../Time Series/24 April Assignment]] for the OLS fit
machinery used in practice.

---

## 2. What makes a "good" model?

A successful financial model balances three criteria.

1. **Data fit.** It provides a reasonable fit to historical data — we
   are confident it describes the actual process.
2. **Theoretical consistency.** It aligns with economic theory so we
   understand *why* the market behaves that way.
3. **Parsimony.** It is reasonably simple. A *parsimonious* model
   achieves its goal with the fewest and simplest assumptions possible.
   Overly complex models break easily.

The art is hitting all three. Most failures relax at least one.

---

## 3. The financial modeler's manifesto

Finance reduces complex, chaotic markets to "universal laws" and often
reduces risk to a single variable (volatility). That ignores the
reality that markets are full of irrational, biased people.

**The modeler's oath — key takeaways**

- *Models are not reality.* The world doesn't perfectly satisfy
  mathematical equations.
- *Elegance vs reality.* Never sacrifice reality just to make the math
  look elegant or simple.
- *Transparency.* Never give false comfort about a model's accuracy.
  State your assumptions and their limitations explicitly.

The 2008 Lehman collapse, summarised in
[[../Derivatives/Lehman Brothers 2007]], is a textbook example of what
happens when these warnings are ignored.

---

## Cross‑references

- [[00 - Introduction]] — the research cycle that produces and tests
  these models.
- [[09 - Expected Utility Theory]] — the bedrock assumption behind
  economic models in finance.
- [[13 - Efficient Market Hypothesis]] — the "market efficiency"
  condition that determines whether a predictive model can work at all.
- [[../Stochastics/05 - Diffusion Processes Catalogue]] — the GBM
  underlying Black–Scholes.
- [[../Stochastics/10 - Geometric Brownian Motion]] — full derivation
  of GBM.
- [[../Time Series/24 April Assignment]] — OLS regression, the standard
  fit method for statistical models like CAPM.
- [[../Derivatives/7 May 2026 - Personal Notes 1]] — options pricing is
  the direct application of the Black–Scholes statistical model.
- [[../Derivatives/Lehman Brothers 2007]] — practical-phase failure
  mode for over-trusted models.
