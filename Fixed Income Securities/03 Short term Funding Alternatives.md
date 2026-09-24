These lecture slides cover the foundational concepts of **Short-Term Funding Alternatives** with a focus on the mechanics, formulas, and quantitative application of **Repurchase Agreements (Repos)**.

## 1. Short-Term Funding Alternatives

Institutions and corporations leverage several instruments to manage short-term liquidity:

- **Retail Deposits:** Traditional bank deposits from individual consumers.

- **Short-Term Wholesale Funds:** Includes highly liquid interbank instruments such as reserve funds, interbank funds, and large-denomination Certificates of Deposit (CDs).

- **Repurchase (Repo) & Reverse Repurchase Agreements:** Collateralized short-term lending arrangements categorized by duration into **overnight repo**, **term repo**, and **repo to maturity**.


## 2. Core Repo Mathematical Formulas

A repo functions as a collateralized loan where a security is sold today with a simultaneous agreement to buy it back later at a higher price. The financial parameters are structured as follows:

$$\text{Initial Margin} = \frac{\text{Security Price}}{\text{Purchase Price}}$$

$$\text{Purchase Price} = \frac{\text{Security Price}}{\text{Initial Margin}}$$

$$\text{Repo Haircut} = \frac{\text{Security Price} - \text{Purchase Price}}{\text{Security Price}}$$

$$\text{Repurchase Price} = \text{Purchase Price} \times \left[1 + \left(\frac{\text{Repo Rate} \times \text{Repo Term}}{\text{Days in a Year}}\right)\right]$$

## 3. Step-by-Step Solutions to the Examples

### **Example 1: Calculating the Repo Haircut**

**Given Data:**

- Security Price (5-year Kenya Treasury bond at face value) = **KES 1,000,000**

- Initial Margin = **104% (1.04)**


**Step 1: Calculate the Purchase Price.**

$$\text{Purchase Price} = \frac{\text{KES } 1,000,000}{1.04} = \text{KES } 961,538.46$$

**Step 2: Calculate the Repo Haircut.**

$$\text{Repo Haircut} = \frac{1,000,000 - 961,538.46}{1,000,000} = \frac{38,461.54}{1,000,000} \approx 0.03846 \text{ or } \mathbf{3.85\%}$$

_(Alternatively, it can be derived directly from the margin: $1 - \frac{1}{1.04} \approx 3.85\%$)_

### **Example 2: Calculating the Repurchase Price**

**Given Data:**

- Security Price (5-year Kenya Treasury bond at face value) = **KES 10,000,000**

- Initial Margin = **105% (1.05)**

- Repo Term = **20 days**

- Days in a Year (Count Convention) = **360 days**

- Annual Repo Rate = **0.80% (0.008)**


**Step 1: Calculate the upfront Purchase Price (loan principal disbursed).**

$$\text{Purchase Price} = \frac{\text{KES } 10,000,000}{1.05} = \text{KES } 9,523,809.52$$

**Step 2: Compute the Repurchase Price at maturity ($t = 20$).**

$$\text{Repurchase Price} = 9,523,809.52 \times \left[1 + \left(\frac{0.008 \times 20}{360}\right)\right]$$

$$\text{Repurchase Price} = 9,523,809.52 \times \left[1 + 0.00044444\right]$$

$$\text{Repurchase Price} = 9,523,809.52 \times 1.00044444 = \mathbf{KES\ 9,528,042.33}$$