# Blind Spots of Climate Investing

Climate investing is the new investment theme for asset owners and managers. Climate investing can be roughly divided into two main categories categories: (i) climate impact and (ii) climate change risks. 

In **climate impact strategies**, the explicit objective is to **decarbonize the portfolio**. In the context of index offering, this involved initially in constructing low-carbon indices. However, as stated by Roncalli et al. (2022) {cite:p}`2022:Roncallietal`, the concept of net zero has accelerated the scope of climate investing. Indeed, while low-carbon indices were mostly a tilt of a business-as-usual portfolio (by removing issuers with the highest carbon footprint), the net zero objective is also to green the economy. Today's reference approach in net zero investing is the Paris-aligned benchmark methodology. But we will see later that it falls short in adressing both dimensions (decarbonizing the portfolio and financing the transition).

In **climate risks strategies**, the objective is to **hedge for climate change-related risks exposure** in the portfolio (Andersson 2016 {cite:p}`2016:Anderssonetal`, Gorgen et al. 2020 {cite:p}`2020:Gorgenetal`, Engle et al. 2019 {cite:p}`2019:Engleetal`, Roncalli et al. 2020 {cite:p}`2020:Roncallietal`). Two dimensions of climate risks need to be addressed: (i) transition risks and (ii) physical risks.
Transition risks correspond to the risks arising from the sudden shift towards a low carbon economy. Such transitions could mean that some sectors of the economy face big shifts in asset values or higher costs of doing business.
Physical risks correspond to the financial losses that come from climate change such as droughts, storms, or floods. As stated by Le Guenedal and Roncalli (2022) {cite:p}`2022:LeGuenedalRoncalli`), investors have paid more attention to the transition risks than to the physical risks up to date.

In the first part, we will see why financing the transition is still an hidden dimension in the most popular approach of net zero investing. In the second part, we'll see that idiosyncratic measures of exposure to climate risks are lacking, especially regarding the physical risks dimension.

## Financing the Transition: the Hidden Dimension in Paris-Aligned Benchmarks

When asset owners and managers speak about net zero investing, they mainly focus on portfolio decarbonization. But building a net zero investment portfolio is more complex thant building a decarbonized portfolio, because the objective function encompasses at least two goals: **decarbonizing the portfolio and financing the transition** (Roncalli et al. 2022 {cite:p}`2022:Roncallietal`).

In net zero investing, the Paris-Aligned Benchmarks is a popular methodology. The EU Technical Expert Group (TEG) developed the PAB rulebook stipulating standards for an index to align with the Paris temperature goal of 1.5°C. The minimum requirements are (i) a 7% year-on-year average self-decarbonization in line with IPCC's 1.5°C scenario and (ii) an initial carbon intensity reduction of 50% relative to the investable universe.

However, the **PAB rulebook falls short in incentivising the transition to a green economy**. To achieve the trajectory, most current PAB actively under-weight high impact sectors such as Energy, Materials or Utilities (GICS sectors), and overweight Information Technology, Financials and Real Estate (Steffen 2022 {cite:p}`2022:Steffen`). 

Indeed, the PAB rulebook's exposure constraint requires intensity reductions to be achieved within "*high impact sectors*", not through the exclusion of these intensive impact sectors as a whole. However, as the constraint applies to the aggregate of all high impact sectors as a whole, one could divest from electric utilities and shift the weight to water utilities for example. 

In practice, this means that there is a **clear decoupling between the real economy and PABs**. As stated by MSCI for example, the carbon intensity reductions in their benchmark are mainly driven by over-weights in Consumer Discretionary and Information Technology for example (Steffen 2022 {cite:p}`2022:Steffen`). 

To overcome these shortcomings of the PAB framework as a net zero investing solution, Roncalli et al. (2022) {cite:p}`2022:Roncallietal` propose to integrate net zero transition metrics into the tilting methodology. The authors propose to use a **green taxonomy** and compute the corresponding green intensity for each issuer as a net zero transition metric. In the following, we will follow a similar but simplified approach.

## Will Hedging for Climate Risks Exposure at Firm Level Still Unreachable?

One can divide the issue in climate risks hedging strategies into two dimensions: (i) systematic or idiosyncratic measure of risks and (ii) the type of risks addressed (transition or physical risks). 

Some recent literature propose to address the exposure to the **systematic transition risks with market measures** (Gorgen et al. 2020 {cite:p}`2020:Gorgenetal`, Roncalli et al. 2020 {cite:p}`2020:Roncallietal`). To the best of our knowledge, no equivalent approach has been developed for exposure to systematic physical risks up to data. This underlies the focus on the transition risks dimension by investors.

Regarding the **idiosyncratic exposure to climate risks**, some **critics arised concerning the fundamental** data traditionaly used to address them. 

First, for the transition risks dimension, **carbon emissions** are frequently used as fundamental. However, Aswani et al. (2022) {cite:p}`2022:Aswanietal` shows that **more than half of the observations reported by Trucost are estimated** and not disclosed by firms (up to 80% for the most recent data). Hu et al. (2022) {cite:p}`2022:Huetal` state that this figure put question on the quality of analysis to be conducted using these data as fundamental as the estimated emissions are "***are systematically biased as the estimation methods rely heavily on firm fundamentals and industry-level factors***". 

Second, for the physical risks dimension, fundamental data are sparse and difficult to handle to say the least. This mainly involves the modelling of projected event-intensity and **disasters exposure** at fine-grained geolocation level (county-level for US for example). This requires the **geolocation fo the portfolio**. Hu et al. (2022) {cite:p}`2022:Huetal` point out two main issues regarding the use of these data as fundamental data for assessing firms' level exposure to physicals risks: (i) these data **cannot distinguish impacts on different firms in the same geographic location** (either country in some database / modelling approaches or county-level); (ii) the use of these data generally assume that **firms are affected only if headquarters are located in the disaster area**. 

In order to overcome those issues, a recent literature propose to use **text-mining in order to measure firm-level exposure to climate risks** (Sautner et al. 2020 {cite:p}`2020:Sautneretal`) and Hu et al. 2022 {cite:p}`2022:Huetal`). In the following, we will follow a similar approach.

## About the Use of Natural Language Processing for Uncovering Hidden Dimensions in Climate Investing

In what we've seen above, current hidden dimensions in climate investing (transition in net zero investing and firms level climate risks exposure in climate risks hedging) aren't due to unwilligness of investors or regulators. 

Indeed, the most stringent root of shortcomings in current climate impact or climate risks strategies are due to the lack of proper data, noteworthy in the context of indices tilting, where large coverage needs to be ensure.

To address the transition dimension in net zero investing, we need to tackle the issue regarding the green activities from issuers. We will propose an exploratory approach, with a **text-based measure of exposure to green activities and the corresponding index tilted towards transition activities**.

To address the need for firm-level climate risks exposure measure, we will propose a **text-based measure and the corresponding index hedging for exposure to climate risks**.

In the following part, we will briefly present a common simple framework in order to address these dimensions with Natural Language Processing and explore the implications for a tilted benchmark.