# Working Notes — Applied Analytics For Finance

Single running scratch file for active-recall writing across sessions. Add a new dated section per study session; don't split per chapter.

---

## 2026-09-10 — Chapters 03–11 (Asset Returns through Neural Networks stub)

Closed-book recall prompts — try these before checking the chapter notes:

1. Why do quants use log returns instead of simple returns? Name two distinct reasons.
2. Write out the TSS = SSReg + SSR identity, with degrees of freedom for each term. What does the F-statistic actually test?
3. Derive $\beta_i=\operatorname{Cov}(R_i,R_m)/\operatorname{Var}(R_m)$ — where does this come from?
4. State the bias-variance decomposition of expected test MSE. Which term does Ridge/Lasso primarily trade off?
5. Write the Ridge and Lasso penalty terms side by side. Which one can zero out a coefficient, and why?
6. Derive $\ln(p(X)/(1-p(X))) = \beta_0+\beta_1X$ from the logistic function.
7. What's the one key assumption behind Naive Bayes, and why does it make the posterior tractable?
8. Sketch the Support Vector Classifier's optimization problem from memory — what do $M$, $\epsilon_i$, and $C$ each control?
9. What does the kernel trick actually replace, mechanically, in the SVM decision function?

---

## 2026-09-12 — Ch.5 variable selection, Ch.12 diagnostics/GLS, Ch.9 MLE derivation

Closed-book recall prompts:

1. Write the AIC and BIC formulas from memory. Which one penalizes model size more harshly as $n$ grows, and why?
2. Explain the $Y=X^2$, $X\sim\mathcal N(0,1)$ toy example — why would a naive SLR fit miss this relationship entirely?
3. What does the Durbin-Watson test actually check for? What does a Breusch-Pagan/White test check for instead?
4. Write $\operatorname{Var}(\epsilon)=\sigma^2 I$ and explain, in your own words, exactly which two conditions this single matrix equation encodes.
5. What's the practical difference between GLS, WLS, and FGLS — and why is FGLS the one actually used in practice?
6. Derive the logistic-regression log-likelihood $\ell(\beta)$ from the likelihood $L(\beta)$ — don't just state it, write out the product-to-sum step.

---

## 2026-09-13 — Ch.4 full Gauss-Markov BLUE proof + unbiased $\hat\sigma^2$

Closed-book recall prompts:

1. Derive $w_i=\dfrac{X_i-\bar X}{\sum_j(X_j-\bar X)^2}$ from the OLS slope formula, and show $\sum_i w_i=0$, $\sum_i w_iX_i=1$.
2. Starting from $\hat\beta_1=\beta_1+\sum_i w_i\epsilon_i$, which two assumptions (name them) are needed to conclude $\mathbb E(\hat\beta_1)=\beta_1$ — and what specific job does each one do?
3. Why is "$\epsilon$ is Normally distributed" *never* the right justification anywhere in the unbiasedness or minimum-variance proof? What's the one property of $\epsilon$ that actually matters?
4. Set up the double sum $\left(\sum_i w_i\epsilon_i\right)^2=\sum_i\sum_j w_iw_j\epsilon_i\epsilon_j$ and show which assumptions collapse the off-diagonal vs. the diagonal terms.
5. State the source-material typo in $\operatorname{Var}(\hat\beta_0)$ and the one-line sanity check ($\bar X=0$) that catches it.
6. For the "Best" proof: define an arbitrary linear estimator $\hat\beta_0^*=\sum_i a_iY_i$ and derive its two unbiasedness conditions on $\sum_i a_i$ and $\sum_i a_iX_i$ from scratch.
7. Write $a_i=v_i+d_i$ and derive $\sum_i d_i=0$, $\sum_i d_iX_i=0$ from the unbiasedness conditions on $a_i$ and $v_i$.
8. Prove $\sum_i v_id_i=0$ (the "(show this)" step) using $v_i=\frac1n-\bar Xw_i$ and the $d_i$ properties above.
9. Conclude $\operatorname{Var}(\hat\beta_0^*)=\operatorname{Var}(\hat\beta_0)+\sigma^2\sum_i d_i^2$ and explain in one sentence why this proves OLS is "Best."
10. Redo the entire "Best" argument for $\hat\beta_1^*$ from memory — what changes, and what stays identical?
11. Derive the residual identity $\hat\epsilon_i=(\epsilon_i-\bar\epsilon)-(\hat\beta_1-\beta_1)(X_i-\bar X)$ from $\hat\epsilon_i=Y_i-\hat\beta_0-\hat\beta_1X_i$ — what two substitutions does it take?
12. Show $\sum_i\hat\epsilon_i^2=\sum_i(\epsilon_i-\bar\epsilon)^2-S(\hat\beta_1-\beta_1)^2$, and derive $\mathbb E\left[\sum_i\hat\epsilon_i^2\right]=(n-2)\sigma^2$ from it.
13. Why does $\hat\sigma^2$ divide by $n-2$ rather than $n$? Explain degrees of freedom using OLS's own first-order conditions ($\sum_i\hat\epsilon_i=0$, $\sum_iX_i\hat\epsilon_i=0$).

