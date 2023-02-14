# Climate Risks Hedging

In this part, we propose an exploratory approach of Natural Language Processing usages for constructing a climate risks hedged version of the S&P 500.

While Gorgen et al. (2020) {cite:p}`2020:Gorgenetal` and Roncalli et al. (2020) {cite:p}`2020:Roncallietal` use a factor-based approach to estimate firm's loading to the systematic carbon risk (with a Brown-Minus-Green Factor), we propose to follow Sautner et al. (2020) {cite:p}`2020:Sautneretal` and Hu et al. (2022) {cite:p}`2022:Huetal` by estimating directly a firm-level measure of exposure to climate risks using text mining.

In contrast to Gorgen et al. (2020) {cite:p}`2020:Gorgenetal` and Roncalli et al. (2020) {cite:p}`2020:Roncallietal`, this approach allows us to address both transition and physical risks.

In contrast with Roncalli et al. (2020){cite:p}`2020:Roncallietal`, we consider the construction of the climate risks hedged portfolio as a benchmark optimization problem and not as a minimum variance problem.

## Using Earnings Calls Transcripts for a Firm-Level Estimate of Climate Risks Exposure

We follow Sautner et al. (2020) {cite:p}`2020:Sautneretal` and Hu et al. (2022) {cite:p}`2022:Huetal` in constructing firm-level measure of exposure to climate risks using earnings calls transcripts.


We apply the framework described previously, using the keywords contained in the earnings calls transcripts of S&P 500 firms, as $K^{transcripts}_{i,t}$.

We use the list of initial keywords from Sautner et al. (2020) {cite:p}`2020:Sautneretal` as the source for our theme-specific keywords $S^{transition}$ and $S^{physical}$.

We obtain two different *rough* exposure measures:

\begin{equation}
Exposure^{transition}_{i,t} = \frac{1}{K^{transcripts}_{i,t}}\sum_{k^{transcripts}}^{K^{transcripts}_{i,t}}\tau(k^{transcripts}_{i,t})
\end{equation}

and 

\begin{equation}
Exposure^{physical}_{i,t} = \frac{1}{K^{transcripts}_{i,t}}\sum_{k^{transcripts}}^{K^{transcripts}_{i,t}}\tau(k^{transcripts}_{i,t})
\end{equation}

That can be summed in order to get the *rough* climate risks exposure:

\begin{equation}
Exposure^{climate}_{i,t} = Exposure^{transition}_{i,t} + Exposure^{physical}_{i,t}
\end{equation}


## Absolute vs. Relative Climate Risks Exposure


## Climate Risks Exposure in the S&P 500 Universe

## Climate Risks Hedging

## Climate Risks S&P 500