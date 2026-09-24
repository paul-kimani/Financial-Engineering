# 01 — Optimization and Linear Programming Fundamentals

> The building blocks every later chapter assumes: what decision variables,
> objective functions and constraints are, and how optimization problems get
> classified (constrained vs. unconstrained, continuous vs. discrete).

---

### The Three Essential Elements
Every optimization model is built on three specific mathematical components:
- **Decision Variables:** These are the unknown inputs you have the power to control and want the solver to determine. In a quantitative model, for example, these could be the exact number of shares to allocate to Safaricom versus KCB on the Nairobi Securities Exchange, or the specific threshold parameters in a Python mean-reversion script.

- **Objective Function:** The mathematical equation representing your ultimate goal. You are either trying to maximize this function (like total expected portfolio return or a Sharpe ratio) or minimize it (like risk, tracking error, or execution latency in a C++ order routing engine).

- **Constraints:** The strict mathematical boundaries your decision variables must respect. These translate real-world limitations into math, such as a strict capital budget, risk limits, or a rule prohibiting short sales.
### Classifying the Problem
Optimization problems are categorized based on the nature of these elements:

- **Constrained vs. Unconstrained:** If a problem has strict boundaries (constraints), it is a constrained optimization problem. If variables can take any value without restriction, it is unconstrained. Unconstrained problems are mathematically simpler but rarely reflect real-world markets.
  
- **Continuous vs. Discrete (Integer):**
  
    - **Continuous:** The decision variables can take any real number value, including fractions (e.g., allocating $45.5\%$ of capital to a specific asset).

    - **Discrete (Integer):** The variables are restricted to whole numbers or a specific set of distinct values (e.g., you cannot buy a fraction of a server, or you must route orders in whole lots).

### The Mathematical Setup

To represent an optimization problem mathematically, we define a mapping and a domain:

- **The Mapping:** We define a function $f: \mathbb{R}^n \to \mathbb{R}$. This takes an $n$-dimensional vector of decision variables $x = (x_1, x_2, \dots, x_n)$ and maps it to a single scalar value (the cost, loss, or return).

- **The Feasible Set ($S$):** We define a subset $S \subseteq \mathbb{R}^n$ representing the **feasible region**—the collection of all points $x$ that satisfy every constraint simultaneously.

The problem of finding the best point is written as:
$$\min_{x} f(x) \quad \text{s.t.} \quad x \in S$$
_(Note: Maximizing a function $g(x)$ is mathematically equivalent to minimizing $-g(x)$, so the standard convention is almost always framed as a minimization problem.)_
### Two Failure Modes of a Problem

Before looking for a solution, a formulation can fail in two primary ways:

1. **Infeasibility ($S = \emptyset$):**

    The feasible set $S$ is empty. This occurs when constraints contradict one another (e.g., $x \ge 5$ and $x \le 2$), meaning no point exists that satisfies the criteria.

2. **Unboundedness:**

    There exists a sequence of feasible points $x_k \in S$ such that as $k \to \infty$, the objective function $f(x_k) \to -\infty$. This typically indicates a missing or poorly defined constraint, allowing the function to decrease infinitely.
### Defining the Solution: Global Minimizers

If a problem is neither infeasible nor unbounded, we look for a minimizer:
- **Global Minimizer ($x^*$):**

    A point $x^* \in S$ is a global minimizer if its objective value is less than or equal to the objective value of every other point in the entire feasible set:
$$f(x^*) \le f(x), \quad \forall x \in S$$
- **Strict Global Minimizer:**

    A point $x^* \in S$ is a strict global minimizer if it uniquely achieves the lowest value with no ties:
$$f(x^*) < f(x), \quad \forall x \in S \text{ where } x \neq x^*$$

**Local Minimizers and the "Open Ball"**

In non-convex optimization, an algorithm might settle at a point that is the best in its immediate vicinity, even if it is not the absolute lowest point overall. This is a **local minimizer**.

