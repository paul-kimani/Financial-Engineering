**Fundamental Law of Active Management** (originally formulated by Richard Grinold and Ronald Kahn).

  
It breaks down a portfolio manager's performance—measured by the **Information Ratio ($\text{IR}$)**—into two distinct drivers: forecasting skill and trading frequency/diversity.

### Key Concepts Breakdown

#### 1. Information Ratio ($\text{IR}$)

$$\text{IR} = \frac{R_p - R_b}{\sigma_{\text{active}}}$$

- **What it is:** The ratio of active return (portfolio return $R_p$ minus benchmark return $R_b$) to active risk/tracking error ($\sigma_{\text{active}}$).
- **Meaning:** It evaluates how effectively a manager generates alpha per unit of risk taken relative to a benchmark. 

#### 2. The Grinold Approximation (Fundamental Law)

$$\text{IR} \approx \text{IC} \times \sqrt{BR}$$

This breaks performance into two core components:
- **Information Coefficient ($\text{IC}$) — Skill/Quality:**
    - Measures the skill of the manager in forecasting asset returns.
    - Mathematically, it is the correlation (often rank correlation) between predicted returns and actual realized returns across assets.
    - Range: $[-1, 1]$. An $\text{IC}$ of $0.05$ to $0.10$ is considered quite strong in quantitative equity strategies.
- **Breadth ($BR$) — Frequency & Scale:**
    - The number of **independent** investment decisions (or "bets") made per year.
    - Merely trading 1,000 times a day on highly correlated assets does not yield a $BR$ of 1,000 because those bets are interdependent. True breadth requires cross-sectional or intertemporal independence.

### Core Takeaway
To double your Information Ratio ($\text{IR}$), you have two choices:
1. **Improve Forecast Accuracy ($\text{IC}$):** Refine models or acquire better data to double prediction quality (difficult to achieve).
2. **Increase Breadth ($BR$):** Quadruple the number of independent trade signals (e.g., expanding from 50 stocks to 200 stocks, or moving to higher-frequency decision cycles) because $BR$ enters under a square root ($\sqrt{BR}$).