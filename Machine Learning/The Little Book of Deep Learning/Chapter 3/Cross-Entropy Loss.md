**The Logic:** Now that we have a probability (e.g., the model says there is a 90% chance this is a dog), we need to calculate how "wrong" the model is. In statistics, the best model is the one that _maximizes the likelihood_ of the true data. If the image is actually a dog, we want the probability $\hat{P}(dog)$ to be as close to 1.0 as possible.

**The Derivation:**

1. **Maximum Likelihood:** For a dataset of $N$ samples, the total likelihood is the product of all the individual probabilities of the correct classes. Maximizing a product of many small fractions leads to numerical underflow (the computer rounds to zero).
2. **Log-Likelihood:** To fix this, we take the natural logarithm (log). The log function turns multiplication into addition, which is mathematically stable. We want to _maximize_ the sum of the logs of the correct probabilities.
    
3. **Turning it into a "Loss":** In machine learning, optimization algorithms are designed to _minimize_ an error, not maximize a score. Therefore, we simply multiply our log-likelihood by −1. Maximizing a number is the exact same as minimizing its negative.
    
4. **Averaging:** We divide by N to get the average loss across all samples, so our metric doesn't artificially inflate just because we used a larger batch size.
    

**The Conclusion:** This gives us the Cross-Entropy Loss function. We minimize the negative average log-probability of the true classes (yn​):

Lce​(w)=−N1​n=1∑N​logP^(Y=yn​∣X=xn​)

If you substitute the Softmax formula into this P^, you get the expanded version shown in the text.

### 3. Contrastive (Triplet) Loss

**The Logic:** Instead of assigning a category, Metric Learning wants the model to act like a ruler. The model outputs a distance f(x1​,x2​;w). The logic is simple:

- The distance between an anchor (xa​) and a positive match (xb​) should be small.
    
- The distance between the anchor (xa​) and a negative match (xc​) should be large.
    

We want to guarantee a "safety margin" between these two distances. Let's say the margin is 1. The negative distance must be at least 1 unit larger than the positive distance:

f(xa​,xc​;w)≥f(xa​,xb​;w)+1

**The Derivation:**

1. **Rearrange the inequality:** We want an equation that we can push toward zero. Let's move everything to one side:
    
    1−f(xa​,xc​;w)+f(xa​,xb​;w)≤0
    
2. **Apply the Hinge Loss:** If the statement above is true (the resulting math is ≤0), the model did its job perfectly. The loss should be 0.
    
3. If the statement is false (the result is >0), the model failed, and the loss should be exactly that positive amount. We can represent this behavior perfectly using a Maximum function: max(0,value).
    

**The Conclusion:** If the negative sample is safely far away, the internal math is negative, and max(0,negative number)=0(No loss!). If the negative sample is too close, the internal math is positive, and the model receives a penalty proportional to the violation:

max(0,1−f(xa​,xc​;w)+f(xa​,xb​;w))