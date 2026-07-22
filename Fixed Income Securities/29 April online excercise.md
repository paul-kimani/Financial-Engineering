![[Images/Pasted image 20260506062741.png]]
A one-period interest rate tree models how the 1-year interest rate evolves from today (Time 0) to next year (Time 1). Today's 1-year rate is fixed, but next year's 1-year rate can either move to a high rate ($R_{1,H}$) or a low rate ($R_{1,L}$).

To properly calibrate this tree, we will eventually need to make sure it accurately prices the 2-year 6% annual coupon bond based on the given spot rates.

To construct the tree, our first step is to establish the starting point or the "root" of the tree today.

Based on the spot rate table provided in the image, what is the 1-year interest rate 📈 for Time 0?

---

## Cross‑references

- [[22 April 2026]] — the calibration target bond uses the same PV / YTM valuation formula.
- [[22 April online excercise]] — forward-rate discounting counterpart.
- [[../Stochastics/01 - Introduction to Stochastic Processes]] — discrete-time stochastic process foundations underlying the tree.
- [[../Stochastics/05 - Diffusion Processes Catalogue]] — Vasicek / CIR are the continuous-time analogue of this tree.
- [[../Stochastics/09 - Martingales]] — risk-neutral pricing on the tree (no-arbitrage condition).
- [[../Stochastics/25 - Vasicek Model]] — the continuous-time short-rate model this one-period rate tree discretises.

