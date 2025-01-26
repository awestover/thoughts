A network decomposition is a nice partition of your graph makes many local distributed problems tractable (e.g., coloring).

**Strong network decomposition**:
You partition your graph into parts, such that the induced subgraph on each part consists of a collection of low-diameter graphs
![[Pasted image 20240801105217.png]]



It's usually ok to have a **weak network decomposition** instead. It's the same thing, except instead of having connected components of diameter $D$, you have **clusters** where the distance between each pair of points in a cluster (allowing you to use edges in $G$) is at most $D$, and the clusters can't have any edges between them (but ofc there might be a path that leaves the part and then comes back into a new part).
