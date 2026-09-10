# Applied Analytics For Finance (BSF 3216)

> Data preparation and statistical/ML modeling for financial data —
> from raw datasets through regression, factor models, resampling,
> regularization, and classification, building toward tree methods and
> neural networks.

---

## How to read these notes

Chapters are numbered in study order. Each is a self-contained reference
file. [[Working Notes]] is a single running scratch file for active-recall
writing — not split per chapter.

| #  | Chapter | Status |
| -: | :--- | :--- |
| 01 | [[01 The Data Life Cycle.]] — raw → cleaned → prepared data, EDA, wrangling vs. preprocessing | Core |
| 02 | [[02 Data-sets]] — dataset anatomy, structured/semi/unstructured data, relational databases | Core |
| 03 | [[03 Asset Returns.]] — simple vs. log returns, why log returns, KDE link | Core |
| 04 | [[04 Supervised Learning Vs unsupervised learning.]] — supervised/unsupervised, SLR model & assumptions | Core |
| 05 | [[05 Multiple Linear Regression and the F-test]] — SSR/TSS/SSReg, F-statistic, R² & adjusted R² | Core |
| 06 | [[06 CAPM and Multifactor Models]] — CAPM, beta, Fama-French 3-factor, R implementation | Core |
| 07 | [[07 Model Assessment, Bias-Variance and Resampling]] — bias-variance tradeoff, classification metrics, CV, bootstrap | Core |
| 08 | [[08 Regularization - Ridge and Lasso]] — shrinkage, Ridge vs. Lasso, worked toy examples | Core |
| 09 | [[09 Logistic Regression and Classification]] — logistic/logit, MLE, Naive Bayes | Core |
| 10 | [[10 Support Vector Machines]] — hyperplanes, maximal margin, soft margin, kernel trick | Core |
| 11 | [[11 Neural Networks (stub)]] — placeholder, pending lecture | Not yet written |

Standalone reference note: [[Kernel Density Function]] (linked from ch.3 and ch.9).

---

## Datasets referenced in this unit

Not written up as standalone chapters (they're inputs, not concepts) — noted here for quick recall of what each is and where it's used:

- **NSE 20 Share Index** — daily price series, used in Lecture One's intro to loading/plotting price data before computing returns.
- **WeekInt.txt** — weekly interest-rate series (`aaa_dif`, `cm10_dif`, `cm30_dif`, `ff_dif`) — used in Lecture One for simple/multiple regression practice (e.g. `lm(aaa_dif ~ cm10_dif)`, R²≈0.746) before moving to the CAPM/Fama-French material in [[06 CAPM and Multifactor Models]].
- **F-F_Research_Data_Factors_daily.CSV** — Fama-French daily factors (Mkt.RF, SMB, HML) — the raw data behind [[06 CAPM and Multifactor Models#5. The Fama-French 3-Factor Model|ch.6 §5]].

---

## Notational conventions

- Log return: $r_t=\ln(P_t/P_{t-1})$ — see [[03 Asset Returns.]].
- Regression response is always $Y$; predictors $X_1,\dots,X_p$; parameters $\beta_0,\dots,\beta_p$.
- CAPM/factor-model notation uses $R$ for asset/factor returns, $\beta$ for factor loadings (context distinguishes this from the regression $\beta$ above).
- Classification labels: $\{0,1\}$ for logistic regression/Naive Bayes, $\{-1,+1\}$ for SVM (different conventions, same underlying idea — noted explicitly in [[10 Support Vector Machines]]).
