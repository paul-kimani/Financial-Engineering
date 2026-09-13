# 06 — Quant Developer Reading List

> A complete, tiered book curriculum for the QD path — from mathematical foundations through high-performance systems. Every book is annotated with its level and *why it belongs on this list specifically for a QD*, not just "it's a good book." Read in tier order; within a tier, books can be tackled in parallel.

This list is the QD-specific companion to the [[../../Quant Roadmap - Reading List|root reading list]]. Shared books appear here with a QD-specific annotation explaining *what to focus on* rather than the general rationale.

**Levels:** [Beginner] · [Core] · [Advanced]

---

## Tier 1 — Mathematical Foundations

The QD needs working fluency, not research-level depth. Focus on computational intuition — eigen-decomposition as a tool, not a proof object.

| Book | Level | Why it's here for a QD |
|---|---|---|
| *Mathematics for Machine Learning* — Deisenroth, Faisal, Ong | Core | The best single-volume bridge: linear algebra, calculus, and probability each taught with the ML payoff visible throughout. Free PDF. Read this *before* Axler — it contextualises the machinery before you study it rigorously. |
| *Linear Algebra Done Right* — Sheldon Axler | Core | Eigenstructure and matrix decompositions are everywhere in QD work — covariance matrices in risk systems, PCA in factor models, Cholesky decomposition in Monte Carlo. Axler gives you the conceptual precision to use them without cargo-culting. |
| *Introduction to Linear Algebra* — Gilbert Strang | Beginner alt. | Strang if you need the computational approach first (matrix multiplication, elimination, factorisation before abstraction). His free MIT OCW lectures pair with it. |
| *Introduction to Probability* — Blitzstein & Hwang | Core | Distributions, conditioning, expectation, generating functions — the probability you need to implement pricing models correctly and to pass the maths round in interviews. Builds intuition rather than just machinery. |
| *Convex Optimization* — Boyd & Vandenberghe | Advanced | The LP duality and QP material is directly relevant: portfolio optimisation is a QP, transaction cost minimisation is a constrained LP, and many risk constraints are convex. Free PDF. Focus on Chapters 1–5 and 11. |

---

## Tier 2 — Programming Core

The QD's primary craft. Python first (it's what you'll prototype and research in); C++ second (it's what production runs in at any firm that cares about latency).

| Book | Level | Why it's here for a QD |
|---|---|---|
| *Python for Data Analysis* — Wes McKinney | Beginner | Written by pandas' creator. Gets you from zero to a working data-analysis and backtesting toolkit. If you're already here, skim the advanced pandas chapter (groupby, time series resampling) — QDs use these daily. |
| *Fluent Python* — Luciano Ramalho | Core | The gap between "my script runs" and "my code would survive a code review." Covers: data model, generators and iterators, decorators, concurrency primitives, and memory model. Read this before any serious Python project — it will change how you write the language. |
| *A Tour of C++* — Bjarne Stroustrup | Core | The shortest credible introduction to modern C++ (C++17/20) from the language's creator. ~200 pages. Read this first — it gives you the mental model before the details. |
| *Effective Modern C++* — Scott Meyers | Core | The 42 items that separate C++ that *runs* from C++ that *runs fast and safely*. Move semantics, `auto`, `constexpr`, smart pointers, concurrency — the idioms production QD code actually uses. Read after Stroustrup's Tour. |
| *C++ Primer* — Lippman, Lajoie, Moo | Beginner alt. | If Stroustrup's Tour is too dense as a first pass, Primer is more gradual. Comprehensive but long — use as a reference once Stroustrup is done, not as a first read. |
| *Introduction to Algorithms* (CLRS) — Cormen, Leiserson, Rivest, Stein | Advanced | The canonical algorithms text. Essential if targeting HFT or any firm that runs competitive-programming-style interviews. Focus on: sorting (Ch. 6–8), data structures (Ch. 10–14), dynamic programming (Ch. 15), graph algorithms (Ch. 22–26). Don't try to read cover-to-cover — use LeetCode to drive which chapters you need next. |

---

## Tier 3 — Systems Programming

The layer below "write code that works" — write code that's fast, correct under concurrency, and doesn't break in production.

| Book | Level | Why it's here for a QD |
|---|---|---|
| *Computer Systems: A Programmer's Perspective* (CSAPP) — Bryant & O'Hallaron | Core | The single best book on what actually happens when your code runs: the memory hierarchy, caching, virtual memory, linking, I/O, network programming, and concurrency — at the hardware/OS level. Explains *why* false sharing kills performance, why cache misses hurt more than arithmetic, and how to think about throughput vs. latency. Chapters 5–6 (optimising performance, the memory hierarchy) are mandatory for any QD. |
| *Operating Systems: Three Easy Pieces* — Arpaci-Dusseau & Arpaci-Dusseau | Core | Processes, threads, locks, condition variables, semaphores, file systems — the OS concepts that come up in systems design interviews and that explain why your multi-threaded code sometimes misbehaves. Free online. Lighter than CSAPP, fully sufficient for interview prep. |
| *C++ Concurrency in Action* — Anthony Williams | Advanced | Threads, mutexes, atomics, lock-free data structures, the C++ memory model — the concurrency machinery you need for any production QD work. The chapter on the C++ memory model (acquire/release semantics, happens-before) is required reading before you touch lock-free code. |
| *Designing Data-Intensive Applications* — Martin Kleppmann | Advanced | Distributed systems, databases, replication, consistency, stream processing — the architecture layer that QDs who build research platforms and data pipelines need to understand. The go-to book for systems design interview prep. Not finance-specific but directly applicable: you *are* building data-intensive applications. |
| *High-Performance Python* — Gorelick & Ozsvald | Core | Python is slow; *fast Python* is about knowing where the slowness is and fixing it. Profiling tools (cProfile, line_profiler), NumPy vectorisation, Cython, Numba, and when to call C from Python — the toolkit for the moment before you'd reach for C++. |

