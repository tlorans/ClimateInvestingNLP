# Investigating the Transition Dimension in Climate Investing with Natural Language Processing

Climate investing is the new investment theme for asset owners and managers. 

Initially, it involved in constructing low-carbon portfolios. However, as stated by Roncalli et al. (2022) {cite:p}`2022:Roncallietal`, the concept of net zero has accelerated the scope of climate investing. Indeed, while low-carbon portfolios were mostly a tilt of a business-as-usual portfolio (by removing issuers with the highest carbon footprint), the net zero objective is also to green the economy. 

In this part, we will first discuss why the transition dimension is not addressed by the popular Paris-Aligned Benchmarks rulebook. Then, we will discuss our text-mining based approach to tackle the transition dimension.

## Financing the Transition: the Hidden Dimension in Paris-Aligned Benchmarks

When asset owners and managers speak about climate investing, they mainly focus on portfolio decarbonization. But building a net zero investment portfolio is more complex thant building a decarbonized portfolio, because the objective function encompasses at least **two goals**: **decarbonizing the portfolio** and **financing the transition** (Roncalli et al. 2022 {cite:p}`2022:Roncallietal`).

In climate investing, the Paris-Aligned Benchmarks is a popular methodology. The EU Technical Expert Group (TEG) developed the PAB rulebook stipulating standards for an index to align with the Paris temperature goal of 1.5°C. The minimum requirements are:
1. a 7% year-on-year average self-decarbonization in line with IPCC's 1.5°C scenario
2. an initial carbon intensity reduction of 50% relative to the investable universe.

However, the **PAB rulebook falls short in incentivising the transition to a green economy**. To achieve the trajectory, most current PAB actively under-weight high impact sectors such as Energy, Materials or Utilities (GICS sectors), and overweight Information Technology, Financials and Real Estate (Steffen 2022 {cite:p}`2022:Steffen`). 

Indeed, the PAB rulebook's exposure constraint requires intensity reductions to be achieved within "*high climate impact sectors*", not through the exclusion of these intensive impact sectors as a whole. However, as the constraint applies to the aggregate of all high impact sectors as a whole, one could divest from electric utilities and shift the weight to water utilities for example. 

In practice, this means that there is a **clear decoupling between the real economy and PABs**. As stated by MSCI for example, the carbon intensity reductions in their benchmark are mainly driven by over-weights in Consumer Discretionary and Information Technology for example (Steffen 2022 {cite:p}`2022:Steffen`). 

A portfolio contributing to the transition would involve to target overweighting green technologies within carbon intensive sectors.

To overcome these shortcomings, Roncalli et al. (2022) {cite:p}`2022:Roncallietal` propose to integrate transition metrics into the tilting methodology. The authors propose to use a **green taxonomy** and compute the corresponding green intensity for each issuer as a net zero transition metric. In the following, we will follow a similar approach. In the absence of decarbonation metrics data, we will focus on the transition dimension only. This contrasts with the approach adopted by Roncalli et al. (2022) {cite:p}`2022:Roncallietal`, who propose an integrated approach (using both transition and decarbonation metrics).

## Our Approach: a Text-Based Transition Investing Methodology

As exposed by Roncalli et al. (2022) {cite:p}`2022:Roncallietal`, we need to specify the measure of greeness for implementing the transition dimension in net zero investing. While the carbon footprint is a well-defined concept, greeness is more difficult to assess. 

We follow Roncalli et al. (2022) {cite:p}`2022:Roncallietal` in choosing the exposure to green activities as our central measure of greeness of issuers. In contrast, we do not use the same measurement approach, developed by Alessi and Battiston (2022) {cite:p}`2022:AlessiBattiston` (the taxonomy alignement coefficient, or TAC), based on a top-down approach to measure alignment to the EU Taxonomy based on sector-level data (NACE). In what follows, we develop a firm-level exposure to green activities measure with text mining, starting from a specific set of keywords describing green technologies in various sector.

We consider the construction of the transition portfolio based on benchmark optimization. In contrast to Roncalli et al. (2022) {cite:p}`2022:Roncallietal`, we do not adopt an integrated approach (integrating both the decarbonization and transition features), and focus on the transition dimension.