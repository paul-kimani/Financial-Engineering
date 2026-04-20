This is a core exam topic in financial mathematics. The approach depends entirely on the **level of rigor expected** in your course. There are three standard approaches:

1.  **The Heuristic "Proof" (Sketch via Taylor Expansion)** - *Most common for MBA/MSc Finance.*
2.  **The Formal Proof (For Simple Functions & Limit)** - *Common for Math/Engineering degrees.*
3.  **The Statement Only** - *Used if the full proof is too long for the exam time.*

Here is exactly how to structure the answer for each scenario.

---

### Scenario 1: The Standard "Finance Exam" Heuristic Proof
**Context:** You are asked to show *why* the extra term \(\frac{1}{2}f''(X_t)\sigma^2 dt\) appears.
**Approach:** 2nd Order Taylor Expansion + "Box Algebra" for \(dW^2\).

#### Part 1: Statement of Itô's Lemma
Let $(X_t$) be an Itô process satisfying:
$$ dX_t = \mu_t dt + \sigma_t dW_t $$

Let $(f(t, x))$ be a twice continuously differentiable function. Then $(Y_t = f(t, X_t))$ is an Itô process, and its differential is:
$$ df(t, X_t) = \left( \frac{\partial f}{\partial t} + \mu_t \frac{\partial f}{\partial x} + \frac{1}{2} \sigma_t^2 \frac{\partial^2 f}{\partial x^2} \right) dt + \sigma_t \frac{\partial f}{\partial x} dW_t$$

#### Part 2: Proof (Heuristic Derivation)
1.  **Expand using Taylor's Theorem:**
    $$
    df = \frac{\partial f}{\partial t} dt + \frac{\partial f}{\partial x} dX + \frac{1}{2} \frac{\partial^2 f}{\partial x^2} (dX)^2 + \dots
    $$
    *(Note: Higher order terms $( (dt)^2, (dt dX) )$ go to zero faster than $(dt)$.)*

2.  **Substitute $(dX)$ into the $((dX)^2$) term:**
    $$
    (dX)^2 = (\mu dt + \sigma dW)^2 = \mu^2 (dt)^2 + 2\mu\sigma (dt)(dW) + \sigma^2 (dW)^2
    $$

3.  **Apply the "Rules of Thumb" (Quadratic Variation):**
    - $((dt)^2 \approx 0)$ (Negligible)
    - $(dt \cdot dW \approx 0)$ (Negligible)
    - $((dW)^2 = dt)$ **(This is the ONLY non-zero second-order term)**

4.  **Substitute back:**
    $$
    (dX)^2 = \sigma^2 dt
    $$

5.  **Plug $((dX)^2)$ and $(dX)$ back into the Taylor expansion:**
    $$
    df = \frac{\partial f}{\partial t} dt + \frac{\partial f}{\partial x} (\mu dt + \sigma dW) + \frac{1}{2} \frac{\partial^2 f}{\partial x^2} (\sigma^2 dt)
    $$

6.  **Group the $(dt)$ terms and the $(dW)$ terms:**
    $$
    df = \left( \frac{\partial f}{\partial t} + \mu \frac{\partial f}{\partial x} + \frac{1}{2} \sigma^2 \frac{\partial^2 f}{\partial x^2} \right) dt + \sigma \frac{\partial f}{\partial x} dW
    $$
    *Q.E.D. (Outline)*

---
## Scenario 2, the rigorous proof

This is a deeper dive into the mathematical foundations. The heuristic "Taylor expansion + box algebra" proof is sufficient for most applied finance exams, but a **rigorous proof** requires building the result from the ground up using measure theory and $(L^2)$ limits.

Here is the complete structure of the rigorous proof. We will prove **Itô's Lemma for Brownian Motion** (the hardest part), then state the extension to general Itô processes.

### Theorem Statement (One-Dimensional Itô's Lemma)
Let $( f \in C^2(\mathbb{R}) )$ (twice continuously differentiable with bounded derivatives). Let $( B_t )$ be a standard Brownian motion. Then for any $( t \ge 0 )$, almost surely:
$$
f(B_t) = f(0) + \int_0^t f'(B_s) dB_s + \frac{1}{2} \int_0^t f''(B_s) ds
$$
Or in differential notation:
$$
df(B_t) = f'(B_t) dB_t + \frac{1}{2} f''(B_t) dt
$$

---

### Part 1: The Setup (Approximation by Step Functions)

We will prove this using a partition of the interval $([0, t])$.
Let $( \Pi_n = \{0 = t_0 < t_1 < \dots < t_n = t\} )$ be a sequence of partitions such that the mesh $( \|\Pi_n\| = \max_k |t_{k+1} - t_k| \to 0 )$ as $( n \to \infty )$.

We start with a **telescoping sum**:
$$
f(B_t) - f(0) = \sum_{k=0}^{n-1} \left[ f(B_{t_{k+1}}) - f(B_{t_k}) \right]
$$

