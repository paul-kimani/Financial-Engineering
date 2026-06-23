The assignment was to solve the 4 possible scenarios of all the possible legs,i.e..,

| KES      | USD      |
| :------- | -------- |
| Fixed    | Floating |
| Fixed    | Fixed    |
| Floating | Fixed    |
| Floating | Floating |
From [[05 Example on Currency Swaps]] we already have 2 legs of the currencies, that is, USD at floating ($USD_{floating}$) and KES at fixed ($KES_{fixed}$)
I will start with the simplest which will be KES at floating, since we just need to reset the value to the initial par value after any interest payment.
We will have :

The first payment from the floating side at day 90 is:
$$100,000 \times 0.10\times\frac{90}{360} = KES~2,500 $$
The value of the "bond" resets to par at the payment time to $KES 100,000$
Hence : at day 90:
$$Total = 100,000 + 2,500 =KES~102,500$$
to find $PV_{KES_{floating}}$ we discount this amount to day 60, that is, 30 days before the payment.
The discounting factor is : $(1+0.11(30/360))^{-1} = 0.99092$

$$PV_{KES(Floating)}​=102,500×0.99092=101,569.30~KES$$
Our Building blocks are:
- Receive USD floating: $\$1007.46$ see $\rightarrow$ [[05 Example on Currency Swaps]]
- Pay KES fixed: $\frac{100,515.96}{105} = \$ 957.30$ 
- Receive USD fixed: $\$997.37$
- Pay KES floating: $\frac{101569.30}{105} = \$967.33$
Now we can get the swap value for all the conditions:

##### Fixed KES and Floating USD
$$V_{swap} = PV_{USD(floating)} - PV_{KES(fixed)}$$
$$V_{swap} = \$1007.46 - \$957.30 = \$50.16$$
##### Fixed KES and Fixed USD (standard currency swap)
$$V_{swap} = PV_{USD(fixed)} - PV_{KES(fixed)}$$
$$V_{swap} = \$997.37 - \$957.30 = \$40.07$$
##### Floating KES and Fixed USD
$$V_{swap} = PV_{USD(fixed)} - PV_{KES(floating)}$$
$$V_{swap} = \$997.37 - \$967.33 = \$30.04$$
##### Floating KES and Floating USD (cross-currency Basis Swap)
$$V_{swap} = PV_{USD(floating)} - PV_{KES(floating)}$$
$$V_{swap}=\$1007.46 - \$967.33=\$40.13$$
### The Quantitative Takeaway

Looking at the four scenarios side-by-side, you can immediately see the isolated impact of the different risk factors.

Notice how Scenario 1 is the most profitable (+$50.14). Why? Because you hold the best possible combination for the current market environment: you are receiving floating USD (benefiting from the USD rates going up) while paying fixed KES (protecting yourself from the KES rates going up), all while the FX rate moved heavily in your favor!
