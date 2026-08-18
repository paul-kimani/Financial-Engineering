From my understanding of #swaps, its essentially layering or a series of futures contracts.

>In essence a swap is an over the counter agreement between 2 companies or parties to exchange cashflows in the future.

## Interest Rate Swaps
In this case, the idea is that one of the parties takes the cashflows at some fixed rate to be determined and the other party gets the cashflows at some floating rate, e.g.., **LIBOR**.

Initially, since there is no premium paid on the swap, we value it at the fixed interest rate.
That is, the initial value is a rate, not an actual value, since both parties have to start at some neutral point, i.e..,$PV_{fixed} = PV{floating}$

Here, we solve for the interest rate by setting our $NPV$ to zero.

*For Example*:
*Consider a 1 Year quartely paying interest rate bond with a notional pay of $1000. The floating rates are based on 90-day spot rates of the MRR. Find the no-arbitrage price of the swap*

| Period   | Spot Rates |
| -------- | ---------- |
| 90 days  | 5%         |
| 180 days | 6%         |
| 270 days | 7%         |
| 360 days | 8%         |

**Solution**
We need to first calculate the discount factors for the corresponding spot rates. Hence we have:

| Discount Factors                   |
| :--------------------------------- |
| $(1 + 0.05(90/360))^{-1} = 0.9877$ |
| $(1 + 0.06(180/360))^{-1}=0.9709$  |
| $(1 + 0.07(270/360))^{-1}=0.9501$  |
| $(1 + 0.08)^{-1}=0.9259$           |

The standard #no-arbitrage formula for the annualized fixed swap rate S is:
$$
S= \frac{1-Z_n}{\sum_{i=1}^nZ_i} \times f
$$
Where:

- $Z_n$​ is the discount factor for the final maturity (Period 4).
- $\sum_{i=1}^nZ_i$ is the sum of all the discount factors (often called the PV01 or the annuity factor). 
- $f$ is the payment frequency per year (in this case, f=4 for quarterly).
Calculating the numerator, we have:
$$
1-Z_n = 1 - 0.9259 = 0.0741
$$
Hence, we have:
$$
S_{periodic}= \frac{0.0741}{3.8346}=0.019324
$$We then annualize by multiplying the payment frequency

$$
S_{annualized} = 0.019324 \times 4 = 0.0773 = 7.73\%
$$
This gives us our swap rate/fixed rate as 7.73%

Now, lets assume that the swap rate is 9%, what is the arbitrage strategy?

Next $\rightarrow$ [[02 Value of an Interest rate swap]]
