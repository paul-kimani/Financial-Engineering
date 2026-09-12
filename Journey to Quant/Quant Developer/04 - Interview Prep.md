# 04 — Quant Developer Interview Prep

> The format, what each round tests, and specifically how to prepare for it.

---

## 1. Typical interview structure (top firms)

Most top QD interviews run 4–6 rounds across two stages:

**Phone screen / online assessment (OA):**
A 60–90 minute timed coding test on a platform like HackerRank or Karat. Usually 2–3 LeetCode-style problems at Medium/Hard difficulty. Some firms (IMC, Jane Street) run their own bespoke assessments that include probability puzzles alongside code.

**On-site or virtual loop (4–5 rounds):**
1. Coding round 1 — algorithms and data structures
2. Coding round 2 — system design or a domain-specific problem (e.g., implement an order book, write a price aggregator)
3. Probability / maths round — mental math, puzzle-solving under pressure
4. Systems / architecture round — "design a market data feed handler", "how would you build a low-latency OMS"
5. Behavioural / culture fit — usually the shortest and often the deciding factor at firms that care about collaboration

At banks the loop is shorter (3 rounds) and less competitive-programming heavy. At HFT firms it can run to 6+ rounds.

---

## 2. Coding rounds: what to practise

The core LeetCode problem types, ordered by frequency in QD interviews:

**Arrays and strings:** Sliding window, two pointers, prefix sums. Understand O(n) approaches to problems that look like they need O(n²).

**Hash tables:** Fast lookups, counting, grouping. Many "find duplicates / find pairs" problems.

**Trees and binary search:** BST properties, traversals (DFS/BFS), binary search on sorted arrays and on the answer space.

**Heaps / priority queues:** Top-K problems, median of a stream, event scheduling. The order book (P4 in the Projects chapter) is a heap problem in disguise.

**Dynamic programming:** Longest common subsequence, knapsack, coin change — the patterns more than the specific problems.

**Graphs:** BFS/DFS for shortest path and connectivity, topological sort.

**The neetcode.io roadmap** is the best free ordering of 150 core problems. Commit to 3 LeetCode problems per week minimum — one easy (warmup), one medium (core skill), one hard (stretch). Track velocity, not completion.

---

## 3. Probability and maths puzzles

The "Green Book" — *A Practical Guide to Quantitative Finance Interviews* (Xinfeng Zhou) — covers the genre. Core topics:

- **Expected value problems:** Expected number of coin flips to get HH, expected number of cards before an Ace, etc.
- **Conditional probability:** Bayes' theorem applied to poker / dice / card problems.
- **Geometric series / combinatorics:** How many ways to...? What's the probability of...?
- **Symmetry arguments:** Many quant puzzle answers come from symmetry — train yourself to look for it first.
- **Martingales (basic intuition):** What is the expected value of a fair game at stopping time? Optional stopping theorem.

*Heard on the Street* (Crack) is the other classic — covers both stats puzzles and brain teasers. Work through 5 problems per week alongside the LeetCode track.

---

## 4. Systems and architecture round

This round is less about memorised answers and more about structured thinking. Practice explaining:

**How you would build X from scratch:** Start with requirements (throughput? latency? consistency?), pick the right data structure (ring buffer for a market data feed, sorted skiplist for an order book), justify each choice, then reason about failure modes.

**Latency sources and mitigation:** Cache misses (false sharing, cache line alignment), system call overhead, lock contention, GC pauses (why C++ / Rust avoid this), network round trips.

**Classic systems questions for quant dev:**
- Design a market data feed handler that normalises data from 10 exchanges.
- Design an order management system. What happens if the network drops mid-order?
- You have a risk system that must reprice 100,000 options in under 1ms. How?
- What is false sharing? Write an example that demonstrates it.
- When would you use a lock-free data structure vs. a mutex? What are the tradeoffs?

**Resources:** *Designing Data-Intensive Applications* (Kleppmann) for distributed systems intuition; *Operating Systems: Three Easy Pieces* for OS fundamentals; your P4 (order book) project as a concrete example to discuss.

---

## 5. Behavioural round

QD behavioural questions are finance-flavoured but structurally standard. Prepare STAR (Situation, Task, Action, Result) answers for:

- A time you debugged a difficult production issue.
- A time you disagreed with a technical decision and what you did.
- A time you had to learn something quickly under pressure.
- Your most technically challenging project and what made it hard.
- Why you want to work specifically at this firm (research the firm's technology stack before every interview).

---

## 6. Preparation timeline

```
Now → 3 months:   LeetCode (3/week, neetcode roadmap order)
                   Probability puzzles (5/week, Green Book)
                   Build P1 + P2 from Projects chapter

3–6 months:        LeetCode (5/week, moving to Hard)
                   Systems reading (DDIA, OSTEP)
                   Build P3 + P4

6 months → grad:   Mock interviews (Pramp, Interviewing.io, or with peers)
                   Systems design practice
                   P6 (C++ rewrite) to have a concrete "I optimised X" story

Job applications:  Submit 3 months before your target start date.
                   Apply broadly — HFT is the dream but bank quant strats roles
                   are a real and valuable stepping stone.
```
