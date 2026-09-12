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
