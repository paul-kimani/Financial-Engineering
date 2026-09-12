# 00 — Quant Landscape & Roles

> What each path actually looks like in practice — the day-to-day work, the hiring landscape, and the career arc — before you commit to a direction.

---

## 1. The three roles in plain terms

**Quant Developer (QD / Quant Dev)**
Builds the *infrastructure* that lets strategies run: execution engines, data pipelines, risk systems, pricing libraries, order management systems. The distinguishing trait is engineering excellence — low-latency C++, clean abstractions, software that doesn't fail when the market moves. Maths is necessary but secondary to code quality and systems thinking. At a top HFT firm (Jane Street, Citadel Securities, IMC, Jump) the quant dev *is* the product.

**Quant Researcher (QR)**
Generates *alpha* — discovers new predictive signals, builds and validates factor models, prices exotic derivatives, or develops risk models. Work is closer to applied academic research: forming hypotheses, testing them rigorously against data, writing up findings, and handing clean implementations to a dev. The distinguishing trait is mathematical depth and statistical rigour — results must survive peer scrutiny, not just a backtest. Employers include hedge funds (AQR, Two Sigma, D.E. Shaw, Man Group), large asset managers, and central bank/regulator research teams.

**Systematic Trader (ST)**
Designs and manages *trading strategies* as a portfolio — sizing, hedging, drawdown control, and knowing when to turn a strategy off. The distinguishing trait is P&L intuition and risk discipline: a trader has real capital at stake and has to be right in a different way than a researcher. At prop firms (SIG, DRW, Tower Research, Optiver) traders often have hybrid QD/QR skill and own the full lifecycle from idea to live. This is the closest path to what your current cTrader bots are building toward.

---

## 2. Commonalities and divergence

All three roles share the same mathematical bedrock — linear algebra, probability, statistics, optimisation, stochastic calculus in varying doses. The divergence is in *depth and emphasis*:

```
Math depth:        Researcher > Developer ≈ Trader
CS/engineering:    Developer >> Trader > Researcher
Markets intuition: Trader > Researcher > Developer
Risk focus:        Trader > Researcher >> Developer
```

The reading list's Tier 1–3 (math, programming, ML) is shared across all three. The fork happens at Tier 4 onward — stochastic calculus and pricing theory matter far more to a researcher than a developer, while CS fundamentals (CLRS, systems programming) matter far more to a developer.

---

## 3. Where each role exists in Africa and globally

**Nairobi specifically:** The local institutional market (NSE, CDSC, major Kenyan banks) barely employs quants in the Western sense — the closest roles are risk analysts, treasury quants, and data scientists at equity funds. East Africa is 5–10 years from having a prop trading ecosystem. This is *not* a reason to deprioritise quant skills — it's a reason to target remote-first firms or to plan for relocation.

**Remote opportunities (realistic from Nairobi):** Jane Street, Two Sigma, AQR, DRW, and others hire globally and have made remote offers in recent years, though interview pipelines remain competitive and visa/timezone logistics matter. Man Group (London), Citadel (Chicago/London/Hong Kong), and D.E. Shaw have offices across time zones.

**Realistic arc from your position:** Build the technical track record (GitHub, competition results, published strategies) during Year 3 and Year 4, target internships aggressively in Year 4 or the summer after graduation, and plan a relocation budget if a role requires it. The alternative — staying Nairobi and building tools for local institutional clients — is also a real path but is closer to fintech than quant finance.

---

## 4. What each role actually tests in interviews

### Quant Developer
- **Competitive programming:** LeetCode Medium/Hard (arrays, trees, graphs, dynamic programming, system design). Top firms run 2–4 coding rounds.
- **CS fundamentals:** OS concepts (memory, concurrency, cache), networking basics, complexity analysis.
- **Low-latency systems:** How do you minimise latency? What's false sharing? When do you use a ring buffer?
- **Probability puzzles:** Coin flips, expected values, dice problems — the "Green Book" (*A Practical Guide to Quantitative Finance Interviews*) covers the genre.
- **Finance (light):** Enough to understand what the system you're building does, not to price it.

### Quant Researcher
- **Math/stats derivations:** Derive the MLE for a Gaussian, derive Black-Scholes PDE from Itô's lemma, prove a basic convergence result — live, on a whiteboard.
- **Probability brainteasers:** Deeper than dev interviews — conditional expectation, martingales, random walk properties.
- **ML conceptual depth:** Assumptions behind each model, bias-variance, what breaks when assumptions fail.
- **Research presentation:** Walk through a paper you've read or a piece of original work. Can you defend it under questioning?
- **Finance:** Derivatives pricing, factor model structure, what drives a P&L attribution.

### Systematic Trader
- **Strategy intuition:** Describe your edge. Why does it persist? What erodes it?
- **Backtesting methodology:** How do you prevent overfitting? Walk-forward vs. in-sample. When do you trust a backtest?
- **Risk and sizing:** Kelly criterion, max drawdown limits, what happens when your strategy stops working — do you turn it off or average down?
- **Markets:** Macro awareness, understanding of the asset class you'd trade.
- **Coding:** Python-fluent, but rarely competitive-programming intensity.

---

## 5. Which path is right for you (honest assessment)

Your current profile: cTrader bots (systematic trader instinct), Financial Engineering coursework (researcher grounding), stated interest in AI/ML and interpretability (researcher + developer overlap).

The honest answer is that you're already *doing* systematic trading — the bots are real work. The question is whether you want to formalise that into a career or pivot toward the deeper maths (researcher) or the engineering rigour (developer). None of the paths require abandoning what you're already building.

**If the bots excite you more than the equations:** Systematic Trader, with a QD secondary.
**If deriving results and reading papers excites you more than running live P&L:** Quant Researcher, with an ST secondary.
**If building clean, fast, reliable systems excites you more than either:** Quant Developer.

All three are in this vault. Run them in parallel until one clearly wins.
