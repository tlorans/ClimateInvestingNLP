# A Simple End to End Framework

In order to explore how Natural Language Processing can help in addressing climate investing issues, we propose a simple end to end framework, starting with an algorithm adapted from Sautner et al. (2020) {cite:p}`2020:Sautneretal` to construct firm-level exposure to a climate theme (later exposure to green activities and exposure to climate risks) and follow Roncalli's approach (2023) {cite:p}`2023:Roncalli` for the benchmark tilting.

## Firm-Level Exposure to a Climate Theme with NLP

Following Sautner et al. (2020) {cite:p}`2020:Sautneretal` approach, one can build a firm-level theme exposure to a specific theme (in Sautner et al., climate risks exposure) based on text mining. 

More specifically, we can define the firm-level exposure to a theme as *the percentage of keywords from the firm's text (in the following of this proposal earnings call transcripts or business description) that are related (ie. semantically similar) to a list of specific theme keywords.*

### Keywords Extraction and Theme-Specific Keywords

We first need to extract, for each firm, a set of keywords $K_{i,t}$ from the firm specific text. We also need to define a set $S$ of theme-specific keywords to be compared with.

Let's have an example with the following text:

*The profitable growth in the gas and low carbon electricity integrated value chains is one of the key axes of Total's strategy. In order to give more visibility to these businesses, a new reporting structure for the business segments’ financial information has been put in place, effective January 1, 2019 and organized around four business segments: Exploration & Production (EP), Integrated Gas, Renewables & Power segment (iGRP), Refining & Chemicals (RC) and Marketing & Services (MS). The iGRP segment spearheads Total’s ambitions in integrated gas (including LNG, liquefied natural gas) and low carbon electricity businesses. It consists of the upstream and midstream LNG activity that was previously reported in the EP segment (refer to the indicative list of assets in the Annex) and the activity previously reported in the Gas Renewables & Power segment. The new EP segment is adjusted accordingly.*

Below, we install the package `keyphrase-vectorizers` that will extract keywords from the text above. 

```python
!pip install keyphrase-vectorizers

from keyphrase_vectorizers import KeyphraseCountVectorizer  

vectorizer = KeyphraseCountVectorizer(stop_words = 'english')

firm_text = "The profitable growth in the gas and low carbon electricity integrated value chains is one of the key axes of Total's strategy. In order to give more visibility to these businesses, a new reporting structure for the business segments’ financial information has been put in place, effective January 1, 2019 and organized around four business segments: Exploration & Production (EP), Integrated Gas, Renewables & Power segment (iGRP), Refining & Chemicals (RC) and Marketing & Services (MS). The iGRP segment spearheads Total’s ambitions in integrated gas (including LNG, liquefied natural gas) and low carbon electricity businesses. It consists of the upstream and midstream LNG activity that was previously reported in the EP segment (refer to the indicative list of assets in the Annex) and the activity previously reported in the Gas Renewables & Power segment. The new EP segment is adjusted accordingly"

K = vectorizer.fit([firm_text]).get_feature_names_out()
```
The resulting list of keywords would be:
```
array(['key axes', 'services', 'natural gas', 'new ep segment',
       'exploration', 'power segment', 'ambitions', 'activity',
       'low carbon electricity businesses', 'business segments',
       'refining', 'ms', 'value chains', 'lng', 'gas renewables', 'ep',
       'indicative list', 'profitable growth', 'place', 'renewables',
       'ep segment', 'businesses', 'integrated gas', 'order', 'strategy',
       'more visibility', 'rc', 'effective january', 'production',
       'low carbon electricity', 'annex', 'financial information',
       'midstream lng activity', 'chemicals', 'igrp segment', 'marketing',
       'new reporting structure', 'gas', 'assets', 'total'], dtype=object)
```
Let's store a short list of theme specific keywords for later use:
```python
S = ['solar pv', 'wind technology','renewables equipment']
```

### Embeddings


Once firm's keywords $K_{i,t}$ are extracted and the set of theme-specific keywords $S$ are defined, we need to transform those unstructured data (text) into a numerical representation. To do so, we use Sentence Transformer capacities to create numerical vector representation of the meanings of $K_{i,t}$ and keywords in $S$:

\begin{equation}
Emb^{k_{i,t}} = ST(k_{i,t})
\end{equation}

and 

\begin{equation}
Emb^{s} = ST(s)
\end{equation}