A point $x^* \in S$ is a local minimizer if it yields a value less than or equal to any other feasible point within a small, restricted neighborhood. Mathematically, this neighborhood is defined using an **open ball**, denoted as $B_{x^*}(\varepsilon)$, with a radius of $\varepsilon > 0$.

The open ball is the set of all points $x$ whose distance (or norm) from $x^*$ is strictly less than $\varepsilon$:
$$B_{x^*}(\varepsilon) = \{x : \vert{}\vert{}x - x^*\vert{}\vert{} < \varepsilon\}$$
For $x^*$ to be a local minimizer, the following must hold true for all points that are both in the feasible set $S$ _and_ inside this open ball:

$$f(x^*) \le f(x), \quad \forall x \in S \cap B_{x^*}(\varepsilon)$$

A **strict local minimizer** applies the exact same concept, but with a strict inequality ($<$), meaning it is uniquely the lowest point in that local neighborhood.

**Explicit Functional Constraints**
In computational models, the feasible set $S$ is almost never left as an abstract concept. It is explicitly programmed using distinct mathematical functions to represent equalities and inequalities.

- $g_i(x) = 0$ represents **equality constraints** (e.g., a portfolio's weights must sum exactly to $1$). The set of all these constraints is indexed by $E$.

- $g_i(x) \ge 0$ represents **inequality constraints** (e.g., no single asset weight can be less than $0$). The set of all these constraints is indexed by $I$.

The feasible region $S$ is literally built from these functions:
$$S = \{x : g_i(x) = 0, \forall i \in E \text{ and } g_i(x) \ge 0, \forall i \in I\}$$

**The Generic Optimization Problem Form**
By substituting the explicit constraints back into the original $\min_{x} f(x)$ framework, we arrive at the standard generic form. Every complex quantitative optimization problem ultimately maps back to this specific structure:
$$\min f(x)$$

$$\text{s.t.} \quad g_i(x) = 0, \quad i \in E$$

$$\quad g_i(x) \ge 0, \quad i \in I$$

The efficiency of solving an optimization problem depends directly on its size and mathematical structure. Three primary factors dictate how easily and quickly an algorithm can find a solution:

- The total number of decision variables involved.

- The total number of constraints bounding the problem.

- The specific mathematical properties of both the objective function and the constraint functions.

**Problem Complexity** Certain mathematical properties make optimization problems significantly easier to solve efficiently. You generally want your models to fall into one of two categories:

- **Linearity:** Problems featuring a linear objective function governed by linear constraints are straightforward to compute.

- **Convexity:** Problems are also easier to solve if they feature both a convex objective function and a convex feasible set.

**Geometric Analysis** Because these mathematical properties drastically alter how difficult a problem is, it is necessary to analyze the geometry and properties of the functions _before_ attempting to solve them. Understanding whether a function is linear, convex, or non-convex determines which algorithms will actually work, which naturally leads directly into defining exactly what convexity is.


**The Intuition of Convexity**

- A function is considered convex based on a simple geometric rule involving any two points on its curve.

- If you select any two points on the x-axis, $x$ and $y$, and map them to their corresponding points on the function at $(x, f(x))$ and $(y, f(y))$, you can draw a straight line connecting those two coordinates.

- For a convex function, this connecting straight line will always sit perfectly on or entirely _above_ the actual curve of the function.


**The Convex Combination**

- To mathematically define any arbitrary point $z$ that sits on the x-axis strictly between $x$ and $y$, the model uses a weighted average called a convex combination.

- This combination is written as $z = \lambda x + (1 - \lambda)y$, where the coefficient $\lambda$ is bounded by $0 \le \lambda \le 1$.

- The coefficients $\lambda$ and $(1 - \lambda)$ are designed to always sum up to exactly $1$.

- If $\lambda = 0$, the formula isolates the right boundary, resulting in $z = y$.

- If $\lambda = 1$, the formula isolates the left boundary, resulting in $z = x$.

- For any fractional value of $\lambda$ between $0$ and $1$, the resulting coordinate $z$ lands somewhere strictly between $x$ and $y$.

**The Formal Mathematical Definition of Convexity**

- The coordinates of Point A (on the curve) are exactly the function evaluated at our intermediate point $z$: $f(\lambda x + (1-\lambda)y)$.

- The coordinates of Point B (on the straight line above it) represent the weighted average of the y-values: $\lambda f(x) + (1-\lambda)f(y)$.

- Stating that Point B must sit at or above Point A gives us the universal inequality that defines convexity:
$$f(\lambda x + (1-\lambda)y) \le \lambda f(x) + (1-\lambda)f(y)$$
**The Crucial Importance of Convexity**

- When an objective function is convex, any local optimum you find is mathematically guaranteed to be the global optimum.

- In applied quantitative models—like optimizing a portfolio's asset weights or minimizing execution latency in a C++ routing engine—this property is a massive advantage. If your trading system's solver finds a minimum cost for a batch of XAUUSD orders, convexity ensures the algorithm hasn't just settled into a deceptive, shallow "valley." It guarantees you have found the absolute best possible answer globally.

- Because of this guarantee, it is highly advisable to formulate optimization problems using convex functions whenever possible.

**Introduction to Linear Programming (LP)**

- Linear programming is arguably the most well-known and frequently solved type of optimization problem.

- An LP problem strictly requires optimizing a linear objective function subject only to linear equality and inequality constraints. There are no exponents, curves, or interacting variables—just straight lines and flat planes.

- For simple two-dimensional problems featuring only two decision variables and a moderate number of constraints, LPs can be solved visually using a graphical method to map the feasible region.

**The Four Possible Outcomes of an LP** When solving an LP graphically (or algorithmically), there are only four possible ways the problem can resolve based on the shape of the feasible region and the slope of the objective function:

- **Unique Optimal Solution:** The solver finds exactly one single best point. In LPs, this always occurs at a corner (vertex) of the feasible region.

- **Infinitely Many Bounded Solutions:** The objective function's slope perfectly matches the slope of one of the constraint boundaries. Any point along that specific line segment yields the exact same optimal cost or return.

- **Unbounded Solutions:** The feasible region is completely open in the direction the objective function is trying to optimize. The cost can decrease infinitely (or profit can increase infinitely) because a bounding constraint is missing.

- **Infeasible (No Solution):** The constraints contradict each other entirely, meaning no feasible point exists that satisfies all rules simultaneously.

**Algorithmic Solvers for Large-Scale LPs** Graphical methods are useless beyond two or three dimensions. When dealing with a large number of decision variables and constraints—such as optimizing a quantitative portfolio across hundreds of equities—the problems become notoriously difficult to solve without specialized algorithms.

- **The Simplex Method:** Developed by George Dantzig in 1947, this is a highly effective tool for solving linear problems. Geometrically, it works by finding a starting corner of the feasible region and "walking" along the edges to adjacent corners, constantly improving the objective function until it reaches the optimal point.

- **Interior-Point Methods:** Introduced by Narendra Karmarkar in 1984, this algorithm takes a different approach. Instead of walking along the outside edges, it cuts a path directly through the "interior" (the inside) of the feasible region. It features an efficient polynomial runtime, making it the preferred engine for massive, high-dimensional optimizations in modern software.

**The General Form in Linear Algebra** To scale these algorithms to $n$-dimensions, the generic constraints are rewritten using vector and matrix notation. If $x$ is a vector of your decision variables, and $c$ is a vector of cost coefficients, the problem takes the following standard form:

  

$$\min f(x) = c^T x$$

  

$$\text{Subject to:} \quad a_i^T x = b, \quad i \in E$$

  

$$\quad a_i^T x \ge b, \quad i \in I$$

  

Here, $c$, $a_i$, and $b$ are all constant vectors belonging to $\mathbb{R}^n$. In a trading context, $c^T x$ could represent multiplying a vector of expected returns ($c$) by a vector of position sizes ($x$) to calculate a portfolio's total yield.