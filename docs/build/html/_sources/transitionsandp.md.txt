# Transition Investing

In this part, we propose an exploratory approach of Natural Language Processing usages for constructing a green transition S&P 500. 

As exposed by Roncalli et al. (2022) {cite:p}`2022:Roncallietal`, we need to specify the measure of greeness for implementing the transition dimension in net zero investing. While the carbon footprint is a well-defined concept, greeness is more difficult to assess. 

We follow Roncalli et al. (2022) {cite:p}`2022:Roncallietal` in choosing the exposure to green activities as our central measure of greeness of issuers. In contrast, we do not use the same measurement approach, developed by Alessi and Battiston (2022) {cite:p}`2022:AlessiBattiston` (the taxonomy alignement coefficient, or TAC), based on a top-down approach to measure alignment to the EU Taxonomy based on sector-level data (NACE). In what follows, we develop a firm-level exposure to green activities measure with text mining, starting from a specific set of keywords describing green technologies in various sector.

We consider the construction of the transition portfolio based on benchmark optimization. In contrast to Roncalli et al. (2022) {cite:p}`2022:Roncallietal`, we do not adopt an integrated approach (integrating both the decarbonization and transition features), and focus on the transition dimension.

## A Green Taxonomy

In order to be able to determine the degree of greeness of issuers based on their activities, one needs a green taxonomy to determine what is deemed to be green or not. 

To provide a shared definition of green activites, the EU Commission has introduced the EU Taxonomy for sustainable economic activities, based on NACE codes. Activities are defined as green if they provide a contribution to a least one environmental objective (among six, even if technical screening critera have been developed for the two climate objectives only so far {cite:p}`2021:ANNEX1` {cite:p}`2021:ANNEX2`) while at the same time do not significant harm to other environmental objective. Furthermore, the activity must comply with minimum social safeguards (such as the UN guiding principles on business and human rights for example).

The main drawback of this initiative is that underlying data simply doesn't exist at the issuer level yet (Alessi and Battiston, 2022 {cite:p}`2022:AlessiBattiston`), at least as long as the Corporate Sustainability Reporting Directive (CSRD) is not implemented yet.

Recognizing the current impossibility to assess firm's greeness based on the EU Taxonomy, we choose to follow the taxonomy of climate solutions proposed by the Project Drawdown {cite:p}`2017:Drawdown`. The Project Drawdown's taxonomy lists green technologies supposed to significantly contribute to the green transition into the following sectors: (i) electricity; (ii) other energy; (iii) food, agriculture and land use; (iv) industry; (v) transportation; (vi) buildings; (vii) health and education; (viii) land sinks; (ix) land sinks; (x) coastal and ocean sinks and (xi) engineered sinks.

## Using Business Description for a Firm-Level Estimate of Greeness

As Alessi and Battiston (2022) {cite:p}`2022:AlessiBattiston` have choosen to follow the EU Taxonomy framework, while data are unavailable at the issuer level, they propose a top-down approach to measure firm greeness, by computing a taxonomy alignment coefficient with data at sector level (NACE sectors). 

As we want to estimate a firm level measure of greeness, we develop a text-mining framework based on the green technologies listed by the Project Drawdown (2017) {cite:p}`2017:Drawdown`.

We apply the framework described previously, using the keywords contained in the business description of S&P 500 firms, extracted from 10-K fillings as $K^{activity}_{i,t}$.

Below is the Python code to load the business description (for 2020 only) in the `materials` folder and retrieve the keywords per business description:

```python
import pandas as pd 

business_desc = pd.read_csv('sandp_business_desc.csv')

from keyphrase_vectorizers import KeyphraseCountVectorizer  
vectorizer = KeyphraseCountVectorizer(stop_words = 'english')

business_desc.dropna(inplace = True)

list_K = [vectorizer.fit([i]).get_feature_names_out().tolist() for i in (business_desc['Business_description'].tolist())]
list_K[2]
```

As an illustration, this outputs the following keywords list for Abbot:

```
['businessgeneral development',
 'october',
 'bill',
 'discovery',
 'abbott medical optics',
 'johnson',
 'illinois corporation',
 'sale',
 'aggregate consideration',
 'alere',
 'acquisition',
 'service provider',
 'nutritional products',
 'established pharmaceutical products',
 'diagnostic device',
 'vision care business',
 'manufacture',
 'reportable segments',
 'businessabbott',
 'cash',
 'development',
 'item',
 'principal business',
 'narrative description',
 'diversified line',
 'businessabbott laboratories',
 'inc.',
 'diagnostic products',
 'health care products',
 'february',
 'abbott',
 'medical devices']
```

We use the list of green technologies from the Project Drawdown as the list of theme-specific keywords $S^{green}$.

