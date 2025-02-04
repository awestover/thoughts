here are a couple of brief notes on a potentially interesting and potentially open problem.

note -- i might wanna ask an expert if this is really open

---

Blocked Cuckoo hashing works as follows:

We have a directed graph $G$. 
We want to maintain the property that every vertex has out-degree at most $d$

We want to insert a new edge, and we're trying to decide on its orientation.
If one of the orientations is valid, just do that. 

If neither are, choose one randomly, and then choose a random edge going out of the now too heavy vertex to flip.
keep iterating the process until everything is resolved. 

must be some rule for stopping the loop

but also there was some claim that augmenting paths must exist. like they just did a bfs?
im confused. 


goal: 
Bound the expected number of blocks probed in the course of an insertion by $f(\varepsilon ^{-1})$.


note: 
if you have capacity $d$ bins and $n$ items, then the number of bins (i.e., vertices in your graph) should be at least $(1+\varepsilon)n/d$.



I think Bill's paper suggests using buckets of size $\sqrt{ \varepsilon ^{-1} }$