---

## Tier 4 — Numerical Methods and Scientific Computing

Pricing libraries, simulation engines, and optimisers all rely on numerical methods. You don't need to derive them from scratch, but you need to know which method to use, when it breaks, and what the error looks like.

| Book | Level | Why it's here for a QD |
|---|---|---|
| *Numerical Recipes: The Art of Scientific Computing* — Press, Teukolsky, Vetterling, Flannery | Core | The working engineer's guide to numerical methods: interpolation, root-finding, numerical integration, ODEs, FFTs, random number generation, Monte Carlo. Not a maths text — it tells you *how to implement it* and *what can go wrong*. The Monte Carlo and random number chapters are directly relevant to options pricing simulation (P3 in the Projects chapter). |
| *Monte Carlo Methods in Financial Engineering* — Paul Glasserman | Advanced | The rigorous treatment of Monte Carlo for derivatives pricing: variance reduction (antithetic variates, control variates, importance sampling), discretisation schemes for SDEs, Greeks by simulation. Needed for anyone implementing a serious pricing library. Read after Numerical Recipes gives you the computational instincts. |

---

## Tier 5 — Finance (Enough to Function)

A QD doesn't need to price exotics by hand, but needs to understand what the systems they build are doing well enough to catch bugs and talk to researchers.

| Book | Level | Why it's here for a QD |
|---|---|---|
| *Options, Futures, and Other Derivatives* — John Hull | Core | The standard derivatives reference. A QD implementing a pricing library needs Hull for the formulas and their assumptions. Focus on: the binomial tree (Ch. 13), Black-Scholes (Ch. 15), Greeks (Ch. 19), and Monte Carlo simulation (Ch. 21). You don't need the exotic products chapters unless your firm trades them. |
| *Trading and Exchanges: Market Microstructure for Practitioners* — Larry Harris | Advanced | How order books, market makers, exchanges, and execution actually work. Required for QDs building execution systems or order management systems. Explains: price impact, spread decomposition, informed vs. uninformed order flow, market manipulation. The systems you build operate in this environment — knowing it makes you better at the job. |
| *Fixed Income Mathematics* — Frank Fabozzi | Core | Bond pricing, duration, convexity, yield curves — the fixed income toolkit that a QD at a bank or rates-focused fund needs. Lighter and more practical than Tuckman. Use if your employer trades fixed income. |

---

## Tier 6 — Interview Preparation

| Book | Level | Why it's here for a QD |
|---|---|---|
| *A Practical Guide to Quantitative Finance Interviews* — Xinfeng Zhou | Core | The "Green Book." Probability puzzles, brain teasers, statistics problems, and finance questions — the genre that dominates the maths rounds of QD interviews at HFT firms and quant funds. Work through all sections: brain teasers, calculus/linear algebra, probability, statistics, stochastic calculus (lightly), finance. |
| *Heard on the Street* — Timothy Crack | Core | The companion to the Green Book — more statistics and probability problems, fewer brain teasers. Covers: distributions, hypothesis testing, regression, derivatives basics, and the classic market-maker interview questions. Work through in parallel with the Green Book. |
| *Cracking the Coding Interview* — Gayle McDowell | Beginner–Core | The standard software engineer interview prep book. Less rigorous than a pure algorithms text, more interview-format-aware. Good for the "what to expect in a coding round" framing and for STAR-format behavioural prep. Use alongside LeetCode rather than instead of it. |

---

## Tier 7 — Context and Career

Read once, not for technique — for a realistic feel of the industry.

| Book | Level | Why it's here for a QD |
|---|---|---|
| *My Life as a Quant* — Emanuel Derman | Any level | Physicist-turned-quant memoir. Gives the QD perspective from someone who built models rather than traded them — the engineering side of quant finance, seen from inside Goldman Sachs. |
| *The Quants* — Scott Patterson | Any level | Narrative history of quantitative trading — the rise of systematic approaches and the 2007–08 crisis. Gives a QD the industry's history and some cautionary tales about models that worked until they didn't. |
| *Flash Boys* — Michael Lewis | Any level | HFT from the outside — the latency arms race, the IEX story, the infrastructure obsession. Not technically accurate in detail, but the cultural picture of HFT firms is recognisable. Good for understanding the world you'd be building for. |

---

## Suggested sequencing

```
Now (Year 3, Sem 2):
  Mathematics for Machine Learning      (Tier 1 — read this first)
  Fluent Python                         (Tier 2 — production Python immediately)
  A Tour of C++                         (Tier 2 — one chapter/week, no pressure)
  Green Book (Zhou)                     (Tier 6 — interview prep, 3 problems/week)

Next semester:
  CSAPP Ch. 5–6, 12                     (Tier 3 — performance and concurrency)
  CLRS Part I–III                       (Tier 2 — algorithms, driven by LeetCode)
  Effective Modern C++                  (Tier 2 — after Tour of C++ is done)
  Operating Systems: Three Easy Pieces  (Tier 3 — OS fundamentals)

Pre-graduation:
  DDIA                                  (Tier 3 — systems design interviews)
  C++ Concurrency in Action             (Tier 3 — when C++ is solid enough)
  Numerical Recipes (Ch. 7, 10, 21)     (Tier 4 — Monte Carlo and optimisation)
  High-Performance Python               (Tier 3 — before the C++ rewrite project)

Post-graduation / first role:
  Glasserman (Monte Carlo)              (Tier 4 — if pricing library work)
  Hull (relevant chapters)              (Tier 5 — as the job demands it)
  Trading and Exchanges                 (Tier 5 — if execution systems work)
```