---

## 2026-09-30 — Ch.1 data life cycle: selection bias, missingness, outliers, leakage, point-in-time

Closed-book recall prompts:

1. Ten stocks start at \$100; by the end 3 are delisted at \$0, 4 sit at \$150, 3 at \$300. Compute the true equal-weighted population return and the survivors-only return from scratch. State the bias in percentage points, and explain why the *dispersion* is understated as well as the mean overstated.
2. Explain why "the stocks in the S&P 500 today, followed back to 2000" is a sample selected on the outcome variable. Name one stock systematically excluded and one systematically included, and say why momentum in particular is exposed.
3. Why is the survivorship bias in a momentum backtest *not* explained by "large caps perform best"? Which factor from ch.6 runs the other way?
4. Name the two ingredients of the fix for survivorship bias. For the second one, give a case where the delisting return is *positive*.
5. Write the three missingness mechanisms with their formal conditions in terms of $P(M\mid Y,X)$. State which fix is valid for each.
6. A dataset has missing delisting returns. Distinguish precisely between "fill with $0\%$" and "assume bankruptcy" — what is a \$0 price as a return? Then explain why conditioning on the delisting *code* converts MNAR into approximately MAR.
7. Classify and justify: (i) ticks dropped in a server outage, (ii) missing prices on zero-volume days with volume recorded, (iii) hedge funds that stop reporting after bad months. For (i), what goes wrong with row deletion in a *time series* specifically?
8. Forward-filling stale prices on a thinly traded NSE counter: name the three distortions it induces, and name the two beta estimators designed to repair the third.
9. Backfill / instant-history bias — explain how it biases a hedge-fund database in the *opposite* direction to attrition, and why both push reported index returns the same way.
10. A stock shows $-50\%$ on the day of a 2-for-1 split. Show algebraically that the true return is $0\%$, and say which stage of the pipeline this belongs to.
11. Compute $P(|Z|>3)$ under normality and convert it to expected days per trading year. Then give the three separate reasons a "delete beyond $3\sigma$" rule is dangerous for financial returns — including what *masking* means and which robust estimator fixes it.
12. Standardising with full-sample $\bar x, s$ before splitting: write the leaked $z$-score explicitly and say exactly what leaked into what. Why is full-sample PCA or feature selection a worse leak than scaling?
13. A random 80/20 shuffle instead of a chronological split. Give the two distinct failure mechanisms (one about the *target*, one about *overlapping features*), and explain why $k$-fold CV from ch.7 needs modifying for time series.
14. Write the correct four-step order of operations for split-and-scale. Which step do people get wrong *after* correctly splitting first?
15. Purging and embargo — how long should the gap between train and test be, and why?
16. Signal from the day-$t$ close, filled at the day-$t$ close. Why is this look-ahead, and why does a cTrader cBot using `Bars.ClosePrices.Last(1)` inside `OnBar()` avoid it automatically?
17. Name the two dates every accounting value needs, state the usage rule connecting them, and explain the Fama-French June convention as a fallback.
18. Using today's 2019 GDP figure in a 2019 backtest is look-ahead on *two* separate counts. Name both. Why are revisions largest exactly where a macro signal matters most?
19. State the reference-time / knowledge-time principle and the single inequality a valid backtest must satisfy. Then show how survivorship bias, look-ahead bias, missing delisting returns and stale data vintages are all violations of that one rule.

