**Theorem**: Suppose $S$ is a collection of subsets that are top faces of a $\lambda$-one-sided $k$-partite $1/k^3$-high dimensional  expander. 
Then given a family of local functions which passes the agreement test with good probability, then, ...something...

Some definitions: 
Let $X$ be the set of edges in a hypergraph.
- $X$ is a **Simplicial Complex** if it is downwards closed. (i.e., $A\subset B, B\in X \implies A\in X$.)
- $X(\ell)$ refers to edges of size $\ell+1$.
- Sometimes hyperedges are also called **faces**. 
- A $d-1$ dimensional simplicial complex has largest hyper-edge size of $d$. 
- The **top face** of a $(d-1)$-dimensional simplicial complex is $X(d-1)$.
- **Link:** the collection of faces that are disjoint from $\sigma$ whose union belongs to $X$.
- Ex: the *link* of a vertex is the set  of its neighbors. The link of an edge is the set of common neighbors of both vertices. 
- Note that the link of a face is a simplicial complex of lower dimension.
- A probability distribution on the top face induces a probability distribution on the vertices by selecting a top face and then passing to two random vertices in it. This gives a weighted graph called the **$1$-skeleton**.
- A $(d-1)$-dimensional simplicial complex is a $\lambda$-one-sided HDX if for every face $\sigma\in X(t)$  $t\le d-3$, the $1$-skeleton of the link $X_\sigma$ is a $\lambda$-one-sided expander graph, meaning the normalized eigenvalues of the random walk on the skeleton are at most $\lambda$. 

