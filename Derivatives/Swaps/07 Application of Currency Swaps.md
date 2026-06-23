Here, we shift the focus to **application and hedging strategy**.

Here is a breakdown of what is happening in both scenarios, validating your notes and explaining the underlying quantitative logic.

### Liability Management & The Dealer Spread

We have an example that illustrates a classic **Cross-Currency Swap with a Financial Intermediary**.

Two companies have borrowed in currencies that do not match their operational revenues, creating massive FX risk on their balance sheets.
- **The US Company:** Borrowed £1,000,000 at a fixed 8%. They want to pay floating USD.
- **The Italian Company:** Borrowed $1,500,000 at a floating MRR (Market Reference Rate). They want to pay fixed Euros/Pounds. _(Note: Your symbol transitions from £ at the top to € at the bottom, but the math holds perfectly either way!)_
- **The Spot Rate:** $1.50 per £. This is the crucial glue. Notice that £1,000,000 $\times$ 1.50 = $1,500,000. The notionals perfectly match, allowing a clean swap.

**The Swap Dealer's Role:**

The example perfectly captures how a bank's swap desk makes money. The dealer doesn't take on market risk; they take on _counterparty credit risk_ and earn a spread.

- The dealer receives **8.1%** from the Italian company.
- The dealer pays out **8.0%** to the US company.
- The floating MRR payments pass cleanly through the dealer.
- **Dealer Profit:** A risk-free **0.1%** spread on the notional amount, locked in for the life of the swap.
![[Screenshot 2026-06-23 at 18.29.49.png]]
**Conclusion Note:** _"The companies mitigate the currency rate exchange risk and remain with interest rate risk."_ The US company successfully swapped a fixed foreign liability for a floating domestic one. They no longer care if the exchange rate moves, but they are now exposed to the domestic central bank raising the MRR.

### Hedging via Implied Notionals.

![[Screenshot 2026-06-23 at 18.32.45.png]]
The above image demonstrates how to hedge a known foreign cash flow by **reverse-engineering the swap notional**.

**The Problem:** You have a guaranteed receipt of $1,000 USD per year, but you operate in Kenya and need KES. If the exchange rate drops below 120/=, your revenues shrink. You want to lock in your KES revenue using a swap.

**The Solution Breakdown:**

Instead of guessing the KES equivalent of a $1,000 cash flow, you used the interest rates to find the "implied" principal amounts.

1. **Capitalize the Cash Flow:** If the US swap rate is 5%, what theoretical principal generates exactly $1,000 a year?
    
    $$USD_{Principal} = \frac{\$1,000}{0.05} = \$20,000$$
    
2. **Translate at Spot:** You take that implied $\$20,000$ principal and convert it to KES at the current spot rate of 120.
    
    $$KES_{Principal} = \$20,000 \times 120 = 2,400,000 \text{ KES}$$
    
3. **Generate the KES Cash Flow:** Now that you have the equivalent KES principal, you multiply it by the domestic Kenyan swap rate (12%) to find the fair periodic payment.
    
    $$KES_{Cash Flow} = 2,400,000 \times 0.12 = 288,000 \text{ KES}$$
    

**The Result:** You enter a swap where you hand over your $\$1,000$ receipt every year, and the dealer hands you $288,000 \text{ KES}$. You have perfectly hedged your FX risk without ever having to exchange a $20,000 principal!

If you were coding this into an execution system, how would you account for counterparty default risk—what happens if the swap dealer goes bankrupt halfway through the contract?