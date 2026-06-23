##### Example 1
*Consider a one year quarterly paying USD/KES swap. The spot exchange rate is KES 100 per USD. Principal is $1,000.*

| Spot Rates | USD | KES |
| :--------- | --- | --- |
| 90 day     | 5%  | 10% |
| 180 day    | 6%  | 11% |
| 270 day    | 7%  | 12% |
| 360 day    | 8%  | 13% |
***Required** : get the swap rate*

**Solution**
I wont repeat the calculation of the discount factors for USD as they were calculated in [[01 Introduction]].

Hence we have:

| Discount Factors |
| :---------------  |
| 0.9877           |
| 0.9709           |
| 0.9501           |
| 0.9259           |
$$ S= \frac{1-Z_n}{\sum_{i=1}^nZ_i} \times f $$
$$ S = \frac{1-0.92593}{3.83457} = 0.01932$$
$$0.01932 \times \frac{320}{90} = 7.73\%$$
For KES:

| Discount Factors                         |
| :--------------------------------------- |
| $(1+0.1(\frac{90}{360}))^{-1}=0.97461$   |
| $(1+0.11(\frac{180}{360}))^{-1}=0.94787$ |
| $(1+0.12(\frac{270}{360}))^{-1}=0.91743$ |
| $(1+0.13)^{-1}=0.8850$                   |
Also:
$$\sum_{i=1}^nZ_i = 3.72591$$
Hence:
$$\frac{1-0.8850}{3.72591}=0.030877$$
$$3.0877\% \times \frac{360}{90}=12.35\%$$
lets look at its valuation 60 days into the contract as an illustration.
The spot rates 60 days into the contract are:

| Spot rates | USD | KES |
| :--------- | --- | --- |
| 30 day     | 6%  | 11% |
| 120 day    | 7%  | 12% |
| 210 day    | 9%  | 14% |
| 300 day    | 10% | 15% |
And now the exchange rate is KES 105 per USD.

**Required**: Calculate the value of the Pay KES fixed receive USD floating swap.

**Solution**
Because the USD leg is floating, we don't need to project and discount all future cash flows. We use the "Floating Rate Note Shortcut": immediately after a reset date, a floating bond is worth par.

Since 60 days have passed, we are between reset dates. The last reset was at Day 0, where the USD spot rate was **5%**.
- The next payment is based on this initial 5% rate, therefore:
$$0.05 \times \frac{90}{360} \times \$1000 = \$12.50$$
 - at day 90 the bond resets to $1000 hence the value just before the interest payment will be:
 $$\$12.50 + \$1000 = \$1012.5$$
 - We then discount it to the $60^{th}$ day which will be:
 $$\frac{\$1012.50}{(1+0.06(\frac{30}{360}))} = \$1007.46$$
For the KES leg:
We need to first calculate the first fixed interest which will be:
$$KES100000 \times 3.0877\% = KES3087.70$$
The discounting factors will be:

| Discounting Factor                        |
| :---------------------------------------- |
| $(1+0.06(\frac{30}{360}))​^{-1}=0.99502$  |
| $(1+0.07(\frac{120}{360}))​^{-1}=0.96154$ |
| $(1+0.09(\frac{210}{360}))​^{-1}=0.92450$ |
| $(1+0.10(\frac{300}{360}))​^{-1}=0.88889$ |


$$PV_{KES}​=(3087.5×Z_{30​})+(3087.5×Z_{120}​)+(3087.5×Z_{210}​)+(103087.5×Z_{300}​)$$
$$PV_{KES}​=3059.47+2968.75+2854.40+91633.34$$
$$PV_{KES}​=100,515.96~KES$$

*(Note: Adding the par value to that final 300-day payment is critical to closing out the principal exchange).*

The mark to market value is given by:

$$V_{swap} = PV_{KES} - (PV_{USD} \times S_0)$$
This is described in [[04 Currency Swaps - Introduction]]
Hence:
$$V_{swap}​=105,783.30−100,515.96 = KES~5267.34$$

[[04 Currency Swaps - Introduction]].     $\leftarrow$ Previous  Next $\rightarrow$ [[06 Currency Swaps Assignment Solutions]]
