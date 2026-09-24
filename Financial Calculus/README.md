# Financial Calculus

> The pricing of derivatives, developed twice over: first discretely
> (the binomial option pricing model, its no-arbitrage condition, and the
> Fundamental Theorem of Asset Pricing), then continuously (the
> Black-Scholes PDE by three independent routes, and the Feynman-Kac
> theorem that turns it into a closed-form price). The two halves meet at
> risk-neutral pricing: discount the expected payoff under an artificial
> probability measure, never the real one.

---

## How to read these notes

Chapters are numbered in study order. Each is a self-contained reference
file. [[Working Notes]] is a single running scratch file for active-recall
writing — not split per chapter.

| #  | Chapter                                                                                                   | Status  |
| -: | :---------------------------------------------------------------------------------------------------------| :------ |
| 01 | [[01 - The One-Period BOPM]] — replicating portfolio, no-arbitrage, risk-neutral probabilities             | Core    |
| 02 | [[02 - The Two-Period and n-Period BOPM]] — dynamic hedging, backward induction, the general formula       | Core    |
| 03 | [[03 - FTAP, Risk-Neutral Measure and Martingales]] — no-arbitrage ⟺ martingale measure; completeness      | Core    |
| 04 | [[04 - The Black-Scholes PDE (Hedging, Replication, CAPM)]] — the same PDE, derived three ways             | Core    |
| 05 | [[05 - The PDE in Log-Price]] — the change of variable $x=\ln S$                                           | Core    |
| 06 | [[06 - Feynman-Kac and the BSM Formula]] — PDE ⟺ expectation; the closed-form call price                   | Core    |

Source material: [[00 - Lecture Notes Transcript]] is a faithful,
page-by-page transcription of the handwritten notebook (`Photos/`,
IMG_3619–IMG_3650, plus the earlier `IMG_2768.heic`), with every slip in
the original marked `[sic]` rather than silently corrected. The six
chapters above are the cleaned-up, consolidated version — errors fixed,
gaps filled, and cross-referenced — and are what to study from; the
transcript is the record of what the notebook actually says, for whenever
that distinction matters.

## Notational conventions

- $u, d$ — the up/down multipliers per period, $0<d<1<u$.
- $r$ — the one-period risk-free rate (discrete chapters) or the
  continuously-compounded risk-free rate (continuous chapters).
- $\tilde p, \tilde q$ (equivalently $\mathbb Q$) — the risk-neutral
  probabilities/measure; $\tilde p+\tilde q=1$.
- $\Delta$ — the hedge ratio, $\partial V/\partial S$ in the continuous
  setting.
- $\tau = T-t$ — time to maturity, used throughout Chapter 6.
- Matches [[../Stochastics/README|Stochastics]]'s conventions: $W_t$ is
  standard Brownian motion, $\mathcal F_t$ its natural filtration, all
  $dW_t$ integrals are Itô.

## Related folders

- [[../Stochastics/27 - Stochastic Models of Derivative Prices]] — BOPM vs
  BSM assumptions, from the Stochastics course's own angle.
- [[../Stochastics/28 - Derivation of the Black Scholes PDE]] and
  [[../Stochastics/29 - Derivation of The Black Scholes merton formula]] —
  a second, independent derivation of Chapters 4 and 6 here; useful for
  cross-checking.
- [[../Derivatives/]] — options terminology and payoff diagrams assumed
  known from Chapter 1 onward.
