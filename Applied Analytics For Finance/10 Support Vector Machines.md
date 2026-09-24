# Support Vector Machines

## 1. Hyperplanes

A **hyperplane** is a flat affine subspace of dimension $p-1$ in $p$-dimensional space (it need not pass through the origin). In 2D, a hyperplane is just a line:

$$\beta_0+\beta_1X_1+\beta_2X_2=0$$

Generalizing to $p$ dimensions:

$$\beta_0+\beta_1X_1+\beta_2X_2+\dots+\beta_pX_p=0$$

You can tell which side of the hyperplane a point lies on just from the **sign** of the left-hand side.

## 2. The classification setup

Training data: an $n\times p$ matrix $\mathbf X$, with each observation's class $Y_i\in\{-1,+1\}$ (note: not $\{0,1\}$ like logistic regression — SVM's math is cleaner with $\pm1$ labels). A **separating hyperplane**, if one exists, satisfies:

$$Y_i(\beta_0+\beta_1X_{i1}+\dots+\beta_pX_{ip})>0 \quad \forall i=1,\dots,n$$

(the sign of the classifier's output always matches the true label). A new test point $X^*$ is classified by the **sign** of $f(X^*)=\beta_0+\beta_1X_1^*+\dots+\beta_pX_p^*$ — positive $\to$ class $+1$, negative $\to$ class $-1$. Note this is structurally the same linear-boundary idea as logistic regression's decision boundary in [[09 Logistic Regression and Classification#The decision boundary|ch.9]], just with a different labeling convention and a different fitting criterion.

## 3. The Maximal Margin Classifier

If a separating hyperplane exists, there are usually infinitely many of them. The **maximal margin hyperplane** is the one that maximizes the **margin** $M$ — the perpendicular distance from the hyperplane to the *nearest* training observation on either side. Points that lie exactly at this minimum distance are the **support vectors** — they're the only points that actually determine where the hyperplane sits.

Optimization problem:

$$\max_{\beta_0,\dots,\beta_p,M} M \quad\text{s.t.}\quad \sum_{j=1}^p\beta_j^2=1,\qquad Y_i(\beta_0+\beta_1X_{i1}+\dots+\beta_pX_{ip})\ge M\;\;\forall i$$

($M>0$ is the cushion; the constraint set ensures every observation is on the correct side, at least distance $M$ from the hyperplane.) This is a genuine constrained-optimization problem — the same flavor of Lagrangian machinery from [[03 - Duality of Linear Programming|the LP-duality unit]] applies here (the actual SVM dual is worked through in that unit's notes, §9, as a concrete non-LP example of Lagrangian duality).

**Problem:** if no separating hyperplane exists (the **non-separable case**), $M>0$ has no solution at all. And even when one exists, the maximal margin hyperplane can be *extremely* sensitive to a single observation — moving one point can flip the hyperplane entirely, a strong sign of overfitting the training data.

## 4. The Support Vector Classifier (soft margin)

The fix: allow some observations to sit on the *wrong* side of the margin, or even the wrong side of the hyperplane entirely — trading a little training accuracy for a hyperplane that's far more robust (this is, again, the bias-variance tradeoff from [[07 Model Assessment, Bias-Variance and Resampling#2. The Bias-Variance Decomposition|ch.7]], now expressed geometrically). This is the **Support Vector Classifier** (soft-margin classifier):

$$\max_{\beta_0,\dots,\beta_p,M} M \quad\text{s.t.}\quad \sum_{j=1}^p\beta_j^2=1,\qquad Y_i(\beta_0+\dots+\beta_pX_{ip})\ge M(1-\epsilon_i),\qquad \epsilon_i\ge0,\;\;\sum_{i=1}^n\epsilon_i\le C$$

- $\epsilon_i$ (slack variables) — reveal where observation $i$ sits relative to the margin/hyperplane: $\epsilon_i=0$ = correct side of margin; $0<\epsilon_i\le1$ = violates the margin but still correct side of the hyperplane; $\epsilon_i>1$ = wrong side of the hyperplane entirely.
- $C\ge0$ (the **budget**, a tuning parameter, typically chosen via cross-validation — see [[07 Model Assessment, Bias-Variance and Resampling#4.1 Cross-Validation CV|ch.7]]) — bounds $\sum\epsilon_i$, so it controls how many/how severe the margin violations can be. Larger $C$ = more tolerance for violations = wider margin, more bias, less variance; smaller $C$ = stricter, narrower margin, less bias, more variance.

Observations that lie exactly on the margin, or on the wrong side of it, are the **support vectors** — and critically, the decision rule depends *only* on these, not on points far from the boundary. This is what makes the support vector classifier robust to outliers away from the boundary, unlike, say, LDA (not covered in depth here) which uses the full data.

## 5. Support Vector Machines: nonlinear boundaries via kernels

The support vector classifier only handles a *linear* boundary between classes. The **Support Vector Machine (SVM)** extends it to non-linear boundaries by enlarging the feature space using **kernels**.

### The inner-product form

The linear support vector classifier can be written entirely in terms of inner products between observations:

$$f(X) = \beta_0+\sum_{i=1}^n\alpha_i\langle X_i,X\rangle$$

where $\langle a,b\rangle=\sum_{i=1}^r a_ib_i$ is the inner product, and $\alpha_1,\dots,\alpha_n,\beta_0$ are parameters estimated from the $\binom{n}{2}$ pairs of training observations. Crucially, it turns out $\alpha_i\neq0$ **only for the support vectors** — so letting $\mathcal S$ be the index set of support points:

$$f(X) = \beta_0+\sum_{i\in\mathcal S}\alpha_i\langle X_i,X\rangle$$

### The kernel trick

A **kernel** $K(X_i,X_i')$ generalizes the inner product. Replace $\langle X_i,X\rangle$ with $K(X_i,X)$ everywhere and you get a non-linear classifier "for free," without ever explicitly computing coordinates in the enlarged feature space:

$$f(X) = \beta_0+\sum_{i\in\mathcal S}\alpha_iK(X_i,X_i')$$

Common kernels:

| Kernel | Formula | Notes |
|---|---|---|
| Linear | $K(X_i,X_i')=\sum_{j=1}^p X_{ij}X_{i'j}$ | recovers the plain support vector classifier |
| Polynomial (degree $d$) | $K(X_i,X_i')=\left(1+\sum_{j=1}^p X_{ij}X_{i'j}\right)^d$ | $d>1$ gives a much more flexible, curved boundary |
| Radial | $K(X_i,X_i')=\exp\!\left(-\gamma\sum_{j=1}^p(X_{ij}-X_{i'j})^2\right)$, $\gamma>0$ | highly local/flexible; behaves like a similarity measure that decays with distance |

The combination of a support vector classifier with a non-linear kernel is what's specifically called a **Support Vector Machine**. This "swap the inner product for a kernel" idea is a general and powerful trick — you'll see the exact same $\langle X_i,X\rangle\to K(X_i,X)$ substitution described from the optimization-theory side (via the SVM dual problem) in [[03 - Duality of Linear Programming#9. Game theory and SVM duality|the LP-duality unit's §9]].

## 6. Cheat sheet

| Concept | One-line takeaway |
|---|---|
| Hyperplane | $(p-1)$-dimensional flat subspace; classify by the sign of $f(X)$ |
| Maximal Margin Classifier | Maximizes distance to nearest point; fails if not linearly separable, or overfits if it barely is |
| Support vectors | The (few) points that actually determine the hyperplane |
| Support Vector Classifier | Soft margin via slack $\epsilon_i$, budget $C$ — trades training accuracy for robustness |
| $C$ | Tuning parameter: large $C$ = high bias/low variance; small $C$ = low bias/high variance |
| Kernel trick | Replace inner product with $K(\cdot,\cdot)$ to get non-linear boundaries without explicit feature expansion |
| SVM | Support vector classifier + non-linear kernel |

## 7. Open questions for revision

- [ ] Work through why $\alpha_i\neq0$ only for support vectors — this sparsity property is what makes SVM predictions computationally cheap even with large $n$.
- [ ] Compare the radial kernel's behavior to the [[Kernel Density Function|Kernel Density Estimator]] from ch.3 — both use a "bump function centered on each data point" idea; how does the bandwidth-like role of $\gamma$ compare to KDE's bandwidth $b$?
- [ ] Cross-reference the SVM dual problem (Lagrangian relaxation) with [[03 - Duality of Linear Programming|the LP-duality unit]]'s §9 to see the full derivation of how the kernel trick emerges from the dual's structure.

---
*Source: BSF 3216 Lecture Two — "Supervised Learning" (Support Vector Machines).*
