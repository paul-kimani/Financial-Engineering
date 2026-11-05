
| **Greeks** | **Call Option(c)**                                                                              | Put Option(P)                                                                                   |
| ---------- | ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Delta      | $\Delta_c = N(d_1)$                                                                             | $\Delta_c = N(d_1) -1$                                                                          |
| Gamma      | $\Gamma _c=\frac{N'(d_1)}{s\sigma\sqrt{\tau}}=\frac{Ke^{-r\tau}N'(d_2)}{s^2\sigma\sqrt{\tau}}$  | $\Gamma _p=\frac{N'(d_1)}{s\sigma\sqrt{\tau}}=\frac{Ke^{-r\tau}N'(d_2)}{s^2\sigma\sqrt{\tau}}$  |
| Theta      | $\theta_c=-rKe^{-r\tau}N(d_2)-\frac{\sigma\cdot SN'(d_1)}{2\sqrt{\sigma}}$                      | $\theta_p=-rKe^{-r\tau}N(-d_2)-\frac{\sigma\cdot SN'(d_1)}{2\sqrt{\sigma}}$                     |
| Rho        | $\rho_c=rKe^{r\tau}N(d_2)$                                                                      | $\rho_p=-rKe^{-r\tau}N(-d_2)$                                                                   |
| Vega       | $\mathbf{v}_c=\sqrt{\tau} \cdot SN'(d_1)$<br>OR<br>$\mathbf{v}_c=\sqrt{\tau}Ke^{-r\tau}N'(d_2)$ | $\mathbf{v}_p=\sqrt{\tau} \cdot SN'(d_1)$<br>OR<br>$\mathbf{v}_p=\sqrt{\tau}Ke^{-r\tau}N'(d_2)$ |
**NB:** $\quad N'(x) = N'(-x)$
