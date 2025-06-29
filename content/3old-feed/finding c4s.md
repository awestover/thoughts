
> [!tip] Theorem
> Let $t$ be the number of C4s in your graph.
> There is a C4 detection algorithm with the following guarantees:
> If C4 exists, outputs yes with high probability. If C4 not exists outputs no. 
> If $t>n^2$ the running time is $O(n^{3/2})$.
> O/w running time is $\min(m^{4/3}, n^{2}/C_{4}^{1/4})$.


Some notes: 
WMA maxdegree  $\sqrt{ n }$ average degree $\sqrt{ n }/100$.
Because degree profile of $C_{4}$-free graph looks nice. 

The second fast algorithm is by BFSing out of vertices in a random order until we find a C4.
The first fast algorithm is by subsampling the vertices. 
We needed $t$ to be big to have reasonable variance here. 
