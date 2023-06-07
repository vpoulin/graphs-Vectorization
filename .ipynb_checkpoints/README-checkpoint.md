Graph vectorization and visual exploration
==============================

Implement a graph embedding strategy for attributed graphs - this is pretty much cut-and-pasted from the work I did for embedding hyperedges. Here, nodes are already vectorized, provided from the data - see below. 

What we are trying here for graph embedding is similar to what has been tried with the Wasserstein Weisfeiler-Lehman graph kernels. See https://arxiv.org/pdf/1906.01277v2.pdf. However, instead of using Weisfeiler-Lehman kernels for averaging the appropriate vectors at each node, we iteratively use Wasserstein vectorizations on vertex neighborhoods to generate a sequece of node vectors. 

We will treat each node and its neighborhood as hyperedges, one hyperedge per node. We can then vectorize the hyperedges using the Wasserstein vectorizer. This forms a new vectorization of nodes which includes a similarity notion of its n1-neighborhood with other nodes of other graphs. And we can iteratively repeat this process to capture information about neighborhoods of larger radii.
This sequence of node vectorizations {X_0, X_1, X_2, …} depends on neighborhood structures of larger and larger radii - in a way similar to what is done by the WL kernel. Instead of averaging the node vectors of neighbors, we vectorize the sets of vectors. 

We concatenate these vertex vectors to obtain a topologicaly informed vertex representation and use Wasserstein to vectorize the graphs as the bags of these vertex vectors.

THE DATA: ENZYMES.
---------------
We have 600 small graphs. There is a vector associated to each node for all graphs - this vector is given in the dataset. We want to obtain a graph vector representation. The downstream task is enzyme/graph classification: 6 classes in this dataset. The classes are balanced: 100 graphs in each class.

    https://github.com/snap-stanford/GraphRNN/tree/master/dataset/ENZYMES
    https://paperswithcode.com/sota/graph-classification-on-enzymes

The data can be found here:

*  https://paperswithcode.com/dataset/enzymes


NOTES ON WWL PAPER
---------------

In the paper https://arxiv.org/pdf/1906.01277v2.pdf on Wasserstein Weisfeiler-Lehman graph kernels, they define two types of graphs, but I believe a third one would be useful (probably already defined somewhere).
* **Attributed** graphs: there exists a function from nodes to vectors. We've been refering to this as the G+X problem for a long time. The vectors associated to each node could be 
* **Labelled** graphs: there exists a function from nodes to categorical labels (really, this is very much the same as the previous one if we encode categorical variables with vectors from the canonical basis - one-hot encodings)
* **Vertex-identified(?)** graphs : all graphs are defined on a common set of vertices. Or, there exists a bijection from nodes to categorical labels (a label is either present or not on each graph). This is very natural. For instance, graphs could be extracted from transport plans of goods in a given country: vertices being cities. A city can show up on many of these graphs, so same vertex shows up on many graphs. Vertices are uniquely identified over all graphs. Although this can be captured by the labelled case, it is fondamentaly different enough to deserve its own category as it could benefit from different embedding strategies - depending on the task.

With Wasserstein Weisfeiler-Lehman graph kernels on categorical labels, the label distribution is key and affects the information captured. Let's look at the two extreme cases: all labels are the same or all are different. 
* If **all labels are the same**, node embedding only captures node degrees of nodes reachable within paths of length h (h=0, 1, …, H). Similarities between nodes are all non-zero (since they all have the same label). The similarity between two nodes is larger than that minimal value iff all their neighbors have same degrees. So, if the two nodes have the same degree and all their neighbors have same degree. (If 2 neighborhoods are almost the same but not quite, we are back to the minimum similarity) 
* If **all labels are different** (nodes identified, so we will talk about node ids instead of node labels). Nodes of the same graph are equidistant with the WL labelling scheme. Pairs of nodes of different graphs have a non-zero similarity iff the two nodes have the same id. Two nodes of different graphs have a larger similarity if the two subgraphs centered at each node of radius 2xh (for a given h) are identical - same nodes, same edges. So this is a very strict notion of similarity for vertex-identified graphs. 

The categorical labels can be represented as one-hot encodings and then the extension to continuous attributes can be used on these indicator vectors. This allows for smoother similarity values. For instance, in the categorical case, the contribution to similarity of similar but not identical neighborhoods is zero. 





