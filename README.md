Graph vectorization and visual exploration
==============================
_Author: Tutte Institute for Mathematics and Computing_

Trying to implement a graph embedding strategy. If nodes are already vectorized, we can treat each node and its neighborhood as hyperedges and vectorizer using Wasserstein. This forms a new vectorization of nodes, and we can repeat from there.
This sequence of node vectorizations {X_0, X_1, X_2, …} depends on neighborhood structures of larger and larger diameter - in a way similar to what is done by the WL kernel.
*Would concatenating these vertex vectors and using a Wasserstein similarity on the resulting bag of vertices make sense?*


THE DATA
---------------
The data can be found here:

*  https://paperswithcode.com/dataset/enzymes


