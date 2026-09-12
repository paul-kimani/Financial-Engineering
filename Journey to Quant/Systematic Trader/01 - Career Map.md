# 01 — Systematic Trader Career Map

> What the role looks like at each stage, who hires, and where this path goes.

---

## 1. What a systematic trader does day-to-day

**Strategy development:** Generating trade ideas, formalising them as testable rules, backtesting them with correct methodology, and deciding which ones are real. This is the research component of the ST role — overlapping heavily with QR but with more direct ownership of the P&L outcome.

**Live portfolio management:** Running strategies in production. Monitoring for drawdown breaches, regime shifts, execution degradation, and data issues. Knowing when to turn a strategy down (reduce size) or off (halt it) is a core skill — most traders underweight this.

**Risk management:** Position sizing, correlation management across strategies, portfolio-level drawdown limits. The Kelly criterion gives an upper bound on sizing; most professional traders use a fraction of Kelly (half-Kelly is common) to account for parameter uncertainty. Daily P&L attribution by strategy, by factor, by sector.

**Execution optimisation:** Reducing slippage, minimising market impact, improving fill rates. At the retail/small prop level this is secondary; at scale it can eat a large fraction of the edge.

**Idea sourcing:** Reading papers, watching market structure, talking to other traders. Alpha decays — strategies that worked 3 years ago often don't work now. The pipeline of new ideas has to stay full.

---

## 2. Firm types and what each looks like

**Prop trading firms** (SIG, Optiver, IMC, Tower Research, DRW, Flow Traders): Own-capital trading, no client money. Traders often own the full lifecycle from idea to live. Compensation is typically salary + profit share or partnership. Culture ranges from collaborative (SIG's model) to competitive (many smaller shops). Interview processes emphasise P&L intuition, mental maths, and game theory as much as statistics.

**Pod-based multi-strategy hedge funds** (Millennium, Citadel, Balyasny, Point72): Capital allocated to individual pods, each with a PM and small team. More resources (data, infrastructure) but more scrutiny — pods that don't perform get cut. More quant-dev and QR support available.

**Smaller systematic funds / family offices:** Less prestige but more autonomy and faster learning cycles. You own more of the decisions earlier. Good for building a track record.

**Starting your own:** Not a day-1 option, but a realistic long-term path for STs who build a verifiable track record. The cTrader bots you're running right now are the beginning of that track record.

---

## 3. Career progression

```
Year 0–2 (junior trader / associate):  
    Supporting a senior trader or PM. Running existing strategies, monitoring risk.
    Developing and pitching your own ideas, most of which won't get funded yet.

Year 2–5 (trader):  
    Running your own book, even if small. Full ownership of a strategy or a few strategies.
    Building the intuition for when a drawdown is "normal" vs. when the model is broken.

Year 5–10 (senior trader / PM):  
    Managing a larger book or a team of junior traders.
    Capital allocation decisions. P&L responsibility at the portfolio level.

Year 10+ (PM / principal / founder):  
    Portfolio management at scale, or founding your own fund.
```

The ST path has the shortest ramp from "useful" to "autonomous" — if you can generate real P&L, the market tells you immediately. It also has the fastest career progression of the three paths for those who perform.

---

## 4. Key skills by firm type

| Skill | Prop firm | Pod HF | Small systematic fund |
|---|---|---|---|
| Strategy development + backtesting | ★★★★★ | ★★★★ | ★★★★★ |
| Risk management and position sizing | ★★★★★ | ★★★★★ | ★★★★ |
| Mental maths + game theory | ★★★★★ | ★★★ | ★★ |
| Programming (Python / cTrader) | ★★★ | ★★★★ | ★★★★ |
| Quant/stats depth | ★★★ | ★★★★ | ★★★ |
| Markets knowledge (macro, micro) | ★★★★ | ★★★★ | ★★★ |
| Execution / market microstructure | ★★★★★ | ★★★★ | ★★★ |

---

## 5. What this path means given your current work

Your Gold Pulse bot, Squeeze Pulse Breakout, and Trend Pullback Bot are real work — you're already doing the core ST activity. The upgrade path is:

1. **Methodological rigour:** Run your existing bots through a proper walk-forward backtest (see ST Projects chapter and QR R2) — not because you distrust the strategy, but because you need to know what the real out-of-sample edge is before sizing up.

2. **Risk infrastructure:** Build proper drawdown tracking, per-strategy P&L attribution, and sizing logic (the Kelly criterion note in QUANTFRAME is a start — see ST Projects S3).

3. **Portfolio thinking:** Eventually manage multiple strategies as a portfolio, with correlation management between them — this is the PM-level skill you're building toward.
