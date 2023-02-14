# Climate Risks Hedging

In this part, we propose an exploratory approach of Natural Language Processing usages for constructing a climate risks hedged version of the S&P 500.

While Gorgen et al. (2020) {cite:p}`2020:Gorgenetal` and Roncalli et al. (2020) {cite:p}`2020:Roncallietal` use a factor-based approach to estimate firm's loading to the systematic carbon risk (with a Brown-Minus-Green Factor), we propose to follow Sautner et al. (2020) {cite:p}`2020:Sautneretal` and Hu et al. (2022) {cite:p}`2022:Huetal` by estimating directly a firm-level measure of exposure to climate risks using text mining.

In contrast to Gorgen et al. (2020) {cite:p}`2020:Gorgenetal` and Roncalli et al. (2020) {cite:p}`2020:Roncallietal`, this approach allows us to address both transition and physical risks.

In contrast with Roncalli et al. (2020) {cite:p}`2020:Roncallietal`, we consider the construction of the climate risks hedged portfolio as a benchmark optimization problem and not as a minimum variance problem.

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

However, as we've denoted above, our current exposure measure is a *rough* measure in the sense that it doesn't distinguish between positive or negative mention of the risk discussed. This distinction is related to the distinction between relative and absolute risk made by Roncalli et al. (2020) {cite:p}`2020:Roncallietal`. 

Relative risk measure objective is to be more exposed to firms positively exposed to climate risks and less exposed to firms negatively exposed to climate risks. On the other side, absolute climate risks considers that both large positive and negative climate risks exposure incur a financial risk that must be reduced. In what follow, we will focus on relative risk measure.

Our *rough* climate risks exposure measure doesn't comes with any sign associated with, as it only measure the percentage of keywords from the transcripts related to one of the climate risks. It is not directly comparable to the carbon risk measure from Roncalli et al. (2020) {cite:p}`2020:Roncallietal` resulting from a multi-factor regression approach with a positive or negative sign associated with it.

To transform our *rough* measure to a relative risk measure, we follow Hu et al. (2022) {cite:p}`2022:Huetal` by applying FinBERT to identify the sentiment behind each keyword from the transcripts identified as related to one of the climate risks, considering the context of this keyword.

1. We first need to find $p^{k^{transition}_{i,t}}$ and $p^{k^{physical}_{i,t}}$ the sentences where the keywords are located with a simple regular expression search. 
2. We apply FinBERT on the sentences in order to obtain $Sentiment^{transition}_{i,t}$ and $Sentiment^{physical}_{i,t}$, that are scores between -1 (negative) and 1 (positive), with 0 corresponding to a neutral tone.
3. We finally get a modified exposure measure $RExposure^{transition}_{i,t}$ and $RExposure^{physical}_{i,t}$ by the corresponding sentiment score $Sentiment^{transition}_{i,t}$ and $Sentiment^{physical}_{i,t}$.

## Climate Risks Exposure in the S&P 500 Universe

## Climate Risks Hedging

## Climate Risks S&P 500