After the first day, the rates change from time to time, this means depending on the side long and short, the swap, there is a possiblity of either benefiting or making a loss on the swap.

This directly translates to the swap changing in value. Henceforth, we calculate the value of the swap in monetary terms, i.e..,
$$
V_{swap}=PV_{floating}-PV_{fixed}
$$
*Example*
*From the example on [[01 Introduction]], we go 50 days into the contract, we have the following interest rates as at that date:*

| Period   | Rate |
| :------- | ---- |
| 40 days  | 7%   |
| 130 days | 7%   |
| 220 days | 8%   |
| 310 days | 8%   |
*Determine the value of the pay fixed receive floating swap*

**Soln**
Here, we know that:
$$
V_{swap}=PV_{floating}-PV_{fixed}
$$
Let's start with the simpler one which will be the fixed interest side:

Hence we calculate the new discount factors
$$
\frac{1}{(1+0.07 \times \frac{40}{360})} =0.99228
$$
$$
\frac{1}{(1+0.07 \times \frac{130}{360})}=0.97534
$$
$$
\frac{1}{(1+0.08 \times \frac{220}{360})} =0.95339
$$
$$
\frac{1}{(1+0.08 \times \frac{310}{360})} =0.93555
$$

and we therefore have:

$$
PV_{fixed} = 19.3(0.99228) + 19.3(0.97534) + 19.3(0.95339) + 1019.3(0.93555) = \$1009.98
$$
To value the floating leg, we need to remember one distinct concept.
For a floating rate note or bond, the price resets to par after every payment of interest.
We simply need to look at the very next payment.
The rate for the current 90-day period was already locked in on Day 0.
so the next floating payment is:(with regards to the prevailing spot rate of 5% at the time.)
$$1000×0.05×(90/360)=$12.50$$
The value of the floating leg today is simply the par value plus this known payment, discounted by the 40-day factor:
$$PV_{floating} = (1000 + 12.50) \times Z_{40}$$
$$PV_{floating} = 1012.50 \times 0.99228 = \$1004.68$$
Hence the final Mark-to-Market Value becomes:
$$Value = PV_{floating} - PV_{fixed}$$
$$Value = 1004.68 - 1009.98 = -\$5.30$$
The swap currently has a negative value of $-\$5.30$

[[01 Introduction]] $\leftarrow$ Previous.                  Next $\rightarrow$ [[03 Interest Rate Swap Terminology & Risk Profiles]]

