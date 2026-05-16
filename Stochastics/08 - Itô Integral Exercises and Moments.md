# 08 — Itô Integral Exercises and Moments

> Combining Itô's lemma with the fact that
> $\mathbb{E}\bigl[\int_0^t (\cdot)\,dW_s\bigr] = 0$ turns moment
> computations into ordinary ODEs in $t$. The same trick yields closed forms
> for many Itô integrals involving $\cos$, $\sin$, and powers of $W_t$.

---

## 1. The general method — applying Itô's lemma to find expected values

Suppose we want to compute $\mathbb{E}[Y_t]$ where $Y_t$ is a function of
some underlying stochastic process. The standard procedure is:

1. **Write $Y_t$ as a function of an Itô process.** Set
   $Y_t = f(t, Z_t)$ where $Z_t$ has a known SDE
   $dZ_t = \mu_t\,dt + \sigma_t\,dW_t$.
2. **Apply Itô's lemma** to obtain $dY_t = \alpha_t\,dt + \beta_t\,dW_t$.
3. **Write in integrated form:**

   $$
   Y_t \;=\; Y_0 + \int_0^t \alpha_s\,ds + \int_0^t \beta_s\,dW_s.
   $$

4. **Take expectations.** The Itô integral has mean zero, so

   $$
   \boxed{\;\mathbb{E}[Y_t] \;=\; Y_0 + \int_0^t \mathbb{E}[\alpha_s]\,ds.\;}
   $$

This converts the SDE problem into an **ordinary ODE** for
$\mathbb{E}[Y_t]$, which can be solved directly.

---

## 2. Worked example — $\mathbb{E}[W_t^4]$

### Step 1 — set up $Z_t = f(t, W_t)$

Let $Z_t = W_t^4$, so $f(t, x) = x^4$ with $x = W_t$.

### Step 2 — partial derivatives

$$
\frac{\partial f}{\partial t} = 0, \qquad \frac{\partial f}{\partial x} = 4x^3, \qquad \frac{\partial^2 f}{\partial x^2} = 12 x^2.
$$

### Step 3 — Itô's lemma

$$
df \;=\; 0\cdot dt + 4 x^3\,dx + \tfrac{1}{2}\cdot 12 x^2\,(dx)^2 \;=\; 4 x^3\,dW_t + 6 x^2\,dt,
$$

since $(dW_t)^2 = dt$. With $x = W_t$,

$$
dZ_t \;=\; 6\,W_t^2\,dt + 4\,W_t^3\,dW_t.
$$

### Step 4 — integrate

$$
Z_t - Z_0 \;=\; \int_0^t 6\,W_s^2\,ds + \int_0^t 4\,W_s^3\,dW_s.
$$

Since $W_0 = 0$ gives $Z_0 = 0$,

$$
W_t^4 \;=\; \int_0^t 6\,W_s^2\,ds + \int_0^t 4\,W_s^3\,dW_s.
$$

### Step 5 — take expectations

The Itô integral $\int_0^t 4\,W_s^3\,dW_s$ has mean zero, leaving

$$
\mathbb{E}[W_t^4] \;=\; 6\int_0^t \mathbb{E}[W_s^2]\,ds \;=\; 6\int_0^t s\,ds \;=\; \boxed{\,3 t^2.\,}
$$

This matches the standard moment formula for a Gaussian
$W_t \sim \mathcal{N}(0, t)$, since
$\mathbb{E}[N(0,\sigma^2)^4] = 3\sigma^4$.

---

## 3. Worked example — $\mathbb{E}[W_t^6]$

Same recipe, one power higher.

Let $Z_t = W_t^6$, $f(t, x) = x^6$:

$$
\frac{\partial f}{\partial x} = 6 x^5, \qquad \frac{\partial^2 f}{\partial x^2} = 30 x^4.
$$

Itô's lemma:

$$
dZ_t \;=\; 6\,W_t^5\,dW_t + \tfrac{1}{2}\cdot 30\,W_t^4\,dt \;=\; 6\,W_t^5\,dW_t + 15\,W_t^4\,dt.
$$

Integrated form ($W_0 = 0$):

$$
W_t^6 \;=\; \int_0^t 6\,W_s^5\,dW_s + \int_0^t 15\,W_s^4\,ds.
$$

Take expectations. The Itô integral vanishes; using
$\mathbb{E}[W_s^4] = 3 s^2$ from the previous example,

$$
\mathbb{E}[W_t^6] \;=\; 15\int_0^t 3 s^2\,ds \;=\; 15\cdot t^3 \;=\; \boxed{\,15\,t^3.\,}
$$