Below is the Python code to get the keywords list in the `materials` folder:

```python
green_tech = pd.read_excel('Green_tech.xlsx')
S = green_tech.Keyword.tolist()
S
```

```
['Abandoned Farmland Restoration',
 'Alternative Cement',
 'Alternative Refrigerants',
 'Bamboo Production',
 'Bicycle Infrastructure',
 'Biochar Production',
 'Biogas for Cooking',
 'Biomass Power',
 'Bioplastics',
 'Building Automation Systems',
 'Building Retrofitting',
 'Carpooling',
 'Clean Cooking',
 'Coastal Wetland Protection',
 'Coastal Wetland Restoration',
 'Composting',
 'Concentrated Solar Power',
 'Conservation Agriculture',
 'Distributed Energy Storage',
 'Distributed Solar Photovoltaics',
 'District Heating',
 'Dynamic Glass',
 'Efficient Aviation',
 'Efficient Ocean Shipping',
 'Efficient Trucks',
 'Electric Bicycles',
 'Electric Cars',
 'Electric Trains',
 'Family Planning and Education',
 'Farm Irrigation Efficiency',
 'Forest Protection',
 'Geothermal Power',
 'Grassland Protection',
 'Green and Cool Roofs',
 'Grid Flexibility',
 'High-Efficiency Heat Pumps',
 'High-Performance Glass',
 'High-Speed Rail',
 'Hybrid Cars',
 'Improved Aquaculture',
 'Improved Cattle Feed',
 'Improved Fisheries',
 'Improved Manure Management',
 'Improved Rice Production',
 'Indigenous Peoples’ Forest\xa0Tenure',
 'Insulation',
 'Landfill Methane Capture',
 'LED Lighting',
 'Low-Flow Fixtures',
 'Macroalgae Protection and Restoration',
 'Managed Grazing',
 'Methane Digesters',
 'Methane Leak Management',
 'Micro Wind Turbines',
 'Microgrids',
 'Multistrata Agroforestry',
 'Net-Zero Buildings',
 'Nuclear Power',
 'Nutrient Management',
 'Ocean Power',
 'Offshore Wind Turbines',
 'Onshore Wind Turbines',
 'Peatland Protection and Rewetting',
 'Perennial Biomass Production',
 'Perennial Staple Crops',
 'Plant-Rich Diets',
 'Public Transit',
 'Recycled Metals',
 'Recycled Paper',
 'Recycled Plastics',
 'Recycling',
 'Reduced Food Waste',
 'Reduced Plastics',
 'Refrigerant Management',
 'Regenerative Annual Cropping',
 'Seafloor Protection',
 'Seaweed Farming',
 'Silvopasture',
 'Small Hydropower',
 'Smart Thermostats',
 'Solar Hot Water',
 'Sustainable Intensification for Smallholders',
 'System of Rice Intensification',
 'Telepresence',
 'Temperate Forest Restoration',
 'Tree Intercropping',
 'Tree Plantations (on Degraded Land)',
 'Tropical Forest Restoration',
 'Utility-Scale Energy Storage',
 'Utility-Scale Solar Photovoltaics',
 'Walkable Cities',
 'Waste to Energy',
 'Water Distribution Efficiency']
```

Finally, we obtained the exposure to green activities such as:

\begin{equation}
Exposure^{green}_{i,t} = \frac{1}{K^{activity}_{i,t}}\sum_{k^{activity}}^{K^{activity}_{i,t}}\tau(k^{activity}_{i,t})
\end{equation}

Below is the Python code to obtain the resulting green activities exposure code:

```python
from sentence_transformers import SentenceTransformer, util
import torch

# load the embedding model
ST = SentenceTransformer('all-MiniLM-L6-v2')

# get the embeddings
list_emb_K = [ST.encode(sent, convert_to_tensor=True) for sent in (list_K)]
emb_S = ST.encode(S, convert_to_tensor=True)

# get the cosine similarity score for each keywrod
list_hits = [util.semantic_search(sent, emb_S, top_k=1) for sent in (list_emb_K)]

# get the list of keywords related to our theme
list_T = []

for sent in tqdm(list_hits):
  T = []

  for i in range(len(sent)):
    if sent[i][0]['score'] >= 0.6:
      T.append(i)

  list_T.append(T)


# Get the Exposure Code
business_desc['Exposure_Green_Tech'] = [len(list_T[i]) / len(list_K[i]) for i in range(len(list_T))]
```

## Green Activities in the S&P 500 Universe

![Boxplot Green Exposure](box_plot_green_exposure.PNG)


![green_exposure_10_sub_industries](green_exposure_10_sub_industries.PNG)


![10_securities_green_tech_exposure](10_securities_green_tech_exposure.PNG)

## Transition Investing

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

## Transition S&P 500
