Here are a couple of brief notes on a potentially interesting and potentially open problem.

**Blocked Cuckoo hashing** works as follows:

We have a directed graph $G$. 
We want to maintain the property that every vertex has out-degree at most $d$

We want to insert a new edge, and we're trying to decide on its orientation.
- If one of the orientations is valid, just do that. 
- If neither are, choose one randomly, and then choose a random edge going out of the now too heavy vertex to flip.
- keep iterating the process until everything is resolved, or until you decide that it's taking too long, in which case you can rebuild

**proposition**:
If there is **any** orientation of the edges, you can find it via an augmenting path.

**goal:** \
Bound the expected number of blocks probed in the course of an insertion by $f(\varepsilon ^{-1})$.

**note:** \
if you have capacity $d$ bins and $n$ items, then the number of bins (i.e., vertices in your graph) should be at least $(1+\varepsilon)n/d$.

Bill's paper suggests using buckets of size $d=\sqrt{ \varepsilon ^{-1} }$


---

Feb5 thoughts with N:

- We thought about whether or not such a random graph admits an orientation with good probability
- it's not obvious
- we thought about the classic non-blocked case. 
- where you just have two arrays and each guy gets hashed into both of them.  
- we weren't able to analyze it but it seemed cool. 
