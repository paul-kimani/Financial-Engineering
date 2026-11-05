The Lemma is:
$$
S_tN'(d_1) = Ke^{-r\tau}N'(d_2), \quad \tau = T-t
$$
#### Proof
Go ask a toddler down the street.
Just kidding! the proof is as below:
1. **Ratio of the Densities**
$$
\begin{align*}
\frac{N'(d_1)}{N'(d_2)} &= \frac{\phi(d_1)}{\phi(d_2)} = \frac{\frac{1}{\sqrt{2\pi}}\cdot e^{-\frac{d_1^2}{2}}}{\frac{1}{\sqrt{2\pi}}\cdot e^{-\frac{d_2^2}{2}}} = \frac{e^{-\frac{d_1^2}{2} } }{{e^{-\frac{d_2^2}{2}}}}\\
&= \exp \left(-\left( \frac{d_1^2}{2} - \frac{d_2^2}{2}\right) \right)\\
&= \exp\left(-\frac{d_1^2-d_2^2}{2}\right) \quad \quad (\ast)
\end{align*}
$$

We know that: 
$$
\begin{align*}
a^2-b^2 &= (a-b)(a+b)\\
\Rightarrow d_1^2 - d_2^2 &= (d_1-d_2)(d_1+d_2)\\
\text{But:} \quad d_2 &= d_1 - \sigma\sqrt{\tau}\\
(d_1 - d_2) &= \sigma \sqrt{\tau}\\
(d_1 + d_2)&= 2d_1 - \sigma \sqrt{\tau}\\
\therefore d_1^2 - d_2^2 &= \sigma\sqrt{\tau}\left(2d_1 - \sigma \sqrt{\tau}\right)\\
&= 2d_1\sigma\sqrt{\tau} - \sigma^2 \sqrt{\tau}
\end{align*}
$$

But Recall:$\quad d_1 = \frac{ln \left(\frac{S}{K} \right) + \left( r + \frac{1}{2}\sigma^2\right)\tau}{\sigma \sqrt{\tau}}$ [[08. Greeks For European Options]]
Hence:
$$
\begin{align*}
\sigma \sqrt{\tau} \cdot d_1 &= ln \left(\frac{S}{K} \right) + \left( r + \frac{1}{2}\sigma^2\right)\tau\\
\text{substituting into:} \quad &d_1^2 - d_2^2 \text{, We have:} \\
d_1^2 - d_2^2 &= 2\left( \ln\left( \frac{S}{K}\right)+\left(r+\frac{1}{2}\sigma^2\right)\tau\right)-\sigma^2\tau\\
&= 2 \ln \left(\frac{S}{K}\right)+2r\tau+\sigma^2\tau-\sigma^2\tau\\
&= 2 \ln \left(\frac{S}{K}\right)+2r\tau\\
\therefore \quad &\\ 
\frac{d_1^2 - d_2^2}{2} &= \ln\left(\frac{S}{K}\right) + r\tau\\
\text{and}:\\
\frac{\phi(d_1)}{\phi(d_2)} &= \exp \left\{ -\frac{(d_1^2-d_2^2)}{2}\right\}\\
&= \exp\left\{- \left(\ln\left(\frac{S}{K}\right)+r\tau\right)\right\}\\
&= \left(\frac{S}{K}\right)^{-1} \cdot e^{-r\tau}\\
&=\frac{K}{S} \cdot e^{-r\tau} \quad (\ast\ast)
\end{align*}
$$

Finally:
$$
\begin{align*}
\frac{\phi(d_1)}{\phi(d_2)} &= \frac{K}{S} \cdot e^{-r\tau}\\
\text{Taking the cross terms:}\\
\underline{\underline{S \cdot\phi(d_1) = K\cdot e^{-r\tau}\phi(d_1)}}
\end{align*}
$$
**Hence the proof**




