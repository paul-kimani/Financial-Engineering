# CAPM and Multifactor Models

## 1. The Capital Asset Pricing Model (CAPM)

CAPM is an **economic equilibrium model** — it isn't just a regression, it's a theory about how risky assets *should* be priced in a market at equilibrium. Practically, though, it reduces to a **Simple Linear Regression** (see [[04 Supervised Learning Vs unsupervised learning.|ch.4]]):

$$R_i = R_f + \beta_i(R_m - R_f) + \epsilon_i$$

where:
- $R_i$ — return on asset $i$
- $R_f$ — the risk-free rate
- $R_m$ — return on **the market portfolio**: the (theoretical) portfolio of *all* risky assets, weighted by market capitalization
- $\beta_i$ — asset $i$'s sensitivity to market-wide moves (systematic risk)
- $\epsilon_i$ — idiosyncratic (asset-specific) risk, uncorrelated with the market

This equation is sometimes called the **characteristic line** of the asset — plot $R_i - R_f$ (y-axis) against $R_m - R_f$ (x-axis) and $\beta_i$ is literally the slope.

### Estimating $\beta_i$

$$\beta_i = \frac{\operatorname{Cov}(R_i, R_m)}{\operatorname{Var}(R_m)}$$

This is exactly the OLS slope formula for SLR — CAPM's $\beta$ **is** a regression coefficient with an economic interpretation layered on top: $\beta_i>1$ means the asset amplifies market moves (aggressive), $\beta_i<1$ means it dampens them (defensive).

CAPM is a **single-factor model** — the market itself, $R_m$, is the only systematic risk factor.

## 2. Implementing CAPM in R (from first principles vs. `lm`)

The R workflow from Lecture One builds a CAPM-style estimate two ways and compares them:

```r
library(quantmod)   # to download stock data
stock_namelist <- c("AAPL","GOOG","AXP","GM","PFE","XOM")
data_set <- xts()
for (i in 1:length(stock_namelist)) {
  tmp <- Ad(getSymbols(stock_namelist[i], from="2018-01-02", to="2026-08-17", auto.assign=FALSE))
  tmp <- na.approx(tmp, na.rm=FALSE)     # interpolate NAs
  data_set <- cbind(data_set, tmp)
}
colnames(data_set) <- stock_namelist

SP500_index <- Ad(getSymbols("^GSPC", from="2018-01-02", to="2026-08-17", auto.assign=FALSE))

# log returns of the stocks, and of the market factor
X <- diff(log(data_set), na.pad=FALSE)     # see [[03 Asset Returns.]]
f <- diff(log(SP500_index), na.pad=FALSE)  # market factor
```

**Method 1 — first principles (method of moments):**

```r
beta  <- cov(X,f)/as.numeric(var(f))
alpha <- colMeans(X) - beta*colMeans(f)
```

This is literally the $\beta_i=\operatorname{Cov}(R_i,R_m)/\operatorname{Var}(R_m)$ formula from §1, applied column-wise, plus the corresponding $\alpha_i = \bar R_i - \beta_i\bar R_m$ (the CAPM intercept — in theory $\alpha=0$; a nonzero, statistically significant $\alpha$ is a sign of consistent out/under-performance relative to what CAPM predicts).

**Method 2 — `lm()` (OLS regression), for comparison:**

```r
fit1 <- lm(X ~ f)
summary(fit1)
```