### General formula for even moments of a Gaussian

If $Z \sim \mathcal{N}(0, 1)$,

$$
\mathbb{E}[Z^{2n}] \;=\; \frac{(2n)!}{2^n\,n!}.
$$

For $W_t \sim \mathcal{N}(0, t)$ this scales as

$$
\mathbb{E}[W_t^{2n}] \;=\; t^n \cdot \frac{(2n)!}{2^n\,n!}.
$$

Sanity checks:

- $n = 1$: $\mathbb{E}[W_t^2] = t$. ✓
- $n = 2$: $\mathbb{E}[W_t^4] = 3 t^2$. ✓
- $n = 3$: $\mathbb{E}[W_t^6] = 15 t^3$. ✓

Odd moments are all zero because $W_t$ is symmetric.

---

## 4. Worked example — $\displaystyle\int_0^t \cos W_s\,dW_s$

A typical "find a function whose Itô differential matches the integrand"
exercise.

### Step 1 — choose $f$

Antiderivative of $\cos x$ is $\sin x$, so try $f(t, x) = \sin x$ with
$x = W_t$:

$$
f(t, W_t) \;=\; \sin W_t.
$$

### Step 2 — Itô's lemma

$$
\frac{\partial f}{\partial t} = 0, \qquad \frac{\partial f}{\partial W_t} = \cos W_t, \qquad \frac{\partial^2 f}{\partial W_t^2} = -\sin W_t.
$$

So

$$
d(\sin W_t) \;=\; 0\cdot dt + \cos W_t\,dW_t + \tfrac{1}{2}\,(-\sin W_t)\,(dW_t)^2.
$$

Using $(dW_t)^2 = dt$,

$$
d(\sin W_t) \;=\; \cos W_t\,dW_t - \tfrac{1}{2}\,\sin W_t\,dt.
$$

### Step 3 — integrate from $0$ to $t$

$$
[\sin W_s]_0^t \;=\; \int_0^t \cos W_s\,dW_s - \tfrac{1}{2}\int_0^t \sin W_s\,ds.
$$

Since $W_0 = 0 \Rightarrow \sin W_0 = 0$,

$$
\sin W_t \;=\; \int_0^t \cos W_s\,dW_s - \tfrac{1}{2}\int_0^t \sin W_s\,ds.
$$

### Step 4 — rearrange

$$
\boxed{\;\int_0^t \cos W_s\,dW_s \;=\; \sin W_t + \tfrac{1}{2}\int_0^t \sin W_s\,ds.\;}
$$

The same approach with $f(x) = -\cos x$ gives the symmetric identity

$$
\int_0^t \sin W_s\,dW_s \;=\; 1 - \cos W_t - \tfrac{1}{2}\int_0^t \cos W_s\,ds.
$$

---

## 5. Exercises (recommended)

### A. Find closed forms for the following Itô integrals

1. $\displaystyle\int_0^t W_s^2\,dW_s$ &nbsp; *(hint: $f(x) = x^3/3$)*
2. $\displaystyle\int_0^t e^{k W_s}\,dW_s$ &nbsp; *(hint: $f(x) = e^{kx}/k$, watch the Itô correction!)*

### B. Compute the following expectations

1. $\mathbb{E}\!\left[e^{k W_t}\right]$ &nbsp; *(use the MGF of a Gaussian)*
2. $\mathbb{E}\!\left[(W_t - W_s)^2\right]$
3. $\mathbb{E}\!\left[(W_t - W_s)^4\right]$
4. Show that $\mathbb{E}\!\left[\displaystyle\int_0^T W_s\,dW_s\right] = 0$ &nbsp; and &nbsp; $\operatorname{Var}\!\left[\displaystyle\int_0^T W_s\,dW_s\right] = T^2/2$. &nbsp; *(both follow from [[02 - The Itô Integral]] without computation.)*

### C. SDE manipulation

Consider the SDE

$$
dX_t \;=\; A_t\,dt + Y_t\,dB_t.
$$

1. Write down Itô's lemma for $df(t, X_t)$ where $f$ is a suitable function.
2. Determine $df(t, X_t)$ for $f(t, X_t) = e^{2 t X_t}$.

---

## Cross‑references

- [[03 - Itô's Lemma - Statement and Derivation]] — the lemma being applied.
- [[07 - Solving SDEs with Itô's Lemma]] — the same trick used to derive
  the GBM closed form and $\int W_s\,dW_s$.
- [[09 - Martingales]] — formalises the "$\mathbb{E}[\int \cdot\,dW] = 0$"
  step.
