![[Screenshot 2026-04-15 at 16.30.17.png]]

A #repo is essentially a short-term collateralized loan. The "seller" of the bond is borrowing money from the "buyer" and using that Treasury bond as collateral.
**a) Repo haircut**.

The problem states that the initial margin is 105%. The initial margin tells us the ratio of the collateral's market value to the actual amount of money loaned (the purchase price). 

The "**repo haircut**," on the other hand, represents the percentage discount applied to the collateral's value to protect the buyer if the bond's value drops.

There is a direct mathematical relationship between the initial margin and the haircut. 

The formula to find the repo haircut when you know the initial margin is:

$$Haircut = 1 - \frac{1}{\text{Initial Margin}}$$

To use this formula, we first need to convert the initial margin of 105% into a decimal, which gives us 1.05.

1/21 (which is approximately 4.76%) is the repo haircut.

**b) Purchase price**.

The purchase price is the actual amount of cash the buyer is handing over to the seller today as the loan. The problem states the bond is trading at its face value of KES 10 million, so that is the current market value of our collateral.

Because the buyer requires an initial margin of 105% (or 1.05), they want the collateral to be worth 105% of the cash they are lending out. We can find the purchase price using this formula:

$$Purchase Price = \frac{\text{Market Value of Collateral}}{\text{Initial Margin}}$$

Given the market value of KES 10 million and the initial margin of 1.05, what do you calculate for the purchase price?

9,523,809.52 KES is the purchase price.

**c) Repurchase price**.


First, let's just calculate the **interest** the buyer earns over those 20 days:

$$Interest = \text{Purchase Price} \times \text{Repo Rate} \times \frac{\text{Term}}{360}$$

Using our Purchase Price of 9,523,809.52, the decimal rate of 0.008, and the 20-day term, the interest amount is 4,232.80 KES

Spot on! 4,232.80 KES is exactly right for the interest earned over those 20 days.

To find the final **Repurchase price**, the seller needs to return the original loan amount (the purchase price) _plus_ the interest we just calculated.

So, we just add them together:

$$Repurchase Price = 9,523,809.52 + 4,232.80$$

9,528,042.32 KES is the repurchase price. 
