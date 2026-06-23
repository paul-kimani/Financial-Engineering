# Interest Rate Swap Terminology & Risk Profiles

In the derivatives market, avoiding the terms "long" and "short" prevents costly trading errors. The standard is to define positions by what they do with the **Fixed Rate**.

## 1. The Payer (Pays Fixed, Receives Floating)

- **The Action:** You lock in a fixed borrowing cost and receive floating market rates.
- **Bond Market Equivalent:** **"Short Bond"** (You act as if you issued/sold a fixed-rate bond and bought a floating-rate note).
- **Interest Rate Equivalent:** **"Long Rates"** (You want interest rates to rise).
- **How it makes money:** If central banks hike rates, the floating payments you _receive_ increase, but the fixed payments you _make_ stay the same. Your swap's Net Present Value (NPV) turns **positive**.

## 2. The Receiver (Receives Fixed, Pays Floating)

- **The Action:** You lock in a fixed investment return and pay floating market rates.
- **Bond Market Equivalent:** **"Long Bond"** (You act as if you bought a fixed-rate bond and issued/sold a floating-rate note).
- **Interest Rate Equivalent:** **"Short Rates"** (You want interest rates to fall).
- **How it makes money:** If central banks cut rates, the floating payments you _make_ decrease, but the fixed payments you _receive_ stay the same. Your swap's NPV turns **positive**.

## Analyzing Our Specific Example

In the previous calculation in [[02 Value of an Interest rate swap]], the Net Present Value was **-$5.30**.

- **Who was holding the -$5.30 position?** The **Payer**. The formula we used was $PV_{Floating} - PV_{Fixed}$, which is the Payer's perspective (Receiving Floating, Paying Fixed).
- **Why did they lose money?** Even though spot rates went up (which normally helps the Payer), the very first floating payment was "stuck" at the old 5% rate from Day 0. Meanwhile, they are locked into paying a high fixed rate of 7.726%. Paying 7.72% and receiving 5% creates a loss.
- **Who has the +$5.30 position?** The **Receiver**. Their formula is $PV_{Fixed} - PV_{Floating}$. They are thrilled to be receiving 7.726% while currently only having to pay out based on the old 5% rate.
[[02 Value of an Interest rate swap]] $\leftarrow$ Previous.                Next $\rightarrow$ [[04 Currency Swaps - Introduction]]

