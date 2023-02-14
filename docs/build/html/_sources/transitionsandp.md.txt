# A Green Transition S&P 500

In this part, we propose an exploratory approach of Natural Language Processing usages for constructing a green transition S&P 500. 

We first explain our approach for measuring exposure to green activities with text-mining, and then how to use these data in order to target a transition-enabler benchmark.

## Exposure to Green Activities

As exposed by Roncalli et al. (2022) {cite:p}`2022:Roncallietal`, we need to specify the measure of greeness for implementing the transition dimension in net zero investing. While the carbon footprint is a well-defined concept, greeness is more difficult to assess. 

We follow Roncalli et al. (2022) {cite:p}`2022:Roncallietal` in choosing the exposure to green activities as our central measure of greeness of issuers. In contrast, we do not use the same measurement approach, developed by Alessi and Battiston (2022) {cite:p}`2022:AlessiBattiston` (the taxonomy alignement coefficient, or TAC), based on a top-down approach to measure alignment to the EU Taxonomy based on sector-level data (NACE). In what follows, we develop a firm-level exposure to green activities measure with text mining, starting from a specific set of keywords describing green technologies in various sector. 

### A Green Taxonomy

In order to be able to determine the degree of greeness of issuers based on their activities, one needs a green taxonomy to determine what is deemed to be green or not. 

To provide a shared definition of green activites, the EU Commission has introduced the EU Taxonomy for sustainable economic activities, based on NACE codes. Activities are defined as green if they provide a contribution to a least one environmental objective (among six, even if technical screening critera have been developed for the two climate objectives only so far {cite:p}`2021:ANNEX1` {cite:p}`2021:ANNEX2`) while at the same time do not significant harm to other environmental objective. Furthermore, the activity must comply with minimum social safeguards (such as the UN guiding principles on business and human rights for example).

The main drawback of this initiative is that underlying data simply doesn't exist at the issuer level yet (Alessi and Battiston, 2022 {cite:p}`2022:AlessiBattiston`), at least as long as the Corporate Sustainability Reporting Directive (CSRD) is not implemented yet.

Recognizing the current impossibility to assess firm's greeness based on the EU Taxonomy, we choose to follow the taxonomy of climate solutions proposed by the Project Drawdown {cite:p}`2017:Drawdown`. The Project Drawdown's taxonomy lists green technologies supposed to significantly contribute to the green transition into the following sectors: (i) electricity; (ii) other energy; (iii) food, agriculture and land use; (iv) industry; (v) transportation; (vi) buildings; (vii) health and education; (viii) land sinks; (ix) land sinks; (x) coastal and ocean sinks and (xi) engineered sinks.

### Using Business Description for a Firm-Level Estimation of Greeness

### Green Activities in the S&P 500 Universe

## Transition S&P 500

### Tilting Formulation