---

## 2026-10-02 — Ch.2 data-sets: degrees of freedom, scales, encoding, tidy data, keys

Closed-book recall prompts:

1. Write the two-point system $a+bx_i=y_i$ as a $2\times2$ matrix equation and derive the condition for a unique solution. Why do the $y$ values never appear in that condition, and why does that make the perfect fit worthless?
2. In the matrix form of a regression, what do the rows correspond to and what do the columns correspond to? Why is the first column all 1s, and which parameter is its partner?
3. Fill in the wide / square / tall table from memory: shape, what happens to the fit, and whether the fit is evidence.
4. Define degrees of freedom as $n-k$ and explain in words what a degree of freedom *is* (the "friend with a rule" example).
5. Add $(3,6)$ to the points $(1,3),(2,5)$. Compute the residual of $y=1+2x$ on day 3 with its sign, and use pairwise slopes to show no line fits all three.
6. With 252 daily observations and an intercept plus $p$ features, at what $p$ is a perfect fit guaranteed? Compute df for 30 indicators, then for 35. Which way does adding columns move df, and what is the only way to increase it?
7. What is data snooping, and why does the formula for df overstate the evidence after you choose the best 30 of 100 indicators?
8. Convert a 3-date × 3-asset wide price table to long format. How many rows? Why are the assets not genuinely "features" of a date?
9. In a long-format panel, what is the smallest set of columns that uniquely identifies a row? Why can neither column do it alone?
10. Give the test that distinguishes interval from ratio data, using 30°C vs 15°C converted to Fahrenheit. Which quantity survives the change of units?
11. Why does "gold doubled from \$1,000 to \$2,000" stay true in shillings? What kind of transformation does a ratio scale allow, and what kind does it forbid?
12. Compute the percentage return for WTI going from \$18 to −\$37. Why is the log return undefined, and why is "negate the log of the positive version" a data-destroying fix (what price does it silently substitute)?
13. Explain economically why a futures price can go negative while a gold price cannot. What should you model instead of percentage returns for such series?
14. Write the weekday dummy table with an intercept and show the columns are linearly dependent. Then show algebraically that adding $c$ to $a$ and subtracting $c$ from every day coefficient leaves every prediction unchanged.
15. State the $m-1$ rule. After dropping Monday, interpret $a$ and $b_{\text{Tue}}$ precisely.
16. Coding ratings AAA=1 … B=6 as one column: show that AAA→AA and BBB→BB are forced to the same step. Which scale does this encoding silently impose on an ordinal variable?
17. Write the dummy-encoded yield model with AAA as baseline. How do you read off the BBB→BB step, and what does the extra flexibility cost? Describe the two-column middle ground.
18. Give the two reasons a ticker is a bad primary key, with an example of each, and connect both to a chapter 1 bias.
19. Why store each fact once (normalisation)? What goes wrong if the asset name is repeated in every price row?
20. Design the `fundamentals` table with both reference time and knowledge time, write the as-of query, and explain why this makes look-ahead bias impossible by construction. What is the Python equivalent for aligning trades to quotes?

---

## 2026-10-10 — Ch.3 asset returns: time vs asset aggregation, $R^2/2$, volatility drag, Kelly, distribution shape

Closed-book recall prompts:

