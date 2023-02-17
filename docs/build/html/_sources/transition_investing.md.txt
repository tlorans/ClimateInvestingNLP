# Transition Investing

We use the firm-level measure of greeness computed previously as our main feature to be integrated in our benchmark optimization problem.

We follow the framework described previously and our optimization problem becomes:


\begin{equation*}
\begin{aligned}
& x^* = 
& & argmin \frac{1}{2} \sigma^2 (x |b) - \gamma Exposure^{green} (x | b) \\
& \text{subject to}
& & 1_n^Tx = 1, \\
&&& 0_n \leq x \leq 1_n.
\end{aligned}
\end{equation*}