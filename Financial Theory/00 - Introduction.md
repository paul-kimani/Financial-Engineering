# 00 — Introduction

> Financial economics sits at the intersection of finance and economics.
> This chapter sets out the domain, the research cycle that produces
> the theory, and the **three pillars** — *time, money, uncertainty* —
> that every problem in the rest of the folder ultimately reduces to.

---

## 1. The domain of financial economics

**Financial economics** is the field that analyses the relationship
between financial variables — asset prices, interest rates, share
values — using the tools of economic theory.

- **Economics** studies how people allocate **scarce resources** among
  competing objectives.
- **Finance** is a branch of economics that specialises in how people
  choose between **uncertain future values**.

### The three pillars of financial decisions

Every problem in finance trades off along three axes:

1. **Time** — when do you invest, and how long do you wait for a return?
   (e.g. tying up money for ten years in a retirement fund).
2. **Money** — how much capital is being allocated? (e.g. investing
   $\$10{,}000$).
3. **Uncertainty (risk)** — how confident are you in the future
   outcome? (e.g. the chance the stock drops after you buy it).

The rest of Part I removes uncertainty and works in (time, money). Part
II ([[09 - Expected Utility Theory]] onward) adds uncertainty back in.
Part III ([[13 - Efficient Market Hypothesis]] onward) asks whether
market prices fully digest all available information.

---

## 2. The research cycle

Financial economics is built on a feedback loop between theory and
practice. The same loop recurs throughout the vault.

- **Theoretical phase**
    - *Assumptions* — foundational beliefs about the market (e.g.
      "investors are rational"; see chapter [[03 - Axioms of Choice under Certainty]]).
    - *Model* — the mathematical framework built on those assumptions.
    - *Predictions* — what the model says *should* happen in the real
      world.
- **Empirical phase**
    - *Prices / hypotheses* — observed market data, or testable claims
      from the model.
    - *Tests* — statistical analyses checking whether the predictions
      hold. The EMH tests in chapter [[14 - Tests of the EMH]] are the
      flagship examples.
    - *Results* — validation or challenge of the theory.
- **Practical phase**
    - *Applications* — using the validated model (e.g. a hedge fund
      pricing trades against it).
    - *Conclusions / questions* — observations that feed back into the
      theoretical phase to refine assumptions and restart the loop.

The Lehman Brothers collapse, summarised in
[[../Derivatives/Lehman Brothers 2007]], is a particularly sharp example
of the practical phase generating questions that re-enter theory.

---

## 3. Modelling financial markets

A **model** is a mathematical representation of an economic process.
In finance the purpose is usually to **predict security prices and
returns**.

Two conditions govern how well a model behaves:

- **Market efficiency.** An *efficient* market is one whose current
  prices reflect all available information. If a breakthrough is
  announced, an efficient market instantly re-prices the stock; an
  inefficient market takes time, and predictive models built on price
  history will go stale. Treated formally in chapter
  [[13 - Efficient Market Hypothesis]].
- **Market equilibrium.** Models often assume balance. In disequilibrium
  (panic selling, bubbles, liquidity crises) a model's parameters can
  drift far from their "normal" values.

The distinction between economic models (built from first principles)
and statistical models (fit to data) is the subject of the next chapter.

---

## Cross‑references

- [[01 - Models in Financial Economics]] — economic vs statistical models.
- [[02 - Consumption and Savings Decision]] — the simplest concrete
  problem the framework addresses.
- [[13 - Efficient Market Hypothesis]] — the market-efficiency condition treated rigorously.
- [[../Stochastics/00 - Foundations Review]] — the math toolkit for the "uncertainty" pillar.
- [[../Derivatives/Lehman Brothers 2007]] — practical-phase failure mode.
- [[../Time Series/10 April 2026]] — the empirical / statistical phase of the research cycle in practice.