1. Gold goes \$2,000 → \$2,200 → \$1,980. Compute both simple returns, show that adding them says "break-even", compute the true two-day return, and explain in one sentence why adding lies.
2. Show that the two log returns sum exactly to the two-day log return. Write the telescoping-sum argument for $T$ periods.
3. Why is a −10% simple move larger in log terms than a +10% move? What round trip *does* return you exactly to the start?
4. Write the Taylor series of $\ln(1+R)$ and derive $R-r\approx R^2/2$. What is the relative error, and why is it negligible for daily data but useless for annual?
5. Why is $r<R$ always, and why is the gap larger for falls than for rises? Which Taylor term is responsible?
6. Two assets, 50/50, +20% and −10%. Show that weighted simple returns give the exact portfolio return and weighted log returns do not. Derive $R_p=w_AR_A+w_BR_B$ from portfolio value.
7. Fill in the time/assets aggregation table from memory. How should a backtest combine positions within a day and then chain days?
8. +50% then −50%: compute ending wealth, arithmetic mean, geometric mean and their gap. Compare the gap with $\sigma^2/2$.
9. State $g\approx\mu-\sigma^2/2$ and derive it from $r\approx R-R^2/2$. Verify on the ±50% example that the mean log return equals $\ln(1+g)$.
10. Give three trading consequences of volatility drag.
11. With $g(L)\approx L\mu-L^2\sigma^2/2$, derive the growth-optimal leverage $L^*$ and $g(L^*)$. At what leverage does compound growth hit zero, and why does it go negative beyond that despite positive expected returns?
12. Why do practitioners trade half-Kelly rather than full Kelly?
13. Write the formulas for skewness and kurtosis. What are their values for a Normal? Why is kurtosis a measure of tail weight?
14. Give a strategy type with negative skew and one with positive skew, and describe what each equity curve feels like.
15. Why are monthly returns closer to Normal than daily ones? What is volatility clustering, and which statistic reveals it when returns themselves look uncorrelated?
16. Write the KDE formula and prove, by substitution, that the $\frac1{nb}$ prefactor makes $\hat f$ integrate to 1.
17. What does the bandwidth control? State Silverman's rule and explain why it takes the minimum of $\hat\sigma$ and IQR$/1.34$.
18. Overlay a KDE of daily returns on a Normal with the same mean and variance. Describe the three regions where they differ.

---

## 2026-10-11 — Ch.4 OLS derivation, normal equations, Q8, BLUE inventory, $\hat\sigma^2$, OLS = MLE

Closed-book recall prompts:

1. What decides whether a task is supervised or unsupervised? Give an example where the *same* task can be either.
2. State SL1–SL4 formally and in words. For each, give a typical way it fails in finance. What is the one-line test for telling SL1, SL3 and SL4 apart?
3. Why is "returns regressed on volume, with news driving both" an SL4 violation? Name the resulting bias.
4. Write $S(\beta_0,\beta_1)$, give three reasons for squaring the misses, and the one cost.
5. Differentiate $S$ with respect to $\beta_0$ and derive $\hat\beta_0=\bar Y-\hat\beta_1\bar X$. State the two consequences of this normal equation.
6. Differentiate with respect to $\beta_1$, substitute $\hat\beta_0$, collect the $\beta_1$ terms and derive the computational form of $\hat\beta_1$. What does the second normal equation say about residuals and $X$?
7. Distinguish the two "sums to zero": $\sum(X_i-\bar X)=0$ and $\sum\hat\epsilon_i=0$. Why is each zero?
8. Work Problem Set Q8 from scratch: the four sums, means, $\hat\beta_1$, $\hat\beta_0$, interpretation of both, and three observations about the plot. What ten-second check catches arithmetic slips?
9. Rewrite $\hat\beta_1$ as $\sum w_iY_i$, prove $\sum w_i=0$ and $\sum w_iX_i=1$, and show $\hat\beta_1=\beta_1+\sum w_i\epsilon_i$. Take expectations, naming the assumption behind each move.
10. Define $v_i$, prove $\sum v_i=1$ and $\sum v_iX_i=0$, and show $\hat\beta_0$ is unbiased.
11. Derive $\operatorname{Var}(\hat\beta_1)=\sigma^2/S_{xx}$ from the double sum, naming which assumption kills the off-diagonal and which equalises the diagonal. Why must the denominator be $\sum(X_i-\bar X)^2$ and not $\sum(X_i-\bar X)$?
12. Derive $\operatorname{Var}(\hat\beta_0)=\sigma^2\left(\frac1n+\frac{\bar X^2}{S_{xx}}\right)$ and explain the two terms as pivot height plus lever arm. What happens when $\bar X=0$?
13. Prove Gauss–Markov for $\hat\beta_1$: unbiasedness conditions on $c_i$, the decomposition $c_i=w_i+d_i$, the cross term $\sum w_id_i=0$ (using both conditions), and the conclusion. Why is $\sum a_ib_i\neq\sum a_i\sum b_i$ relevant?
14. Fill in the assumption inventory table from memory.
15. Derive the residual identity $\hat\epsilon_i=(\epsilon_i-\bar\epsilon)-(\hat\beta_1-\beta_1)(X_i-\bar X)$. Why does substituting $\hat\beta_0$ via $\bar Y$ early make it work?
16. Show $\text{RSS}=\sum(\epsilon_i-\bar\epsilon)^2-S_{xx}(\hat\beta_1-\beta_1)^2$, then $\mathbb E(\text{RSS})=(n-2)\sigma^2$. Explain $n-2$ with the normal equations, and connect it to the $n-1$ in the sample variance.
17. Write the normal log-likelihood and explain why maximising it over $\beta_0,\beta_1$ is identical to OLS. Which assumption is needed for this equivalence?
18. Derive $\hat\sigma^2_{\text{MLE}}=\text{RSS}/n$. Is it biased? In which direction and by how much? What does "asymptotically unbiased" mean?


