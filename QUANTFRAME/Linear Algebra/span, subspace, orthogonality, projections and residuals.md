[[projections]]
# 1. Span = “what can I build?”

Think of vectors as building blocks.

If you have vectors ( $v_1, v_2$ ), their **span** is:

> all possible linear combinations of them

So:  
$$
\text{span}(v_1, v_2) = { a v_1 + b v_2 \mid a,b \in \mathbb{R} }  
$$

 Intuition:

- 1 vector → a **line**
- 2 independent vectors → a **plane**
- 3 independent vectors → **3D space**

So span = _everything you can reach using those vectors_

---

# 2. Subspace = “a valid mini-universe”

A **subspace** is just a span that behaves nicely.

Formally, it must:

- Contain the zero vector
- Be closed under addition
- Be closed under scalar multiplication

 Key idea:

> Every span is automatically a subspace

So when you hear:

- “column space”
- “span of columns”
They mean the same thing.

---

# 3. Orthogonality = “perfect independence”

Two vectors are **orthogonal** if:  
$$
v \cdot w = 0  
$$

 Intuition:

- They point in completely different directions
- Like x-axis vs y-axis
Orthogonality is stronger than independence:
- Independent = not redundant
- Orthogonal = maximally non-overlapping
---

# 4. Projection = “closest approximation”

Now we connect everything.

Suppose:
- You have a vector ( b )
- And a subspace ( S ) (span of some vectors)

You ask:

> “What’s the closest vector in ( S ) to ( b )?”

That answer is the **projection**.

---

### Key formula (1 vector case)
$$
\mathrm{proj}_v(b) = \frac{b \cdot v}{v \cdot v} v
$$
---

👉 Intuition:  
You’re “dropping a shadow” of ( b ) onto the subspace.

---

# 5. Residual = “what’s left over”

After projection:

$$
\text{residual} = b - \hat{b}  
$$

where:

- $( \hat{b} )$ = projection

---

### The MOST IMPORTANT fact:

$$
\text{residual} \perp \text{subspace}  
$$

 This is the whole game.

---

# 6. Why orthogonality matters (this is the key insight)

The projection is defined so that:

$$
b - \hat{b} \perp S  
$$

That means:

- The error is **completely unrelated** to the space
- You cannot improve the approximation any further

---

# 7. Connect to linear regression (this is what you're really seeing)

When solving:  
$$
Ax \approx b  
$$

You are:

- Projecting ( b ) onto the **column space of A**

So:
- $( Ax )$ = projection
- $( b - Ax )$ = residual
---

### Core condition:
$$
A^T (b - Ax) = 0  
$$
$$
A^T(b - Ax) = 0
$$
---

 Meaning:

- Residual is orthogonal to every column of ( A )
- So it’s orthogonal to the entire column space

---

# 8. Mental picture (this unlocks everything)

Imagine:

- A plane (your subspace)
- A point floating above it (your vector ( b ))

Then:
- Projection = shadow on the plane
- Residual = vertical arrow down to the plane

And that arrow is:

> perfectly perpendicular

---

# 9. Why you’re getting stuck (honest diagnosis)

Most people try to:

- Memorize formulas  instead of
- Seeing geometry


The real structure is:

```
span → defines subspace
subspace → where projection happens
projection → best approximation
residual → orthogonal error
```

---

# 10. If you want to master this fast

Focus on just ONE idea:

> “Projection = closest point in a span, and error is orthogonal”

Everything else (least squares, regression, normal equations) comes from that.

---
