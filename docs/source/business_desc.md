
# Using Business Description for a Firm-Level Estimate of Greeness

As Alessi and Battiston (2022) {cite:p}`2022:AlessiBattiston` have choosen to follow the EU Taxonomy framework, while data are unavailable at the issuer level, they propose a top-down approach to measure firm greeness, by computing a taxonomy alignment coefficient with data at sector level (NACE sectors). 

As we want to estimate a firm level measure of greeness, we develop a text-mining approach.

More specifically, we define the firm-level exposure to a given activity as *the semantic similarity between the business description of the firm and the description of the activity.*

## Embeddings with a Sentence Transformer Model

We first need to transform the business description and activities descriptiom from our taxonomy from unstructured data (text) into a numerical representation. To do so, we use a Sentence Transformer model to create numerical vector representation of the meanings of the business description of the firm $i$, $D_{i}$ and the description of the activity $k$, $A_{k}$:

\begin{equation}
Emb^{D_{i}} = ST(D_{i})
\end{equation}

and 

\begin{equation}
Emb^{A_k} = ST(A_k)
\end{equation}

Where $Emb^{D_{i}}$ and $Emb^{A_k}$ are the numerical vector representation (embeddings) of $D_{i}$ and $A_{k}$, performed with the Sentence Transformer model $ST()$.

## Cosine Similarity

We want to be able to determine if the business description $D_{i}$ is related to one of the activity defined in our taxonomy.

As we now have numerical vector representations $Emb^{D_{i}}$ and $Emb^{A_k}$, we can apply principles from semantic search by determining the closeness of our two vectors. Recalling that dimensions of our vector representation relate to the underlying meaning of the text, computing the closeness of our vectors allows for determining the semantic similarity between our descriptions. 

One way to do so is by computing the cosine similarity between our two vectors. The cosine similarity measures the cosine of the angle of those two vectors. The closer the cosine similarity to 1 is, the more related the descriptions are:


\begin{equation}
cos(\theta_{D_{i},k}) = \frac{Emb^{D_{i}} \cdot Emb^{A_k}}{||Emb^{D_{i}}|| ||Emb^{A_k}||}
\end{equation}

For each business description, the cosine similarity is computed against each activity description in our taxonomy. The maximum cosine similarity measure is retained, with the associated activity.

## Greeness Measure

Finally, we adopt the following rule to attribute a *greeness* score to each issuer:

1. If $cos(\theta_{D_{i},k}) \leq 0.5$, a 0 score is attributed to the issuer.

2. If the activity $k$ for which the cosine similarity with the business description was the highest is among the brown activities in our taxonomy, a score of $-cos(\theta_{D_{i},k})$ is attributed. A score of $cos(\theta_{D_{i},k})$ is attributed if the activity is among the green activities in our taxonomy.

## Results 

Below is the Python code 