---

### Part 2: Applying Taylor's Theorem (Rigorous Version)

Since $( f \in C^2 )$, we can apply the **exact** Taylor expansion with Lagrange remainder.
For each increment $( \Delta B_k = B_{t_{k+1}} - B_{t_k} )$:
$$
f(B_{t_{k+1}}) - f(B_{t_k}) = f'(B_{t_k}) \Delta B_k + \frac{1}{2} f''(B_{t_k}) (\Delta B_k)^2 + R_k
$$
Where the remainder term $( R_k )$ satisfies:
$$
|R_k| \le \epsilon(|\Delta B_k|) \cdot (\Delta B_k)^2
$$
with $( \epsilon(h) \to 0 )$ as $( h \to 0 )$ (this uses the uniform continuity of $( f'' )$ on compacts or the fact that $( B_t )$ is bounded in probability).

Summing over all $( k )$:
$$
f(B_t) - f(0) = \underbrace{\sum_{k=0}^{n-1} f'(B_{t_k}) \Delta B_k}_{\text{(A)}} + \frac{1}{2} \underbrace{\sum_{k=0}^{n-1} f''(B_{t_k}) (\Delta B_k)^2}_{\text{(B)}} + \underbrace{\sum_{k=0}^{n-1} R_k}_{\text{(C)}}
$$

We must examine the limit of (A), (B), and (C) as $( n \to \infty )$.

---

### Part 3: Convergence of Term (A) - The Itô Integral

By the **definition of the Itô integral**:
- The sum $( \sum f'(B_{t_k}) \Delta B_k )$ is exactly the integral of a **simple process** where the integrand is evaluated at the **left endpoint** $( t_k )$.
- Since $( f' )$ is continuous and $( B_t )$ has continuous paths, $( f'(B_t) )$ is an adapted process with finite second moment.
- By the construction of the Itô integral, the sum converges **in $( L^2 )$ (mean square)** to the Itô integral:

$$
\sum_{k=0}^{n-1} f'(B_{t_k}) \Delta B_k \xrightarrow{L^2} \int_0^t f'(B_s) dB_s
$$

---

### Part 4: Convergence of Term (B) - The Quadratic Variation Trick

This is the heart of the rigorous proof. We need to show that:
$$
\sum_{k=0}^{n-1} f''(B_{t_k}) (\Delta B_k)^2 \xrightarrow{L^2} \int_0^t f''(B_s) ds

$$

**Step 4.1: The Quadratic Variation Theorem**
It is a fundamental theorem of Brownian motion that the quadratic variation $( \langle B \rangle_t )$ satisfies:
$$
\lim_{n \to \infty} \sum_{k=0}^{n-1} (\Delta B_k)^2 = t \quad \text{(in probability, and in } L^2 \text{)}
$$

**Step 4.2: The Localization Argument**
We want to replace $( (\Delta B_k)^2 )$ with $( \Delta t_k = t_{k+1} - t_k )$. Consider the difference:
$$
D_n = \sum_{k=0}^{n-1} f''(B_{t_k}) \left[ (\Delta B_k)^2 - \Delta t_k \right]
$$
We need to show $( \mathbb{E}[D_n^2] \to 0 )$.

**Expanding the square of the sum:**
$$
\mathbb{E}[D_n^2] = \mathbb{E} \left[ \sum_{k=0}^{n-1} \sum_{j=0}^{n-1} f''(B_{t_k}) f''(B_{t_j}) \left( (\Delta B_k)^2 - \Delta t_k \right) \left( (\Delta B_j)^2 - \Delta t_j \right) \right]
$$

**Key Observation (Independence of Increments):**
- If $( k \neq j )$, assume WLOG $( k < j )$. Then $( \Delta B_j )$ is independent of $( \mathcal{F}_{t_j} )$ which contains $( B_{t_k}, \Delta B_k, f''(B_{t_k}) )$.
- Since $( \mathbb{E}[(\Delta B_j)^2 - \Delta t_j | \mathcal{F}_{t_j}] = 0 )$, the expectation of the cross terms ($( k \neq j )$) is **ZERO**.
- Therefore, only the diagonal terms ($( k = j )$) survive in the expectation.

**Step 4.3: Bounding the Diagonal Terms**
$$
\mathbb{E}[D_n^2] = \sum_{k=0}^{n-1} \mathbb{E} \left[ (f''(B_{t_k}))^2 \left( (\Delta B_k)^2 - \Delta t_k \right)^2 \right]
$$
Since $( f'' )$ is bounded (say by $( M )$), we have:
$$
\mathbb{E}[D_n^2] \le M^2 \sum_{k=0}^{n-1} \mathbb{E} \left[ \left( (\Delta B_k)^2 - \Delta t_k \right)^2 \right]
$$
We know that for a normal random variable $( Z \sim \mathcal{N}(0, \sigma^2) )$, $( \mathbb{E}[(Z^2 - \sigma^2)^2] = 2\sigma^4$).
Applying this to $( \Delta B_k \sim \mathcal{N}(0, \Delta t_k) )$:
$$
\mathbb{E} \left[ \left( (\Delta B_k)^2 - \Delta t_k \right)^2 \right] = 2 (\Delta t_k)^2
$$
Thus:
$$
\mathbb{E}[D_n^2] \le 2 M^2 \sum_{k=0}^{n-1} (\Delta t_k)^2
$$

**Step 4.4: Taking the Limit**
Since the mesh goes to zero, $( \sum (\Delta t_k)^2 \le \|\Pi_n\| \sum \Delta t_k = \|\Pi_n\| t \to 0 )$.
Therefore $( \mathbb{E}[D_n^2] \to 0 )$, which proves convergence in $( L^2 )$.
Hence:
$$
\sum f''(B_{t_k}) (\Delta B_k)^2 \xrightarrow{L^2} \sum f''(B_{t_k}) \Delta t_k \xrightarrow{} \int_0^t f''(B_s) ds
$$

---

### Part 5: Convergence of Term (C) - The Remainder

Recall $( R_k )$ satisfies $( |R_k| \le \epsilon(|\Delta B_k|) (\Delta B_k)^2 )$.
We want to show $( \sum R_k \to 0 )$ in probability.
Using the Cauchy-Schwarz inequality:
$$
\left| \sum R_k \right| \le \sqrt{ \sum \epsilon(|\Delta B_k|)^2 } \cdot \sqrt{ \sum (\Delta B_k)^4 }
$$
- As $( n \to \infty )$, $( \max |\Delta B_k| \to 0 )$ (uniform continuity of paths), so $( \epsilon(|\Delta B_k|) \to 0 )$. The first term goes to 0.
- For the second term: $( \sum (\Delta B_k)^4 \to 0 )$ because $( \mathbb{E}[\sum (\Delta B_k)^4] = 3 \sum (\Delta t_k)^2 \to 0 )$.

Thus the remainder term vanishes in the limit.

---

### Part 6: Extension to General Itô Processes

Once Itô's Lemma is proven for $( B_t )$ directly, the extension to:
$$
dX_t = \mu_t dt + \sigma_t dB_t
$$
and a function $( f(t, x) )$ follows by applying the **chain rule** and the definition of the integral with respect to $( X_t )$. The time variable adds a standard calculus term $( \frac{\partial f}{\partial t} dt )$, and the drift term $( \mu_t dt )$ interacts with $( dB_t )$ to produce zero cross-variation.

The final rigorous statement is:
$$
f(t, X_t) - f(0, X_0) = \int_0^t \left( \frac{\partial f}{\partial s} + \mu_s \frac{\partial f}{\partial x} + \frac{1}{2} \sigma_s^2 \frac{\partial^2 f}{\partial x^2} \right) ds + \int_0^t \sigma_s \frac{\partial f}{\partial x} dB_s
$$

### Summary of the Rigorous Proof Structure
1.  **Telescoping Sum** (Breaks the interval into small pieces).
2.  **Taylor Expansion** (Applies exact calculus locally).
3.  **Quadratic Variation** (Replaces $( (\Delta B)^2 \) with \( \Delta t )$). *This is the only non-standard calculus step.*
4.  **Isometry & Independence** (Shows the variance of the error goes to zero).
5.  **Limit Argument** (Proves $( L^2 )$ convergence to the integrals).

---

### Summary: How to Choose Your Approach in an Exam

| If the question is...                                                              | Your Answer Should Be...                                                                                                                                         |
| :--------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **"State and *prove* Itô's Lemma"**                                                | Use **Scenario 1** (Taylor Expansion + Box Algebra). Show the expansion, highlight $((dW)^2 = dt)$, and group terms.                                             |
| **"Derive the Black-Scholes PDE"**                                                 | You just **use** Itô's Lemma. State it, apply it to \(C(t, S_t)\), set $(d\Pi = 0)$. You don't need to re-prove the lemma here.                                  |
| **"Give a heuristic argument for Itô's Lemma"**                                    | Use **Scenario 1**.                                                                                                                                              |
| **"Why does the second derivative term appear in Itô but not standard calculus?"** | Focus exclusively on Step 3 of Scenario 1: **"Because $((dW)^2)$ behaves like $(dt)$, not 0. In standard calculus, $((dx)^2)$ is order $(dt^2)$ and vanishes."** |

### Visual Aid for the Proof
If you draw this little table in your exam answer, it proves you understand **why** the math changes:

| Standard Calculus    | Stochastic Calculus                            |
| :------------------- | :--------------------------------------------- |
| $((dt)^2 = 0)$       | $((dt)^2 = 0)$                                 |
| $((dx)^2 \approx 0$) | $((dW)^2 = dt)$                                |
| Taylor Order: 1st    | Taylor Order: 2nd (because of the $(dt)$ term) |