The point of doing both: they should (and do, in the lecture's output) recover essentially the same $\alpha,\beta$ estimates, since Method 1 is nothing but the closed-form OLS solution computed by hand instead of via `lm()`. Observed betas ranged from AAPL $\beta\approx1.19$ (aggressive) down to PFE $\beta\approx0.56$ (defensive) — pharma names tend to be more defensive/less market-sensitive than tech.

### Idiosyncratic risk and the factor covariance structure

```r
sigma2 <- rep(NA, N)
for (i in 1:N) {
  eps_i <- X[,i] - alpha[i] - beta[i]*f
  sigma2[i] <- (1/(T-2)) * t(eps_i) %*% eps_i     # idiosyncratic variance estimate
}
Psi   <- diag(sigma2)                              # diagonal matrix of idiosyncratic variances
Sigma <- as.numeric(var(f)) * beta %*% t(beta) + Psi   # total covariance = systematic + idiosyncratic
```

This $\Sigma = \sigma_m^2\,\beta\beta^\top + \Psi$ decomposition is the single-factor analogue of the covariance structure used throughout portfolio theory: **total risk = systematic risk (shared, market-driven) + idiosyncratic risk (diversifiable, asset-specific)**. This is precisely why diversification works — $\Psi$ is diagonal (idiosyncratic risks are assumed uncorrelated across assets), so spreading across many assets averages out the $\Psi$ contribution while the $\beta\beta^\top$ systematic term doesn't shrink the same way.

## 3. Portfolio weights (brief note)

For a portfolio of $n$ assets with weight vector $w=(w_1,\dots,w_n)^\top$:

$$\sum_i w_i = 1, \qquad w_i \in \mathbb{R}$$

A **no-short-selling** constraint adds $w_i \ge 0\;\forall i$. This is the same feasible-region idea as the primal LP constraints in [[03 - Duality of Linear Programming|the Optimization unit]] — portfolio optimization (Markowitz mean-variance, not detailed further in this unit yet) is itself a quadratic program subject to exactly this kind of constraint set.

## 4. Multifactor models

CAPM's single-factor assumption is restrictive — real returns respond to more than just "the market." **Multifactor models** generalize CAPM to $p$ factors:

$$R_i = \alpha_i + \beta_{i1}f_1 + \beta_{i2}f_2 + \dots + \beta_{ip}f_p + \epsilon_i$$

where the factors $f_1,\dots,f_p$ have $\mathbb{E}(f_j)$ generally nonzero (unlike $\epsilon_i\sim(0,\sigma^2)$ with $\operatorname{Cov}(\epsilon_i,f_j)=0$). Factors fall into two broad families:

- **Macroeconomic factors:** inflation, interest rates, FX rates — factors external to any specific company.
- **Fundamental factors:** debt ratio, liquidity ratio, profitability — factors derived from a company's own financial statements.

This is structurally identical to the jump from [[04 Supervised Learning Vs unsupervised learning.|SLR]] to [[05 Multiple Linear Regression and the F-test|multiple regression]] — CAPM *is* the multifactor model's $p=1$ special case, with the market return as the sole factor.

## 5. The Fama-French 3-Factor Model

The best-known concrete multifactor model, extending CAPM with two more factors:

$$R_i = R_f + \beta_{1i}(R_m-R_f) + \beta_{2i}\,\text{SMB} + \beta_{3i}\,\text{HML} + \epsilon_i$$

- $R_m - R_f$ — the market factor (same as CAPM)
- **SMB** ("Small Minus Big") — the size factor: historically, small-cap stocks have outperformed large-cap stocks
- **HML** ("High Minus Low") — the value factor: how much a stock's market value trades relative to its book value (high book-to-market = "value" stocks vs. low = "growth" stocks)

> [!note] Beyond 3 factors
> The lecture notes also flag a **5-factor extension** adding a liquidity factor — not detailed further yet in this unit's material, but worth knowing the name exists if it comes up.

### Implementing Fama-French in R

```r
mydata <- read.csv("F-F_Research_Data_Factors_daily.CSV", skip=4)
mydata <- mydata[-nrow(mydata), ]      # remove last row (footer)
fama_lib <- xts(x=mydata[,c(2,3,4)], order.by=as.Date(paste(mydata[,1]), "%Y%m%d"))
# columns: Mkt.RF, SMB, HML

F  <- fama_lib[index(X)]/100           # align dates with stock data, convert % to decimal
F_ <- cbind(ones=1, F)                 # design matrix with intercept column

# closed-form OLS via normal equations: Gamma = (F'F)^{-1} F'X
Gamma <- t(solve(t(F_) %*% F_, t(F_) %*% X))
colnames(Gamma) <- c("alpha","b1","b2","b3")

fit2 <- lm(X ~ F)   # cross-check via lm()
summary(fit2)
```

Same "compute by hand, then verify with `lm()`" pattern as the CAPM section — `Gamma <- (F'F)^{-1}F'X` is the multiple-regression normal-equations solution from [[05 Multiple Linear Regression and the F-test|ch.5]], applied to all 6 stocks simultaneously via matrix algebra instead of one `lm()` call per asset.

**Reading the output:** in the lecture's fitted results, market-factor loadings (`FMkt.RF`) stayed close to the single-factor CAPM betas (e.g. AAPL $\approx1.15$), while SMB and HML loadings varied a lot by stock — e.g. AXP's HML coefficient ($\approx0.75$, highly significant) says AXP behaves much more like a "value" stock than AAPL or GOOG (both near-zero, insignificant HML loadings) — consistent with AXP being a more mature financial-services firm vs. two high-growth tech names.

## 6. Cheat sheet

| Model | Factors | Equation |
|---|---|---|
| CAPM | 1 (market) | $R_i = R_f + \beta_i(R_m-R_f)+\epsilon_i$ |
| Fama-French 3-factor | 3 (market, size, value) | $R_i = R_f+\beta_1(R_m-R_f)+\beta_2\text{SMB}+\beta_3\text{HML}+\epsilon_i$ |
| General multifactor | $p$ | $R_i = \alpha_i + \sum_{j=1}^p \beta_{ij}f_j + \epsilon_i$ |

## 7. Open questions for revision

- [ ] Re-derive $\beta=\operatorname{Cov}(R_i,R_m)/\operatorname{Var}(R_m)$ from the OLS slope formula for SLR, to see explicitly why CAPM's beta *is* a regression coefficient.
- [ ] Given the AXP HML result above, form your own hypothesis for why GM (an industrial/auto name) might load differently on SMB vs. HML than AAPL/GOOG — then check against the fitted `Gamma` table.
- [ ] Look up what makes the 5-factor Fama-French model's added liquidity factor different from HML/SMB.

---
*Source: BSF 3216 Lecture One (R implementation) + BSF 3216 lecture, 7 Sept 2026 (CAPM/multifactor theory, handwritten).*