Where $Emb^{k_{i,t}}$ and $Emb^{s}$ are the numerical vector representation (embeddings) of the keyword $k_{i,t}$ and the keyword $s$, performed with the Sentence Transformer model $ST()$.

Below we install and apply the package `sentence-transformers` for our keywords embeddings. 

```python
!pip install sentence_transformers

from sentence_transformers import SentenceTransformer, util
import torch

ST = SentenceTransformer('all-MiniLM-L6-v2')

emb_K = ST.encode(K, convert_to_tensor=True)
emb_S = ST.encode(S, convert_to_tensor=True)
```

### Cosine Similarity


We want to be able to determine if the keyword $k_{i,t}$ is related to our theme, thanks to the theme-specific keywords.

As we now have numerical vector representations of our keywords $Emb^{k_{i,t}}$ and $Emb^s$, we can apply principles from semantic search by determining the closeness of our two vectors. Recalling that dimensions of our vector representation relate to the underlying meaning of the keywords, computing the closeness of our vectors allows for determining the semantic similarity between our keywords. 

One way to do so is by computing the cosine similarity between our two vectors. The cosine similarity measures the cosine of the angle of those two vectors. The closer the cosine similarity to 1 is, the more related the keywords are:


\begin{equation}
cos(\theta_{k_{i,t},s}) = \frac{Emb^{k_{i,t}} \cdot Emb^s}{||Emb^{k_{i,t}}|| ||Emb^s||}
\end{equation}

For each keyword extracted from firm's text $k_{i,t}$, the cosine similarity is computed against each theme-specific keywords $s$. The maximum cosine similarity measure is retained, and then compared to a predefined threshold (0.6) in order to finally determined if the keyword is related to our theme:


\begin{equation}
  \tau(k_{i,t})=
  \begin{cases}
    0, & \text{if}\ cos(\theta_{k_{i,t},s}) < 0.6 \\
    1, & \text{otherwise}
  \end{cases}
\end{equation}

Where $\tau(k_{i,t})$ corresponds to an indicator taking the value 1 if the keyword is related to our specific theme and 0 otherwise.

Below we use the `semantic_search` function from the `sentence-transformers` package, that compute the cosine similarity between our embeddings.

```python
hits = util.semantic_search(emb_K, emb_S, top_k=1)

T = []

for i in range(len(hits)):
  if hits[i][0]['score'] >= 0.6:
    T.append(i)


print(K[T])
```

Among the keywords extracted from our example, two are identified as close to our theme specific keywords:

```python
['gas renewables' 'renewables']
```

### Exposure


Finally, we can compute our theme exposure such as:

\begin{equation}
Exposure_{i,t} = \frac{1}{K_{i,t}}\sum_{k}^{K_{i,t}}\tau(k_{i,t})
\end{equation}

```python
Exposure = len(T) / len(K)
print(Exposure)
```

```python
0.05
```


## Climate Tilting 

In what follows we present our simple framework for tilting a benchmark according to a dual objective: (i) minimizing the tracking error volatility risk and (ii) improving the *greeness* of the tilted version compared to the benchmark. To do so, we follow the approach from Roncalli (2023) {cite:p}`2023:Roncalli`, formulating our tilting problem as a portfolio optimization with the presence of a benchmark.

### Benchmark Greeness

We need first to define the *benchmark greeness*.
Let's assume $b$ the vector of weights of a benchmark which is equally-weighted:

\begin{equation}
b = \begin{bmatrix}
           0.2 \\
           0.2 \\
           0.2 \\
           0.2 \\
           0.2
         \end{bmatrix}
\end{equation}

Let's assume $G$ a vector of *greeness* measure of the stocks composing the benchmark:

\begin{equation}
G = \begin{bmatrix}
           0.95 \\
           0.08 \\
           0.67 \\
           0.12 \\
           0.47
         \end{bmatrix}
\end{equation}

The benchmark greeness can be computed as:

\begin{equation}
G(b) = b^TG
\end{equation}

Below we import the `numpy` package in order to compute the benchmark greeness.
```python
import numpy as np

b = np.array([0.2, 0.2, 0.2, 0.2, 0.2])
G = np.array([0.95,0.08,0.67,0.12,0.47])
np.dot(b, G)
```

According to the illustrative data we use, the benchmark greeness is:
```
0.4580000000000001
```

### Tracking Error Volatility and Excess Greeness

