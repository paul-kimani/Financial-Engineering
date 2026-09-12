# 04 — Systematic Trader Interview Prep

> What the interview format looks like, what each section tests, and how to prepare for it.

---

## 1. Typical interview structure

ST interviews differ from QD and QR interviews in their emphasis on judgement and P&L intuition alongside quantitative skill. The format varies by firm type:

**Prop trading firms (SIG, Optiver, IMC, DRW):**
- **Online assessment:** Mental maths, probability, and game theory problems under time pressure. Optiver's "80 in 8" mental arithmetic test is famous — 80 multiplication/division problems in 8 minutes. SIG's test includes game theory and expected value puzzles.
- **First round:** Probability puzzles + markets discussion + "walk me through your strategy/trade idea"
- **Final round:** In-depth strategy discussion, risk/sizing questions, behavioural + culture

**Pod-based multi-strategy hedge funds (Millennium, Point72):**
- More QR-flavoured — deeper on statistics and factor models
- Strategy presentation of your own work is central
- Risk management and "when do you stop a strategy" questions

**Smaller systematic funds / family offices:**
- More conversational, less structured testing
- Strong focus on your actual track record and reasoning behind it

---

## 2. Mental maths and probability

For prop trading firm interviews specifically, mental maths is a real filter — not because the work requires arithmetic speed, but because it tests quantitative confidence under pressure.

**Mental maths practice:**
- 2-digit × 2-digit multiplication fluently (12 × 47, 35 × 28, etc.)
- Percentage calculations (what's 17% of 340?)
- Fraction / decimal conversions
- Powers and roots (√144, 2^10, 1.05^4 approximately)

Practice daily for 10 minutes using mental maths apps or the Optiver-style tests that circulate online. Speed matters as much as accuracy — you need to be able to do these without slowing your other reasoning.

**Probability puzzles (same as QR but slightly lighter depth):**
- Expected value in a game: "A fair die, you get £N for rolling N. What is it worth to play? What if you can re-roll once?"
- Conditional probability: "I have two children, at least one is a girl. Probability both are girls?"
- Gambler's ruin: "You have £10, opponent has £10, you flip a fair coin for £1 each flip. Probability you go broke first?"
- Martingale questions: "A fair random walk — what is the expected time to hit ±5?"

---

## 3. Strategy and markets discussion

This is the round where your existing work matters most. Have a strategy you can walk through at depth:

**What they want to see:**
- You understand *why* the strategy should work (economic rationale, not just "it fit the backtest")
- You tested it properly — walk-forward, realistic costs, not overfitted
- You know what the risks are — what market conditions would hurt it?
- You have thought about sizing — how much would you trade, and why?
- You can articulate when you'd stop the strategy — the answer "when it hits my max drawdown limit" is necessary but not sufficient; they want to know how you distinguish "normal drawdown" from "model is broken"

**Prepare:** A 10-minute walk-through of one of your bots — Gold Pulse or Squeeze Pulse Breakout — that covers: motivation, signal construction, backtest methodology and results, real out-of-sample performance if available, risk management, and what you'd change with more time.

**Markets discussion:**
- Current macro: know the broad drivers of XAUUSD (USD strength, real rates, risk sentiment) and why the pair you trade moves
- "Where would you trade right now and why?" — have a view and a structured argument for it; they don't care if it's right, they care that you can reason about markets

---

## 4. Risk and sizing questions

These are the questions that separate traders from people who have read about trading:

- "Your strategy is down 15% from peak. What do you do?" (There's no single right answer — the right answer is that it depends on whether the drawdown is consistent with the strategy's expected distribution. Know your strategy's historical max drawdown and expected recovery time.)
- "How do you size your positions?" (Have a real answer: Kelly fraction, your risk-per-trade logic, why you chose it. "2% per trade" is a start but explain why 2%.)
- "If two of your strategies suddenly became highly correlated, what would you do?" (Reduce size on one or both — explain why position-level sizing ignores portfolio correlation at your peril.)
- "What's the difference between a strategy that's in a normal drawdown and one that's broken?" (This is the hard question. The honest answer involves comparing realised drawdown to the backtest distribution, checking whether market structure has changed, and checking whether execution has degraded — not just watching the P&L and hoping.)

---

## 5. Preparation timeline

```
Now → 3 months:   Mental maths (10 min/day)
                   Green Book probability puzzles (3/week — lighter than QR track)
                   S1 + S2 (data pipeline + tearsheet) built
                   S3 (Kelly sizing) implemented
                   Prepare the Gold Pulse walkthrough (10 minutes, practice out loud)

3–6 months:        S4 (walk-forward backtest of Gold Pulse) — the "rigorous version of my bot" story
                   Markets reading (FT, macro blogs, know the current XAUUSD setup)
                   Mock strategy presentation with a peer or in a mirror

6 months → grad:   S5 (multi-strategy portfolio)
                   IMC Prosperity competition entry if timing aligns
                   S6 (pairs trading) as additional interview talking point
                   Begin applying: prop firm internship programmes open 9–12 months ahead

Applications:      Top prop firms (SIG, Optiver, IMC) run internship / graduate programmes
                   with applications typically opening Sep–Nov for the following summer.
                   Apply in first round — these fill early.
```
