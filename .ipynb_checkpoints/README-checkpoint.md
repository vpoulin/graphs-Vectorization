Graph vectorization and visual exploration
==============================
_Author: Tutte Institute for Mathematics and Computing_

Implement a graph embedding strategy for attributed graphs. If nodes are already vectorized, we can treat each node and its neighborhood as hyperedges and vectorizer using Wasserstein. This forms a new vectorization of nodes, and we can repeat from there.
This sequence of node vectorizations {X_0, X_1, X_2, …} depends on neighborhood structures of larger and larger diameter - in a way similar to what is done by the WL kernel.
*Would concatenating these vertex vectors and using a Wasserstein similarity on the resulting bag of vertices make sense?*

NOTES
---------------

In the paper https://arxiv.org/pdf/1906.01277v2.pdf on Wasserstein Weisfeiler-Lehman graph kernels, they define two types of graphs defined, but I believe a third one would be useful (it is probably defined somewhere, but I did not see).
* **Attributed** graphs: there exists a function from nodes to vectors. We've been refering to this as the G+X problem for a long time. The vectors associated to each node could be 
* **Labelled** graphs: there exists a function from nodes to categorical labels (really, this is very much the same as the previous one if we encode categorical variables with vectors from the canonical basis - one-hot encodings)
* **Vertex-identified(?)** graphs : all graphs are defined on a common set of vertices. Or, there exists a bijection from nodes to categorical labels (a label is either present or not on each graph). This is very natural. For instance, graphs could be extracted from transport plans of goods in a given country: vertices being cities. A city can show up on many of these graphs, so same vertex shows up on many graphs. Vertices are uniquely identified over all graphs. Although this can be captured by the labelled case, it is fondamentaly different enough to deserve its own category as it could benefit from different embedding strategies - depending on the task.

, we know that the label distribution is key to the usefulness of this approach. Let's look at the two extreme cases. 
* If all labels are the same, node embedding only captures node degrees of nodes reachable within paths of length h (h=0, 1, …, H). Similarities between nodes are all non-zero (since they all have the same label). The similarity between two nodes is larger than that minimal value iff all their neighbors have same degrees. So, if the two nodes have the same degree and all their neighbors have same degree. (If 2 neighborhoods are almost the same but not quite, we are back to the minimum similarity) 
* If all labels are different (nodes identified), nodes of the same graph are equidistant with the WL labelling scheme. Pairs of nodes of different graphs have a non-zero similarity iff the two nodes have the same label. They have a maximum similarity if the two subgraphs centered at each node of radius 2xH are identical. 

The categorical labels can be represented as one-hot encodings and then the extension to continuous attributes can be used on these indicator vectors. This allows for smoother similarity values. For instance, in the categorical case, the contribution to similarity of similar but not identical neighborhoods is zero. With hashing functions, it's all or nothing.


THE DATA
---------------
The data can be found here:

*  https://paperswithcode.com/dataset/enzymes