---

## 2026-10-14 — MLR: unbiasedness and $\text{Var}(\hat\beta)=\sigma^2(X'X)^{-1}$

Closed-book recall prompts:

1. Derive $\hat\beta=\beta+(X'X)^{-1}X'\varepsilon$ from the OLS formula, naming the assumption behind the substitution and the one behind the collapse to $I_{p+1}$. What size is that identity?
2. Take $E(\cdot\mid X)$ and prove unbiasedness. Why can $(X'X)^{-1}X'$ come out of the expectation, and on which side?
3. Order independence, mean independence and orthogonality from strongest to weakest. Which one does the proof need? Give a counterexample where orthogonality holds but $E(\varepsilon\mid X)\ne0$.
4. Why is $E[(\hat\beta-\beta)(\hat\beta-\beta)'\mid X]$ the variance, and which earlier result does that rely on?
5. Compute $A'$ for $A=(X'X)^{-1}X'$. Prove $(M^{-1})'=(M')^{-1}$ and that $X'X$ is symmetric.
6. Write $E(\varepsilon\varepsilon'\mid X)$ as an $n\times n$ matrix. Which assumption sets the diagonal to $\sigma^2$, and which sets the off-diagonal to 0? Why is $\varepsilon\varepsilon'\ne\sum\varepsilon_i^2$?
7. Finish the cancellation to $\sigma^2(X'X)^{-1}$. What do the diagonal and off-diagonal entries of the result mean?
8. Which assumption is missing from the lecturer's variance proposition? What does the formula become without it?

---

## 2026-10-15 — §2.1.8 Cramér–Rao and the information matrix

Closed-book recall prompts:

1. Define $p$, $p+1$, $p+2$ and $n$ in the MLR setting.
2. For $Y_i=\mu+\varepsilon_i$ ($\sigma^2$ known), write $l(\mu)$, compute $l''$ and the information, and explain "peak shift = tilt ÷ steepness". Why is $1/\mathcal I$ the variance floor?
3. Why the minus sign, and why the $E$, in $\mathcal I=-E(H)$? In which block of the MLR Hessian does the $E$ actually do work, and why?
4. Run the intercept-only regression through $\sigma^2(X'X)^{-1}$. What do you get, and what does it show about $X'X/\sigma^2$?
5. Derive all four Hessian blocks and their $-E$. For the cross block and the $\sigma^2\sigma^2$ block, name the exact assumption used. Use the count-the-$\varepsilon$'s rule.
6. Why does $E(\varepsilon'\varepsilon\mid X)=n\sigma^2$ need neither no-autocorrelation nor normality?
7. Invert $\mathcal I_n$ block by block, verify by multiplication, and state what the top-left block proves.
8. CR efficiency vs BLUE: what does each compete against, and what does each assume?

---

## 2026-10-16 — BLUE (matrix Gauss–Markov)

1. State BLUE and its assumptions. Which two assumptions does it **not** need that Cramér–Rao does?
2. What does $\text{Var}(\tilde\beta)\ge\text{Var}(\hat\beta)$ mean for matrices, and what does it guarantee about $c'\beta$?
3. Split $C=A+D$ and derive the condition $DX=0$ from unbiasedness. Why must it hold for every $\beta$?
4. Show $\text{Var}(\tilde\beta\mid X)=\sigma^2CC'$, expand it, kill the cross terms, and simplify $AA'$. Why can't $AD'+DA'$ be written as $2AD'$?
5. Prove $DD'$ is PSD and that OLS is the **unique** BLUE.
