A standard currency swap (often fixed-for-fixed) follows a strict three-phase timeline. Let’s use a practical example: A quantitative firm in Nairobi needs U.S. Dollars (USD) to pay for international server infrastructure, while a foreign firm needs Kenyan Shillings (KES) to fund local operations.
- **Phase 1: Initial Exchange of the Principal(Day 0)**:
	 Unlike the IRS described in [[01 Introduction]], [[02 Value of an Interest rate swap]] and [[03 Interest Rate Swap Terminology & Risk Profiles]], for currency swaps, the principal amount is exchanged at inception using the prevailing spot exchange rates.If the spot rate is 130 KES per 1 USD, the Nairobi firm might hand over 130 million KES and receive $1 million USD. Remember that at day 0 the value of the swap(NPV) is still exactly 0, as explained in [[01 Introduction]].
- **Phase 2: The Periodic Interest Payments**
	 Over the life of the swap, each party pays interest on the currency they received.The Nairobi firm pays a fixed USD interest rate on the $1 million. The foreign firm pays a fixed KES interest rate on the 130 million KES. These payments are _not_ netted out because they are in different currencies; both transactions occur fully.
- **Phase 3: Final Re-exchange of Principal(Maturity)**
	 At the end of the contract, the principal amounts are swapped back at the original exchange rates used, regardless of what the spot market is doing on that day.

Swaps are solely driven by the idea of **comparative advantage**.

Due to factors like banking relationships, tax structures and credit ratings, it is usually cheaper for companies to secure loans in form of the domestic currency. From that perspective, it is possible for the firm to borrow in its locally and use a currency swap to convert it into foreign debt and through this the firm effectively gets a better foreign exchange rate than the one it could have gotten if it went directly to the Foreign bank.

#### Valuation
When it comes to valuation if the currency swap we take an approach that assumes that we are long a bond in one currency and short another bond in another currency.
If you are the Nairobi firm (receiving KES, paying USD), the value of the swap in your base currency (KES) is the present value of the KES cash flows minus the present value of the USD cash flows, converted at today's spot rate.

The formula for getting the value of the swap becomes:
$$
V_{swap} = PV_{KES} - (PV_{USD} \times S_0)
$$
Where:
- $PV_{KES}$ : Is the Present Value of the KES principal and interest you will receive, discounted using the KES zero curve.
- $PV_{USD}$ : Present value of the USD principal and interest you must pay, discounted using the USD (SOFR) zero curve.
- $S_0$ : The _current_ Spot FX rate (expressed as KES per USD).

### Risk Profile of Currency Swaps
Because the final principal exchange is mandatory, currency swaps carry massive exposure to FX shifts. If the Kenyan Shilling depreciates heavily against the Dollar over the life of the swap (e.g., moves from 130 to 150), $S_0$​ increases. That makes the USD leg you have to pay back much more expensive in domestic terms, driving the $V_{swap}$​ **deeply negative for your side of the book.**

[[03 Interest Rate Swap Terminology & Risk Profiles]] $\leftarrow$ Previous   Next $\rightarrow$ [[05 Example on Currency Swaps]]

