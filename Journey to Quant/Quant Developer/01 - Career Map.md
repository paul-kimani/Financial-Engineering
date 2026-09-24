# 01 — Quant Developer Career Map

> What the role actually looks like at each stage of a career, who hires, and what the progression is.

---

## 1. What a QD does day-to-day

The work divides roughly into four types depending on the firm and seniority:

**Pricing and risk libraries.** Implementing derivatives pricing models (Black-Scholes, local vol, SABR) in code fast enough to reprice a portfolio in real time. The maths is given by a researcher; the QD makes it production-grade.

**Execution infrastructure.** Order management systems, execution algorithms (TWAP, VWAP, smart order routing), FIX protocol handling, connectivity to exchanges. At HFT firms this is the whole job — every microsecond counts.

**Data pipelines.** Ingesting, cleaning, normalising, and storing market data at scale. Getting tick data into a database correctly, without gaps or look-ahead, is harder than it sounds and breaks a lot of research if done wrong.

**Strategy platform.** The framework that researchers use to write and backtest strategies — the engine, not the strategies themselves. At Two Sigma or D.E. Shaw, QDs build Jupyter-accessible research platforms; at an HFT firm, QDs build the low-latency simulation engine.

---

## 2. Firm types and what each looks like

**HFT / Market making** (Jane Street, Citadel Securities, IMC, Jump, Optiver): Engineering excellence is the primary asset. C++ is the language of production. Interviews are the hardest anywhere — competitive programming intensity plus systems design. Compensation is highest.

**Quantitative hedge funds** (Two Sigma, D.E. Shaw, Renaissance, Man AHL): Mix of Python research infrastructure and C++ production. More collaborative with researchers. Slightly softer on the competitive programming, harder on systems design and ML infrastructure.

**Investment banks (quant strats / securities technology)** (Goldman Sachs Strats, JP Morgan, Morgan Stanley): XVA, risk, pricing library work. More Python/Java, less C++. More structured, more process, lower comp ceiling than top HFT/hedge funds. More accessible for first roles.

**Fintech / Asset management technology:** Lower barrier to entry, good for building transferable skills, lower ceiling. Can be a stepping stone.

---

## 3. Career progression

```
Year 0–2 (junior):     Writing and maintaining production code with oversight.
                        Learning the codebase, fixing bugs, owning one module.

Year 2–5 (mid-level):  Owning systems end-to-end. Designing new components.
                        Mentoring juniors. Starting to have opinions about architecture.

Year 5–10 (senior):    Technical lead on a system or track. 
                        Deciding between architectural approaches.
                        Often managing one or two junior devs.

Year 10+ (principal / VP / partner):
                        Setting technical direction. Owning a P&L line indirectly
                        (the systems your team builds generate it).
```

At HFT firms the progression is faster but the pool is smaller. At banks it's more structured and slower. At hedge funds it's somewhere between.

---

## 4. Key skills by firm type

| Skill | HFT | Quant hedge fund | Bank |
|---|---|---|---|
| C++ (modern, performant) | ★★★★★ | ★★★ | ★★ |
| Python (research-quality) | ★★ | ★★★★★ | ★★★★ |
| Algorithms / data structures | ★★★★★ | ★★★★ | ★★★ |
| Systems programming (OS, networking, concurrency) | ★★★★★ | ★★★ | ★★ |
| ML infrastructure | ★★ | ★★★★★ | ★★★ |
| Financial knowledge | ★★ | ★★★ | ★★★★ |
| SQL / data engineering | ★★★ | ★★★★ | ★★★ |