Having the previous benchmark, let's assume a portfolio with the same issuers than the benchmark, but with different weights $x$:

\begin{equation}
x = \begin{bmatrix}
           0.10 \\
           0.15 \\
           0.30 \\
           0.20 \\
           0.25
         \end{bmatrix}
\end{equation}

Let's have the following covariance matrix:

\begin{equation}
\Sigma = 
\begin{pmatrix}
0.1536 & 0.006 & 0.0108 & 0.0156 & 0.024 \\
0.006 & 0.17 & 0.018 & 0.026 & 0.04 \\
0.0108 & 0.018 & 0.1324 & 0.0468 & 0.072 \\
0.0156 & 0.026 & 0.0468 & 0.1776 & 0.104 \\
0.024 & 0.04 & 0.072 & 0.104 & 0.28 \\
\end{pmatrix}
\end{equation}

We can compare the relative performance of the portfolio compared to the benchmark with the tracking error volatility:

\begin{equation}
\sigma(x|b) = \sqrt{(x - b)^T \Sigma (x - b)}
\end{equation}

Below we compute the tracking error volatility:
```python
x = np.array([0.1, 0.15, 0.30, 0.20, 0.25])
Sigma = np.array([[ 0.1536,  0.006,  0.0108,  0.0156,  0.024],
                 [0.006,   0.17,   0.018,   0.026,   0.04],
                 [0.0108,  0.018,  0.1324,  0.0468,  0.072],
                 [0.0156,  0.026,  0.0468,  0.1776,  0.104],
                 [0.024,   0.04,   0.072,   0.104,   0.28]])

te = np.sqrt((x - b).T @ Sigma @ (x - b))
```
And the result is:
```
0.06268173577685926
```

We also need to define the *excess greeness* of our portfolio, that is the positive or negative deviation from our benchmark greeness:

\begin{equation}
G(x|b) = (x - b)^T G
\end{equation}

Below is the code to compute the excess greeness of our portfolio:
```python
excess_greeness = (x - b).T @ G
```

And the result is:
```
-0.008500000000000021
```

### Climate Investing Objectives

In the previous example, we compared the climate performance of a portfolio against a benchmark, with predefined portfolio's weights.

However, the objective of investors is to improve the portfolio's greeness while controlling the tracking error volatility.

Denoting $\gamma$ as the risk tolerance parameter, we have the following optimization problem:

\begin{equation*}
\begin{aligned}
& x^* = 
& & argmin \frac{1}{2} \sigma^2 (x |b) - \gamma G (x | b) \\
& \text{subject to}
& & 1_5^Tx = 1, \\
&&& 0_5 \leq x \leq 1_5.
\end{aligned}
\end{equation*}

Where the constraints simply mean that the sum of the resulting weights $x$ need to be 1 and that individual weights $x_i$ need to between 0 and 1.

Varying the parameter value of $\gamma$ gives you the efficient frontier of tracking a benchmark with a greeness objective.

Below we install and import the `qpsolvers` package and formulate the QP problem of the optimization:

```python
!pip install qpsolvers 

from qpsolvers import solve_qp

def portfolio_tilting(Sigma, G, b, gamma):
  """QP formulation"""
  
  P = Sigma # Q 
  q = - (gamma * G + Sigma @ b) # R, minus because we want to maximize it!
  A = np.ones(len(G)).T # A
  b = np.array([1.]) # B
  lb = np.zeros(len(G))
  ub = np.ones(len(G))
  print(q)
  x = solve_qp(P = P,
               q = q,
               A = A, 
               b = b,
               lb = lb,
               ub = ub,
               solver = 'osqp')
  
  return x

```

Below is the code to draw the efficient frontier, varying $\gamma$ value:

```python
results_excess_greeness = []
results_te = []

from numpy import arange

list_gammas = arange(0,1.2, 0.01)


for gamma in list_gammas:
   x = portfolio_tilting(Sigma = Sigma, 
                    G = G,
                    b = b,
                    gamma = gamma)
   excess_greeness, te = get_perf_tilting(x = x, 
                                       b = b, 
                                       Sigma = Sigma,
                                       G = G)
   results_excess_greeness.append(excess_greeness)
   results_te.append(te)

import matplotlib.pyplot as plt


plt.plot(results_te, results_excess_greeness)

plt.xlabel("TE Volatility")
plt.ylabel("Greeness Excess")
plt.show()
```

![Efficient frontier](efficientfrontier.PNG)
