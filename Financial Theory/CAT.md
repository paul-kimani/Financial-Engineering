# CAT — Time Series & Regression Solutions

> A full worked CAT solution on time series and simple linear regression:
> defining a time series and its objectives, interpreting the simple linear
> model $y = \beta_0 + \beta_1 x + \epsilon$ and its assumptions, then
> linearising an exponential growth curve $Y = a\beta^x$ via logs, fitting it by
> least squares to get $\hat{Y} = 1.984(1.391)^x$, predicting $\hat{Y}_{x=6} = 14.39$,
> and computing $SE(\hat{\beta}_0) = 0.0128$.

---

Below is a full worked solution using the uploaded notes. I will treat **Question 2 as an exponential curve** because the table includes $x=0$, so a power curve $Y = aX^\beta$ is not valid at $x=0$. The data pattern also clearly fits $Y = a\beta^x$.

---

## Question One

### (a) Define time series

A **time series** is a sequence of observations recorded at successive points in time, usually at equally spaced intervals. It may be written as:

$$
x_1, x_2, \ldots, x_n
$$

or

$$
\{x_t : t = 1, 2, \ldots, n\}
$$

Examples include daily stock prices, monthly rainfall, annual GDP, hourly temperature, and weekly sales. This follows the definition in the notes.

---

### (b) State and explain two objectives of time series

#### 1. Description

This means describing the important features or patterns in the time series. A time series plot may reveal trend, seasonality, changing variance, dependence, outliers, missing values, and breaks.

#### 2. Prediction / Forecasting

This means using past and present values of the series to estimate future values. For example, past sales data can be used to forecast future sales.

---

### (c) Simple linear regression model

Given:

$$
y = \beta_0 + \beta_1 x + \epsilon
$$

where $y$ is the dependent variable, $x$ is the independent variable, and $\epsilon$ is the random error term. The notes define simple linear regression in this form.

#### (i) Physical interpretation of $\beta_0$ and $\beta_1$

$\beta_0$ is the **intercept**. It is the expected value of $y$ when $x = 0$.

$\beta_1$ is the **slope**. It measures the change in the mean value of $y$ for a one-unit increase in $x$. The notes state that $\beta_0$ and $\beta_1$ have the physical interpretation of intercept and slope respectively.

#### (ii) Define $\epsilon$ and state its significance

$\epsilon$ is the **random error term**. It captures the difference between the actual observed value of $y$ and the value predicted by the regression line.

Its significance is that it accounts for deviations of the data from the fitted straight-line model. In practice, it represents effects of omitted variables, measurement errors, and random disturbances.

#### (iii) State any three assumptions of the model

Any three:

1. The model is linear in $X$.
2. The errors have zero mean:

$$
E(\epsilon_i) = 0
$$

3. The errors have constant variance:

$$
\operatorname{Var}(\epsilon_i) = \sigma^2
$$

4. The errors are independent:

$$
\operatorname{Cov}(\epsilon_i, \epsilon_j) = 0
$$

5. There is no relationship between the error and the corresponding $x$-value:

$$
\operatorname{Cov}(\epsilon_i, x_i) = 0
$$

The notes list these as classical regression assumptions.

---

## Question Two

Given:

| $x$   |  0 |   1 |    2 |    3 |   4 |
| ----: | -: | --: | ---: | ---: | --: |
| $Y_t$ |  2 | 2.7 | 3.90 | 5.36 | 7.4 |

The curve expected from the data is:

$$
Y = a\beta^x
$$

Taking natural logs:

$$
\ln Y = \ln a + x\ln\beta
$$

Let:

$$
Y^* = \ln Y, \quad \beta_0 = \ln a, \quad \beta_1 = \ln\beta
$$

So the transformed linear model is:

$$
Y^* = \beta_0 + \beta_1 x
$$

---

### Step 1: Transform the data

| $x$ |  $Y$ | $\ln Y$ |
| --: | ---: | ------: |
|   0 | 2.00 |  0.6931 |
|   1 | 2.70 |  0.9933 |
|   2 | 3.90 |  1.3610 |
|   3 | 5.36 |  1.6789 |
|   4 | 7.40 |  2.0015 |

---

### Step 2: Fit the linear regression

Using least squares:

$$
\hat{\beta}_1 = \frac{S_{xy}}{S_{xx}}
$$

$$
\hat{\beta}_0 = \bar{Y}^* - \hat{\beta}_1\bar{x}
$$

The notes give the least-squares idea as choosing parameter estimates to minimize the sum of squared residuals.

From the calculations:

$$
\hat{\beta}_0 = 0.6851
$$

$$
\hat{\beta}_1 = 0.3302
$$

Therefore:

$$
\ln a = 0.6851
$$

$$
a = e^{0.6851} = 1.9839
$$

and:

$$
\ln\beta = 0.3302
$$

$$
\beta = e^{0.3302} = 1.3913
$$

So the fitted curve is:

$$
\boxed{\hat{Y} = 1.9839(1.3913)^x}
$$

Approximately:

$$
\boxed{\hat{Y} = 1.984(1.391)^x}
$$

---

### (ii) Predict $Y_t$ when $x = 6$

$$
\hat{Y} = 1.9839(1.3913)^6
$$

$$
\hat{Y} = 14.3897
$$

Therefore:

$$
\boxed{\hat{Y} \approx 14.39}
$$

---

### (iii) Calculate the standard error of $\hat{\beta}_0$

For the transformed regression:

$$
SE(\hat{\beta}_0) = s\sqrt{\frac{1}{n} + \frac{\bar{x}^2}{S_{xx}}}
$$

where:

$$
s^2 = \frac{SSE}{n-2}
$$

From the residual calculations:

$$
SSE = 0.0008206
$$

$$
n = 5
$$

$$
s^2 = \frac{0.0008206}{5-2} = 0.0002735
$$

$$
s = 0.01654
$$

Also:

$$
\bar{x} = 2
$$

$$
S_{xx} = 10
$$

Therefore:

$$
SE(\hat{\beta}_0) = 0.01654\sqrt{\frac{1}{5} + \frac{2^2}{10}}
$$

$$
SE(\hat{\beta}_0) = 0.01654\sqrt{0.6}
$$

$$
SE(\hat{\beta}_0) = 0.01281
$$

Thus:

$$
\boxed{SE(\hat{\beta}_0) \approx 0.0128}
$$

---

## Final Answers

$$
\boxed{\hat{Y} = 1.984(1.391)^x}
$$

$$
\boxed{\hat{Y}_{x=6} = 14.39}
$$

$$
\boxed{SE(\hat{\beta}_0) = 0.0128}
$$

_Note: If the question truly intended $Y = aX^\beta$, then $x = 0$ makes the log transformation impossible because $\ln 0$ is undefined. In that case, the first data point would have to be omitted, but the paper likely intended the exponential model $Y = a\beta^x$._

---

**See also:** [[12.1 Revision CAT1]] · [[../Time Series/]]
