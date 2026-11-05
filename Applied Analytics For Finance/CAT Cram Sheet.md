# CAT Cram Sheet — BSF 3216

> One-page recall sheet: formulas, the assumption behind each result, and the classic traps.

**Symbols.** $n$ = observations; $p$ = predictors (excluding the intercept); $p+1$ = number of $\beta$'s. SSReg = regression SS, SSR = residual SS, TSS = SSReg + SSR.

## Data
- **Domains:** nominal (labels) → ordinal (order, no equal gaps) → interval (equal gaps, arbitrary zero: °C) → ratio (true zero: price). Negative oil futures behave as interval data.
- **Preprocessing:** format (normalise/scale/standardise), type (discretise/binarise), structural (pivot). Fit on the **train set only** (avoid leakage).

## OLS / MLR
- $\hat\beta=(X'X)^{-1}X'Y$, from $\partial S/\partial\beta=-2X'Y+2X'X\beta=0$. Normal equations: $X'\hat e=0$.
- **Assumptions:** ML1 linear · ML2 random sample · ML3 $E(\varepsilon\mid X)=0$ · ML4 full rank · spherical $E(\varepsilon\varepsilon'\mid X)=\sigma^2I$ · normality (for exact tests/CRLB).
- **Unbiased:** $\hat\beta=\beta+(X'X)^{-1}X'\varepsilon$, so $E\hat\beta=\beta$. Needs **ML3** only.
- **Variance:** $\sigma^2(X'X)^{-1}$. Needs **spherical errors**.
- **Count the $\varepsilon$'s:** one → ML3; $\varepsilon_i^2$ → homoskedasticity; $\varepsilon_i\varepsilon_j$ → no autocorrelation.
- $s^2=\text{SSR}/(n-p-1)$ is unbiased; $\text{SSR}/n$ (MLE) is biased down.
- **CRLB:** $\mathcal I=\text{diag}(X'X/\sigma^2,\ n/2\sigma^4)$, so $\mathcal I^{-1}=\text{diag}(\sigma^2(X'X)^{-1},\ 2\sigma^4/n)$. OLS attains it, so it is efficient (needs normality).
- **BLUE (Gauss–Markov):** $C=A+D$, $DX=0$, $\text{Var}=\sigma^2(X'X)^{-1}+\sigma^2DD'$. No normality needed.
- **Distributions:** $\hat\beta\sim N(\beta,\sigma^2(X'X)^{-1})$; $(\hat\beta-\beta)'X'X(\hat\beta-\beta)/\sigma^2\sim\chi^2_{p+1}$; $\hat\beta\perp\hat\sigma^2$.

## ANOVA and tests
| Source | df | MS |
|---|---|---|
| Reg | $p$ | SSReg/$p$ |
| Error | $n-p-1$ | SSR/$(n-p-1)$ = $s^2$ |
| Total | $n-1$ | |

- **Overall F:** $F=\dfrac{\text{SSReg}/p}{\text{SSR}/(n-p-1)}=\dfrac{R^2/p}{(1-R^2)/(n-p-1)}$, testing $H_0:\beta_1=\dots=\beta_p=0$.
- **Partial F:** $\dfrac{[\text{SSR}_R-\text{SSR}_F]/(p_F-p_R)}{\text{SSR}_F/(n-p_F-1)}=\dfrac{(R_F^2-R_R^2)/(p_F-p_R)}{(1-R_F^2)/(n-p_F-1)}$. ⚠️ The notes' denominators are wrong.
- **$t=\hat\beta_j/\text{SE}$**, df $n-p-1$. One extra variable: $F=t^2$.
- **Sequential ANOVA** (`anova()`) is order-dependent: each row is "given the rows above". Only the **last** row's $F=t^2$. The total SSReg, residuals and $R^2$ don't depend on order.
- **$R^2$** = SSReg/TSS, never falls when a variable is added. **$\bar R^2=1-(1-R^2)\frac{n-1}{n-p-1}$** rises iff the added variable's $|t|>1$.
- **AIC** $=n\ln\hat\sigma^2+2(p+1)$; **BIC** $=n\ln\hat\sigma^2+(p+1)\ln n$, with $\hat\sigma^2=\text{SSR}/n$. Smaller is better. BIC is stricter once $n\ge8$. F needs nested models; AIC/BIC don't.

## Reading R output (checklist)
1. $n$ = residual df + $p$ + 1.
2. Check: $R^2=$ SSReg/TSS, $F$ from $R^2$, RSE $=\sqrt{\text{MSE}}$.
3. Interpret each $\hat\beta$ "holding others fixed".
4. Significance via $|t|$ or $|z|>1.96$. **Check the printed p-values against $t$/$z$.**
5. Overall F, $R^2$ vs $\bar R^2$.
6. Residual quartiles: symmetric? fat tails?

## Bias–variance
- $\text{Err}(x_0)=\sigma^2+\text{Bias}^2+\text{Var}$.
- More flexible models: bias ↓, variance ↑. Training error always ↓; test error is U-shaped.
- Training error underestimates test error because the model fitted the noise.

## Logistic regression
- $\ln\frac{p}{1-p}=\eta=x'\beta$ and $p=\frac{e^\eta}{1+e^\eta}$. A $+1$ in $X$ gives log-odds $+\beta$, odds $\times e^\beta$, and $\Delta p\approx\beta p(1-p)\le\beta/4$.
- **MLE:** $l=\sum[Y\eta-\ln(1+e^\eta)]$; score $\sum x_i(Y_i-p_i)=0$; Hessian $-\sum x_ix_i'p_i(1-p_i)$, which is concave, so there is one peak. Solved by Newton: $\beta-H^{-1}g$.
- Wald $z=\hat\beta/\text{SE}$. Predict Up if $\hat p>0.5$.
- **LPM problem:** $p$ can leave $[0,1]$.

## Classification metrics
⚠️ The notes' $f_{ij}$ has **rows = actual**; R's `table(pred, actual)` has **rows = predicted**. Label the cells first.

| Metric | Formula |
|---|---|
| Accuracy | $(TP+TN)/n$ |
| Error | $1-$ accuracy |
| Precision | $TP/(TP+FP)$ |
| Recall | $TP/(TP+FN)$ |
| Specificity | $TN/(TN+FP)$ |
| F1 | $2PR/(P+R)$ |
| Type I | $FP/(FP+TN)$ |
| Type II | $FN/(TP+FN)$ |

- Always compare accuracy with the majority-class baseline.
- Raising the threshold: precision ↑, recall ↓.

## Bayes
- $P(C\mid x)=\dfrac{P(x\mid C)P(C)}{P(x)}$: posterior ∝ likelihood × prior.
- **Naive Bayes:** $P(C\mid x)\propto P(C)\prod_jP(x_j\mid C)$. It assumes features are **independent given the class**. Predict the class with the largest posterior.
