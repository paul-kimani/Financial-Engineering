### The Softmax Function

**The Logic:** A neural network outputs raw, unbounded numbers called "logits", denoted as $f(x;w)y$​ (the score for class $y$ given input $x$ and weights $w$). If we are trying to classify an image as a dog, cat, or bird, we cannot work with raw scores like `[5.2, -1.1, 2.0]`. We need them to be a valid probability distribution where every value is between 0 and 1, and they all sum exactly to 1.

**The Derivation:**

1. **Make everything positive:** We cannot have negative probabilities. To convert any real number to a strictly positive number, we use the exponential function $e^z$ (or $exp(z)$).
    - Our logit for a specific class $y$ becomes: $exp(f(x;w)y)$​
        
2. **Normalize to sum to 1:** To make sure our positive numbers represent parts of a whole (100%), we divide the score of our target class by the sum of the scores of _all_ possible classes (denoted by z).
    
    - Sum of all classes: $\sum{z~​expf(x;w)z}​$

**The Conclusion:** By dividing the specific class's exponentiated score by the sum of all exponentiated scores, we arrive at the estimated probability $\hat{P}$ for class $y$:
$$
\hat{P}(Y=y|X=x)= \frac{e^{f(x;w)z}}{\sum{z~e^{f(x;w)z}}}
$$
