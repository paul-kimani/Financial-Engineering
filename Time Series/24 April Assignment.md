### **Multiple Linear Regression Estimation**
![[Images/WhatsApp Image 2026-04-24 at 09.16.22.jpeg]]
To estimate the regression coefficients $\beta_0$, $\beta_1$, and $\beta_2$, we will use the Ordinary Least Squares (OLS) method by solving the system of normal equations.

![[Images/WhatsApp Image 2026-04-24 at 09.17.47.jpeg]]
The normal equations for a multiple linear regression model with two independent variables are:
1. $n\beta_0 + \beta_1\sum X_1 + \beta_2\sum X_2 = \sum Y$
2. $\beta_0\sum X_1 + \beta_1\sum X_1^2 + \beta_2\sum X_1 X_2 = \sum X_1 Y$
3. $\beta_0\sum X_2 + \beta_1\sum X_1 X_2 + \beta_2\sum X_2^2 = \sum X_2 Y$

**Step 1: Calculate the required sums from the data**

Given $n = 5$ observations:

- $\sum X_1 = 1 + 2 + 3 + 4 + 5 = 15$
- $\sum X_2 = 2 + 1 + 4 + 3 + 5 = 15$
- $\sum Y = 10 + 12 + 13 + 15 + 16 = 66$
- $\sum X_1^2 = 1^2 + 2^2 + 3^2 + 4^2 + 5^2 = 1 + 4 + 9 + 16 + 25 = 55$
- $\sum X_2^2 = 2^2 + 1^2 + 4^2 + 3^2 + 5^2 = 4 + 1 + 16 + 9 + 25 = 55$
- $\sum X_1 X_2 = (1)(2) + (2)(1) + (3)(4) + (4)(3) + (5)(5) = 2 + 2 + 12 + 12 + 25 = 53$
- $\sum X_1 Y = (1)(10) + (2)(12) + (3)(13) + (4)(15) + (5)(16) = 10 + 24 + 39 + 60 + 80 = 213$
- $\sum X_2 Y = (2)(10) + (1)(12) + (4)(13) + (3)(15) + (5)(16) = 20 + 12 + 52 + 45 + 80 = 209$

---

**Step 2: Set up the Normal Equations**

Substitute the calculated sums into the three normal equations:

1. $5\beta_0 + 15\beta_1 + 15\beta_2 = 66$
2. $15\beta_0 + 55\beta_1 + 53\beta_2 = 213$
3. $15\beta_0 + 53\beta_1 + 55\beta_2 = 209$

---

**Step 3: Solve the system of equations**

First, eliminate $\beta_0$. Multiply equation (1) by 3:

- $15\beta_0 + 45\beta_1 + 45\beta_2 = 198 \quad \text{--- (Equation 1a)}$
Subtract (1a) from equation (2):
- $(15\beta_0 + 55\beta_1 + 53\beta_2) - (15\beta_0 + 45\beta_1 + 45\beta_2) = 213 - 198$
- $10\beta_1 + 8\beta_2 = 15 \quad \text{--- (Equation 4)}$

Subtract (1a) from equation (3):

- $(15\beta_0 + 53\beta_1 + 55\beta_2) - (15\beta_0 + 45\beta_1 + 45\beta_2) = 209 - 198$
- $8\beta_1 + 10\beta_2 = 11 \quad \text{--- (Equation 5)}$

Now, solve the system of two equations (4 and 5). Multiply equation (4) by 5 and equation (5) by 4:
- $50\beta_1 + 40\beta_2 = 75$
- $32\beta_1 + 40\beta_2 = 44$

Subtract the lower equation from the upper equation:

- $18\beta_1 = 31$
- $\beta_1 = \frac{31}{18} \approx 1.722$

Substitute $\beta_1$ back into Equation 4 to find $\beta_2$:

- $10\left(\frac{31}{18}\right) + 8\beta_2 = 15$
- $\frac{310}{18} + 8\beta_2 = \frac{270}{18}$
- $8\beta_2 = \frac{270 - 310}{18}$
- $8\beta_2 = -\frac{40}{18}$
- $\beta_2 = -\frac{5}{18} \approx -0.278$

Substitute $\beta_1$ and $\beta_2$ back into the original Equation 1 to find $\beta_0$:

- $5\beta_0 + 15\left(\frac{31}{18}\right) + 15\left(-\frac{5}{18}\right) = 66$
- $5\beta_0 + \frac{465}{18} - \frac{75}{18} = 66$
- $5\beta_0 + \frac{390}{18} = 66$
- $5\beta_0 + \frac{65}{3} = 66$
- $5\beta_0 = \frac{198}{3} - \frac{65}{3}$
- $5\beta_0 = \frac{133}{3}$
- $\beta_0 = \frac{133}{15} \approx 8.867$

**Step 4: Final Regression Coefficients and Line**

**Estimated Coefficients:**

- $\beta_0 = \frac{133}{15} \approx 8.867$
- $\beta_1 = \frac{31}{18} \approx 1.722$
- $\beta_2 = -\frac{5}{18} \approx -0.278$

**Estimated Regression Line:**

$$Y = 8.867 + 1.722X_1 - 0.278X_2$$