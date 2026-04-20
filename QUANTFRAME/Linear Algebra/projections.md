Alright — I’ll compress everything you wrote into **clean, high-yield notes** you can actually revise from (not just read once and forget).

---

#  PROJECTIONS, SPAN, ORTHOGONALITY 

---

# 1.  Core Idea: Projection

A projection splits a vector into two parts:

$$
b = b_{\text{proj}} + b_{\text{residual}}  
$$

- $( b_{\text{proj}} )$: lies in the subspace (explained part)
- $( b_{\text{residual}} )$: orthogonal to the subspace (error)

### Key Property:
$$
b_{\text{proj}} \perp b_{\text{residual}}  
$$

---

# 2. Intuition (Must Remember)

- Projection = **closest point in a subspace**
- Residual = **shortest error vector**
- Think: **shadow of a vector onto a surface**

---

# 3.  Projection onto a Line

Given vectors ( a ) and ( b ):

$$
\text{proj}_a(b) = \frac{a^T b}{a^T a} , a  
$$

### Breakdown:

- $( a^T b )$: alignment (how similar directions are)
- $( a^T a = |a|^2 )$: size of $( a )$
- Scalar = “how much of $( a )$ is in $( b )$”

---

# 4.  Decomposition

$$
b = b_{\parallel} + b_{\perp}  
$$

- $( b_{\parallel} = \text{projection} )$
- $( b_{\perp} = b - \text{projection} )$

---

# 5.  Critical Condition (Optimization Insight)

Projection minimizes:  
$$
|b - ca|^2  
$$

And satisfies:  
$$
a^T (b - \text{proj}_a(b)) = 0  
$$

This is why it's the **best approximation**

---

# 6.  Projection onto a Subspace

Let matrix $( A )$ span a subspace.

### Projection matrix:

$$
P = A(A^T A)^{-1} A^T  
$$

### Projection:

$$
\hat{b} = Pb  
$$

---

# 7. Key Condition (VERY IMPORTANT)

$$
A^T (b - Ax) = 0  
$$

 Residual is orthogonal to **every column of ( A )**

---

# 8. Special Case: Orthonormal Columns

If $( A^T A = I )$:

$$
P = A A^T  
$$

No inverse needed (this is why QR is powerful)

---

# 9. Projection Matrix Properties

A matrix $( P )$ is a projection if:
### 1. Symmetric:
$$
P^T = P  
$$

### 2. Idempotent:
$$
P^2 = P  
$$

---

### Eigenvalues:

- $( 1 )$: vectors in subspace
    
- $( 0 )$: vectors orthogonal to subspace
    

---

# 10.  Orthogonal Complement

For subspace ( S ):

$$
S^\perp = {v : v^T s = 0 \ \forall s \in S }  
$$

### Decomposition:

$$
\mathbb{R}^n = S \oplus S^\perp  
$$

 Every vector splits uniquely into:

- component in $( S )$
- component in $( S^\perp )$

---

# 11.  Complementary Projection

If:  
$$
P = \text{projection onto } S  
$$

Then:  
$$  
I - P = \text{projection onto } S^\perp  
$$

---

# 12. Least Squares = Projection

Problem:  
$$
Ax \approx b  
$$

Solution:  
$$
x̂ = (A^T A)^{-1} A^T b  
$$

---

### Interpretation:

- $( Ax̂ )$: projection of $( b )$ onto $( \text{Col}(A) )$
- Residual:   $$
    r = b - Ax̂  
    $$
### Key condition:

$$  
A^T r = 0  
$$

---

# 13.  Regression Interpretation

$$
y = X\beta + \epsilon  
$$

- $( X\betâ )$: projection (explained returns)
- $( \epsilon )$: residual (noise)
    

---

# 14. Quant Finance Meaning

|Concept|Meaning|
|---|---|
|Projection|systematic component|
|Residual|idiosyncratic risk|
|Column space|factor space|
|Orthogonality|no remaining signal|

---

# 15.  MASTER SUMMARY

### Everything reduces to this:

- Span → defines subspace
- Projection → closest point in that subspace
- Residual → leftover error
- Orthogonality → error is independent
---

# One Sentence to Remember Forever

> **Projection finds the best approximation inside a #span, and the error is always orthogonal.**
