### **1. Motivation: Transforming to the Log-Stock Price**

The BSM PDE is a fundamental tool for pricing options. The original form is expressed in terms of the stock price ($S$):

$$\frac{\partial V}{\partial t} + rS\frac{\partial V}{\partial S} + \frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} - rV = 0$$

The task set in this session was to derive the equivalent PDE in terms of the log-stock price, $x = \ln S$, showing all the necessary steps — including the transformation of the 1st and 2nd partial derivatives.

### **2. The Transformation and the First Derivative**

The transformation is:

$$x = \ln S \implies S = e^x$$

Express $\dfrac{\partial V}{\partial S}$ in terms of $x$, using the chain rule for partial derivatives:

$$\frac{\partial V}{\partial S} = \frac{\partial V}{\partial x}\cdot\frac{\partial x}{\partial S}$$

but $x = \ln S$, so $\dfrac{\partial x}{\partial S} = \dfrac1S$:

$$\therefore \frac{\partial V}{\partial S} = \frac{\partial V}{\partial x}\cdot\frac1S$$

### **3. The Second Derivative (Chain Rule + Product Rule)**

Express $\dfrac{\partial^2V}{\partial S^2}$ in terms of $x$ (chain rule for the partial derivative, plus the product rule):

$$\frac{\partial^2V}{\partial S^2} = \frac{\partial}{\partial S}\left\{\frac{\partial V}{\partial S}\right\} = \frac{\partial}{\partial S}\left\{\frac{\partial V}{\partial x}\cdot\frac1S\right\}$$

This is a product of two terms, $u = \dfrac1S$ and $w = \dfrac{\partial V}{\partial x}$:

$$\frac{\partial(uw)}{\partial S} = u\frac{\partial w}{\partial S} + w\frac{\partial u}{\partial S}$$

**1st term:**

$$\frac{\partial u}{\partial S} = \frac{\partial}{\partial S}\left\{\frac1S\right\} = \frac{\partial}{\partial S}\{S^{-1}\} = -S^{-2} = -\frac1{S^2}$$

**2nd term** — apply the chain rule again, as with the 1st derivative:

$$\frac{\partial w}{\partial S} = \frac{\partial}{\partial S}\left\{\frac{\partial V}{\partial x}\right\} = \frac{\partial}{\partial x}\left\{\frac{\partial V}{\partial x}\right\}\cdot\frac{\partial x}{\partial S} = \frac{\partial^2V}{\partial x^2}\cdot\frac{\partial x}{\partial S} = \frac{\partial^2V}{\partial x^2}\cdot\frac1S$$

Combining via the product rule:

$$\frac{\partial^2V}{\partial S^2} = u\frac{\partial w}{\partial S} + w\frac{\partial u}{\partial S} = \frac1S\left(\frac{\partial^2V}{\partial x^2}\cdot\frac1S\right) - \frac1{S^2}\cdot\frac{\partial V}{\partial x}$$

$$\therefore \frac{\partial^2V}{\partial S^2} = \frac1{S^2}\frac{\partial^2V}{\partial x^2} - \frac1{S^2}\frac{\partial V}{\partial x}$$

### **4. Substituting Back into the PDE**

From the original PDE, the two $S$-dependent terms transform as:

$$rS\frac{\partial V}{\partial S} = rS\left(\frac1S\frac{\partial V}{\partial x}\right) = r\frac{\partial V}{\partial x}$$

$$\frac12\sigma^2S^2\frac{\partial^2V}{\partial S^2} = \frac12\sigma^2S^2\left\{\frac1{S^2}\frac{\partial^2V}{\partial x^2} - \frac1{S^2}\frac{\partial V}{\partial x}\right\} = \frac12\sigma^2\left\{\frac{\partial^2V}{\partial x^2} - \frac{\partial V}{\partial x}\right\}$$

### **5. The PDE in Terms of the Log-Stock Price**

Substituting both transformed terms into the original PDE gives:

$$\frac{\partial V}{\partial t} + r\frac{\partial V}{\partial x} + \frac12\sigma^2\left(\frac{\partial^2V}{\partial x^2} - \frac{\partial V}{\partial x}\right) - rV = 0$$

### **6. The Feynman-Kac Theorem**

The Feynman-Kac Theorem establishes a connection between a **stochastic differential equation (SDE)** and a **partial differential equation (PDE)**. It's a key result particularly for pricing options, as it allows us to solve a PDE by calculating an expectation under a risk-neutral measure.

The theorem states that the solution to certain PDEs can be represented as a conditional expected value of a function of a stochastic process.

Consider a stochastic process $X_t$ that follows the SDE:

$$dX_t = \mu(X_t,t)\,dt + \sigma(X_t,t)\,dW_t$$

Also define a function:

$$F(t,x) = \mathbb{E}_{t,x}\left[\phi(X_T)\right]$$

Therefore, the Feynman-Kac Theorem states that the function $F(t,x)$ is the solution to the PDE:

$$\frac{\partial F}{\partial t} + \mu(t,x)\frac{\partial F}{\partial x} + \frac12\sigma^2(t,x)\frac{\partial^2F}{\partial x^2} = 0$$

with the terminal condition:

$$F(T,x) = \phi(x)$$

**Proof:** *(The session's notes cut off right after the "Proof:" heading — to be picked up and filled in from the next class.)*
