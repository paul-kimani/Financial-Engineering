![[Images/Forward Rates.png]]

To value any bond, we first need to determine its future cash flows and then discount those cash flows back to their present value using the given interest rates.

Since the problem doesn't state a specific face value (or par value) for the bond, it is standard practice to assume a face value of 100. The image states we are looking at a 4-year bond with a 3.75% annual coupon.

As our first step, assuming a face value of 100.
3.75 is  the annual coupon payment in Years 1, 2, and 3.

For Year 4, since the bond matures, the investor receives that final 3.75 coupon _plus_ the original 100 face value back. So, the total cash flow in Year 4 is 103.75.

Now we have our complete set of cash flows:
- **Year 1:** 3.75
- **Year 2:** 3.75
- **Year 3:** 3.75
- **Year 4:** 103.75
To find the value of the bond today, we need to discount each of these future cash flows back to their present value using the forward rates provided in the table.

Let's start with the first one. The table gives us the "0y1y" rate (which is the 1-year rate starting today) as 1.88%.

$$\sum_{i=1}^{N}{\frac{3.75}{(1+r_i)^i}}$$

So for Year 1, we plug in our cash flow of 3.75, our time period $i = 1$, and our rate $r_1 = 1.88\%$ (which we write as $0.0188$ in the formula).

First payment = 3.6808
Now, for Year 2, things get a little more interesting because the table gives us **forward rates** rather than standard spot rates.

A forward rate like "1y1y" (2.77%) is the interest rate for a 1-year period that _starts_ 1 year from now.

To bring a cash flow from Year 2 back to the present (Year 0), we have to discount it step-by-step: first, we discount it back to Year 1 using the Year 2 forward rate (1y1y). Then, we discount it back one more year to the present using the Year 1 rate (0y1y).

For year 2:
$$
\frac{3.75}{(1+0.0277)(1.0188)} = 3.5816
$$
For year 3:
$$
\frac{3.75}{(1.0277)(1.0188)(1.0354)} = 3.4593
$$
For year 4:
$$
\frac{3.75+100}{(1.0277)(1.0188)(1.0354)(1.0412)} = 91.9159
$$


Finally:
$$
3.6808+3.5816 + 3.4593 + 91.9159 = 102.6367
$$
