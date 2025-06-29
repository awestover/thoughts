These are some notes on fine grained complexity theory, from Ryan + Virginia's class. 
I am not actually taking the class, but am reading the notes. 
These are now also notes from Ronitt's Sublinear time algorithms class. I actually am taking this class, but have chosen to digest the material mostly at my own pace. I enjoy  having occasional readings about cool algorithms and some occasional problems to work on, and an excuse to think about permutation avoidance again. 

I will abbreviate fine grained to FG and sublinear time to SL. 

# FG lecture 1
**Theorem**: SETH implies OV

**Proof**
Let $f$ be a SAT instance. 
Break variables into two parts $A,B$ each of size $n/2$. 
Make all partial assignments. 
Then given a partial assignment make a word where you put a $0$ at the spot if the partial assignment satisfies that clause, and 1 else. 
You also have two bits at the front of the vector to indicate whether you are a partial assignment to A or to B. 
Then, if two vectors are orthogonal, it means that at every spot there is at least one $0$, i.e., one of the assignments satisfies the clause. 

**Theorem** $3/2$-approximating diameter in $m^{2-\varepsilon}$ is impossible under SETH.

**Proof**
Three part graph. 
Left: vertex for each vector $v_{1},\dots,v_n$
Right: vertex for each vector $v_{1},\dots,v_n$
Middle: Vertex for each coordinate.

Edges:
Connect vector to the coordinates where it has a $1$.

Also add two "star" vertices. 
Left star vertex has edge to all middle vertices, left vectors and other star
Right star vertex has edge to all middle vertices, right vectors and other star.

Then diameter is either 2 or 3. 3 iff orthogonal pair. 

# lecture 2 

**Theorem** Dominating set in $n^{2k+\varepsilon}$ time for $k\ge 4, \varepsilon>0$.

**Proof**:
$A[S,i]$= can you get to $i$ in 0 or 1 step from $S$?
$AA^{T}$ does meet in the middle. 
You just need to check if $AA^{T}$ is the zero matrix.

**multipoint evaluation**
We can evaluate a polynomial at $k$ points in $\tilde{O}(n+k)$ time.
This is FFT.

**interpolation**
Given $n$ points you can fit a degree $n$ polynomial to them in $\tilde{O}(n)$ time.

**multiplication**
You can multiply degree $n$ polynomial in time $\tilde{O}(n)$.

**3-sum on small magnitude numbers**

**Theorem**
Suppose we have a 3-sum instance where the numbers lie in $[-M,M]$. 
(you wanna tell if there are 3 summing to zero)
We can solve this in $O(M \polylog(M) + n)$ time.
Let's go to 3-partite 3-sum with sets $S_{1},S_{2},S_{3}$. Add $M$ to all the numbers and make polynomials. 
Then you form polynomial $\sum_{k\in S_i} x^{k}$.
You multiply the polynomials, and check if the coeff of $3M$ is non-zero.

**4-Russians**
Matrix vector multiplication with pre-processing. 
Break into blocks. For each block, Store $A_{i,j}\cdot w$ for every $w\in \{0,1\}^{b}$.
This takes preprocessing time $n^{2}2^{b}$ and lets us do evalutaiton in $(n/b)^{2}$ time.

# lecture 2 again, but different
ETH: 3-sat requires $2^{\delta n}$ time for some $\delta$.

### independent set

**Example**:
Encoding a 3SAT instance as an indepset instance.
![[seth.png]]

Problem: this doesn't let us prove an exponential lower bound because we blew up the problem instance too much. 

Like you start with a n var m clause 3SAT instance, and produce an m vertex graph. 
$2^{\delta |V|}$ IS algorithm would give you $2^{\delta m}$ 3SAT algorithm. which is totally possible.

**one hope:** 
maybe there is some OP sparsification that we can do.
- probably not.

but,  there is this lemma:

IPZ sparsification lemma:
there is a $2^{\varepsilon n} \text{poly}(n)$ time algorithm that takes a $k$-CNF formula on n vars and produces $2^{n\varepsilon}$ many $k$-CNFs such that F is satisfied iff the or of these is satisfied. Each of these formula guys has n vars and $n (k/\varepsilon)^{O(k)}$ clauses.

yup now the IS thing works.

### SETH
For all $\varepsilon>0$, exists $k$ such that $k$-SAT requires $2^{(1-\varepsilon)n}$ time.

**claim: SETH implies ETH**
**even stronger claim**:
If there is any $k$ for which $k$-SAT requires expo time then 3-SAT requires expo time.

Again we use sparsification.

Standard kSAT --> 3SAT reduction:
$x_{1} \lor x_{2} \lor x_{3} \lor x_{4}$ create some auxilliary vars.
$(x_{1} \lor x_{2} \lor y_{1}) \land (\neg y_{1} \lor x_{3} \lor x_{4})$ 

Turns a n var m clause kSAT instance into a $mk$ var $mk$ clause 3SAT instance.

So first we sparsify. 
Turn n var m clause ksat into a bunch of n var ~n clause kSAT instances, 
and then turn these into kn var ~kn clause 3SAT instances. 

So if 3SAT were subexponential we would get way too fast of an algorithm for kSAT like this under SETH.

### ksum

**Theorem**: if for all $\varepsilon>0$ there exists $k$ such that $k$-SUM on small numbers takes time $O(n^{\varepsilon k})$ then ETH is false.

**proof**
Let's just work with a sparse formula, by virtue of sparsification lemma.
Turns out to be better to work with 1-in-3 SAT (exactly one var in each clause is satisfied to make the clause true). But this only costs us constant blowup to do so its fine.

We do the standard "one-hot encoding" technique.
Split the vars into k groups
make the numbers: 
- have some part of the number to indicate whether you've done a partial assignment from each group
- then have some other part for making sure the clauses are all satisfied.

 ah this is clever. 
### Sparsification

OBS: Branching can reduce the number of clauses
for example $x \lor y  \lor z$ and $x\lor y' \lor z'$ if you branch on $x$ then either you can delete both clauses or you can make them be just 2-variable clauses.

1. consider the variable that appears in the most clauses
2. the fact that we're not sparse yet means that we can branch and kill some clauses.

# SL lec 3

**connected components estimation**

Algorithm is as follows:
```
Repeat a couple times:
	pick a random vertex v
	BFS out of v for at most 100/epsilon steps, 
	stop early if you exhaust the connected component.

	define nhat[v] to be either the size of the connected component that v is in, or 100/epsilon if the connected component is big. 

Use the nhat's to guess the number of connected components.
```

So the key idea behind this estimator is the following observation:
If we let $n_v$ be the number of vertices in $v$'s connected component and $C$ be the number of connected components then we have:
$$
\sum_{v} \frac{1}{n_v} = C
$$
So we are going to compute some numbers $\hat{n_v}$ with the following property: 
$$
\left|\frac{1}{n_v} - \frac{1}{\hat{n}_v}\right| < \varepsilon/2.
$$
Actually, we kind of already talked about how to do this above. 
Anyways. 

So, 
$$
\left|\mathbb{E}_v \left[ \frac{n}{\hat{n}_v} \right] - C\right| \le n\varepsilon/2.
$$
And then we need to go into some kind of computation to show that if we have a good approximation on average and we iterate a couple times we can get a good approximation with decent pr. 
In particular you can just do a Chernoff bound. 
Specifically, Hoeffding bound says: if $X_{1},\dots,X_k$ are independent random variables taking values in $[0,1]$, then the chance that their average deviates from it's expectation $\mu$ by more than $\delta \mu$ is at most $2\exp(-\delta^{2}\mu/3)$. 
For us this gives a constant pr of the average successfully being good, which is what we wanted. 

**MST estimation**
Kruskal's algorithm says that to find an MST you can do the following stuff:
- add as many weight $1$ edges as is possible.
- then do weight $2$
- etc.

This allows us to relate MST estimation to estimating the size of some connected components defined by taking all edges with weight smaller than some threshold.

# SL lec4

Today we are going to talk about a sublinear (in $m$) time algorithm for $\Delta+1$ coloring a graph.

**Sparsification lemma**
If you sample $100\log n$ colors from $\{1,\dots,\Delta+1\}$ that each vertex is allowed to use, then probably the graph is colorable using colors from these lists. 

**proof:**
We'll do it below. 

**Claim:** 
Once we've sparsified the color palettes, we can ignore edges $uv$ where $u,v$ have disjoint palettes. 
Doing so leaves us with only $n\log ^{2} n$ edges on average (and even whp).

**Proof**
For each vertex $v$, let's count the number of times a neighbor has a color from $v$'s palette; note that if a neighbor shares multiple colors, we get points for each of these. 
Let $k=\log n$ be the size of the color palettes.
If we fix a neighbor $w$ and a color $c$ from $v$'s palette then $w$ has color $c$ in it's palette with probability $k/\Delta$.  Thus, the expected number of times a neighbor shares one of $v$'s colors is like $k^{2}$.
Thus, the expected number of neighbors of $v$ is $k^{2}$

Now we give a $\tilde{O}(n^{2}/\Delta)$ time algorithm for coloring. 
Note that the trivial greedy algorithm runs in time $O(n\Delta)$, so combining both algorithms we can do $O(n\sqrt{ n })$.

**Algorithm**

- Sample some random colors for everyone.
- Construct sets $V_c$ which are the set of vertices with color $c$ in their palette.
- Construct the sparse edge set $E'$ as follows:
- For each $c \in [\Delta]$, 
	- For each pair of vertices $v,w\in V_c$
	- Add $vw$ to $E'$ if $(v,w) \in E$
This takes time 
$$
\Delta \cdot \binom{(n\log n)/\Delta}{2} = \tilde{O}(n^{2}/\Delta).
$$
Finally, do the greedy algorithm in this sparse graph. 


**sparsification lemma**\
We don't actually prove the sparsification lemma. we prove something a bit weaker: we allow $2\Delta$ colors.
Anyways the proof is as follows:
- run the greedy algorithm 
- show that it fails with $1/n^{3}$ pr at each step 
- union bound

##  SL Lec 5

In these notes VC will refer to vertex cover (a set of vertices that hits every edge).

> Q: How to approximate min VC size in sublinear time?
> A: We will give a distributed algorithm for computing a small vertex cover. 

More precisely, after running the distributed algorithm, every vertex should raise a flag saying whether or not they wish to participate in the VC. 

We work in the LOCAL model of distributed computation.

We let $\Delta$ denote the max degree of the graph.

> [!tip] Distributed Vertex Cover Algorithm
> 
```python
for each edge e:
	Initialize x[e] = 1/Delta
for i = 1  ... log_{1+gamma}(Delta)	:
	for each v that has sum_{edges e incident to v} x[e] >= 1/(1+gamma):
		add v to the VC
		for all e incident to v:
			freeze e
	for each unfrozen edge e: 
		multiply x[e] by 1+gamma
```

**Claim 1:** This algorithm outputs a VC. 
**pf**: Endpoints of frozen edges are added to VC. All edges are eventually frozen.

**Claim 2**:
We can interpret the final values of $x_e$ as a fractional matching. This just means that $\sum_{e\ni v} x_e \le 1$ for all $v$.
This fractional matching will satisfy: 
$$\text{size of our VC} \le 2(1+\gamma)\sum_e x_e.$$

**Proof**
We only increase edge weights if we're sure that it's safe to do so. 
The fact that our VC size is not much larger than the fractional matching size can be seen as follows:
- Suppose we form a sum $s$ by starting at $s=0$ and then adding $x_e$ to $s$ whenever we freeze an endpoint of $e$. 
- Because we freeze every edge eventually, at the end  we will have $s = 2 \sum_e x_e$.
- The amount that we increment $s$ by when we freeze edges incident to some vertex $v$ is at least $1/(1+\gamma)$ by design.
- Thus, $2\sum_e x_e \le (1+\gamma)|VC|$.

**Claim 3**: 
The value of a fractional matching gives a lower bound on the size of the minimum vertex cover.

**pf**
This is super obvious for matchings. 
For fractional matchings, assign each edge to an endpoint in the VC. Have each VC vertex aggregate its assigned edges weights.
The total weight aggregated by the VC vertices is $\sum_e x_e$, and it is at most $1$ per vertex. QED.

## SL Lec 6
Last class I think we got a $2+\varepsilon$ approximation to min VC. 

CAUTION: maximal $\neq$ maximum

In this lecture we will talk about approximating the size of a maximal matching (?)
Let $M$ be a maximal matching and let $V^*$ be a minimum Vertex Cover. 
Observe: $|M| \le |V^{*}| \le 2|M|$.

**Claim**: a maximal matching has size at least $m/(2\Delta)$

**pf**: each edge you take eliminates fewer than $2\Delta$ edges as options. 

Greedy Maximal matching alg: 
- take edges until you can't take any more

Now we have an interesting idea: 
- Imagine we had an oracle that would tell us whether an edge was in the matching. 
- Would that help?

For estimation it totally would. 
We would just sample some vertices to estimate the probability that a random vertex is an endpoint of an edge of the matching. 

And we add a bit just to guard against terrible underestimates. 

ok fine so this'd be great if we had such an oracle. 

Intuitively: maybe we don't need to actually run greedy to figure out it's bx on an edge?

Here's a weird (but useful) recursive way of writing out our greedy algorithm

```python
def matched(e):
	for all edges e' adjacent to e that come before e in ordering:
		if matched(e'):
			return False (e not matched)
	return True (e matched)
```

Let's try seeing how expensive this is if we do a random ordering. 

Under a random ordering, the probability of going on some specific path of length $k$ is $1/k!$.
The number of paths of length $k$ from an edge is at most $(2\Delta)^{k}$.
Thus, the expected cost of the recursion is: 

$$
\sum_{k\ge 0} \frac{(2\Delta)^{k}}{k!} \le 2^{O(\Delta)}.
$$
So we can get a $2^{O(\Delta)}\varepsilon^{-O(1)}$ time algorithm for 2-approximate VC (i.e., for computing the size of some maximal matching). 

Is this good? Well, I wouldn't use it unless $\Delta < o(\log n)$. But in that case, sure it's good. 

### FG 09-27

Virginia showed us a neat trick in class today: breaking the problem up into lots of little parts. 
Specifically, the problem was **dynamic reachability** and she wanted to give some combinatorial lower bounds based on triangle detection / BMM.
And we could get a simple lower bound pretty easily, but chopping up the problem in a nice way let us get a lower bound even with arbitrary polynomial precomputation. 

So that was nice.

### SL 7

Property testing for graphs:
- "$\varepsilon$-far" will mean  "can edit $\varepsilon \Delta \cdot n$" edges to get in class; here $\Delta$ is max degree limit.

**Kuratowski theorem**
planar iff no K5 or K33 minors  
minor = graph obtained by contracting edges

**Def**
$(\varepsilon,k)$-hyperfinite if can remove $\varepsilon n$ edges to get a graph where all connected components have size $\le k$.

i.e., Remove few edges to break graph into tiny pieces.

**Theorem**
For all $\varepsilon,\Delta$, exists $c$ st
planar graph of max degree $\Delta$
is $(\varepsilon \Delta, c/\varepsilon^{2})$ hyperfinite.

So we can use the following approach for testing planarity:
1. Find a way to remove a small number of edges in order to split G into small CCs.
2. If step (1) fails, we know we aren't planar.
3. If step (1) succeeds, we can just check one of these little CCs for whether or not it's planar.

Similar to what we did for matching last time, we're going to break the problem into two parts:
making a "Partition Oracle" and using a Partition Oracle.

**Partition Oracle**
- input: vertex $v$
- output: Name of the part of the partition that $v$ belongs to

Require: partitions are small and connected.
Also require: if $G$ is planar, then at most $\varepsilon \Delta n/4$ edges cross partition.

It's pretty straightforward how to do testing once you have such an oracle.

**Building the oracle**
1. global partitioning strategy
2. locally implement it

$S$ is a $(\delta,k)$-isolated neighborhood of node $v$ if 
- $v\in S$
- $S$ is connected \
- $|S|\le k$ 
- edges connecting $S$, $\bar{S}$ are at most $\delta |S|$

**Claim**: in a planar graph, in any decent partition *most* nodes have $(\varepsilon \Delta, c/\varepsilon^{2})$-isolated neighborhoods.
I don't follow this yet, but let's keep going.

**Global Partitioning algorithm** (note: I don't think we actually do this, this is just how you would in theory glue stuff together?)

```python
order vertices randomly
partition = []
for i in range(n):
	if v_i still in graph:
		if exists (delta,k)-isolated nbrhd of v_i in remaining graph:
			S = this neighborhood
		else:
			S = {v_i} # hopefully this is unlikely
	partition.append(S)
	Remove S from graph
```

We want to show that this algorithm works reasonably well.
**Lemma**:
A $(1-\varepsilon)$-fraction of nodes enjoy $\varepsilon,\Delta\varepsilon^{-3}$ isolated nbrhoods.

**Proof sketch**
Using the hyperfiniteness theorem, we know that there is a partition such that if we rm $\varepsilon \Delta n$ edges, then every CC in the resulting graph has size at most $O(\varepsilon^{-2})$.

For each vertex $v$, let $X_v$ be the number of edges that touch the CC that $v$ ends up in once we've cut the edges.
Then, $\sum X_v \le \varepsilon \Delta n \cdot O(\varepsilon ^{-2})$.
In other words, 
$$
\mathbb{E}[X_v] \le O(\Delta/\varepsilon).
$$
By Markov, 
$$
\Pr[X_v \ge \Delta \varepsilon ^{-3} \cdot \varepsilon] \le O(\Delta/\varepsilon) / (\Delta \varepsilon ^{-2}) < O(\varepsilon).
$$

So basically the global algorithm is pretty good.

Now, this global algorithm is basically just greedy.
So I'm hopeful that if we do the same random ordering trick that we did last time, maybe good things will happen. Like we can cut long range dependencies and just locally simulate the greedy thing.

**Local Simulation of Partition Oracle**
Yeah so the way we do it is indeed just like last time. (this is not to diminish the technique --- on the contrary, I think the broad-applicability of this technique is exciting!)
But even having seen last time it took me a minute to see this so let me write it down.

Here's a recursive version of the local oracle:

```python
# fix a random ordering of the vertices
# input: vertex
# output: a small neighborhood of v that has few edges to the outside world
# note: these neighborhood things need to be **consistent** across calls to the oracle
def oracle(v):
	for each vertex w within distance k of v, where w has lower rank than v:
		run oracle(w)
	if some nearby w already claimed v:
		output the part that claimed v
	else:
		just do a little BFS out of v (respecting the nearby guys who have their own thing going on) and decide on a neighborhood for v
```

Running time analysis is as follows:
$$
\sum_{i\ge 1} (\Delta ^{k})^{i}/i! = \exp(\Delta ^{k})
$$
for recursion-y stuff. 
(Also, who thought that doubly exponential running time was a good idea?)
(lol I guess think of $\Delta,\varepsilon$ as constants, then this is pretty good).

Anyways, then we have to do the little BFS thing, but this is just small money in the total running time.

Apparently you can do much better dependence on $\Delta, \varepsilon$. ok.

## property testing lower bounds

**Problem:**
Given $v\sim \{0,1\}^{n}$, find a vector $\hat{v}$ such that $||v-\hat{v}||_1 \le n/8$.
Queries that you're allowed to make:
You can choose any vector $w\in \mathbb{R}^{n}$ and ask for $\mathsf{sign}(v\cdot w)$.

**Claim**:

You can't solve this problem using fewer than $o(n/\log n)$ queries.

**Proof**: 
Suppose that you had some (potentially randomized) algorithm that could output a vector $\hat{v}$ such that $\Pr_i [v_i = \hat{v}_i] \ge 7/8$ in time $f(n)$. Then, we could by symmetry have a randomized algorithm with the property that for each $i$, $\Pr_{alg}[v_i = \hat{v}_i]\ge 7/8$.
Then, if we ran this $10 \log n$ times and took a majority vote, we would have an estimate that is correct with probability $1-1/n^5$ for each coordinate of $v_i$.
Taking a union bound, we have that with probability $1-1/n^4$ we have actually just recovered $v$.
This is information theoretically impossible to do faster than $\Omega(n)$ -- we only get one output bit per query. 
Thus we must have $f(n)\cdot 10\log n \ge \Omega(n)$. 
So $f(n)\ge \Omega(n/\log n)$.

**Remark**:
I'm pretty sure that you actually need $\Omega(n)$ queries, but haven't thought about how to make the lower bound tighter.

## FG -- matrices

Solve LCA in $n^{2.5}$ assuming $\omega=2$.
First, compute all of these guys:
$$
\left\lfloor  \frac{\max\{ j\mid A_{ij}=B_{jk} \}}{\sqrt{ n }}  \right\rfloor 
$$
Then just do something easy to get
$$
\max\{ j\mid A_{ij}=B_{jk} \} 
$$


## SL SPANERS

**Def**: $k$-spanner is a subgraph $H$ such that $d_H(u,v)\le k\cdot d_G(u,v)$

Remark suppose $G$ is a $C_{4}$-free bipartite graph with about $n^{1.5}$ edges -- note that such graphs exist.
Suppose we remove an edge from $G$. Then, there is no path of length $\le 3$ between the vertices that we just deleted an edge between. 

This is a proof that $n^{1.5}$ edges are required to get a $3$-spanner. 

Similar lower bounds for other $k$ assuming girth conjecture. 

Today, we give an LCA algorithm for computing a $3$-spanner with $O(n^{1.5}\log n)$ edges.
We will be able to make queries of the form "is *this* edge in the spanner graph?" in $n^{3/4}$ time.

First idea: 
We can delete an edge from every triangle.

For today: 
We assume max-degree $n^{3/4}$.
ok, fine.

First, how would you even do this globally?

1. Pick random set $S$ of size $\sqrt{ n }\log n$
2. Then, $S$ has neighbor to all high degree vertices. 
3. Now we construct a graph as follows:
4. Add all edges touching $S$
5. Add all edges touching low degree vertices
6. Add ONE edge from $v$  to all adjacent clusters

This graph clearly has a very small number of edges.

Suppose $(u,v)$ is an edge and both $u,v$ are high degree vertices. 
If $u,v$ are in the same cluster, then if $w$ is the cluster center we have a length $2$ path $uwv$.
If $u,v$ are in different clusters, then let $w$ be the cluster center of $v$'s cluster. We know that $vw$ is an edge, and that $u$ has an edge to some $w'$ which is in $v$'s cluster. 
Overall, we deduce that $u,w',w,v$ is a path that exists.

**side remark** -- I quite enjoy some algorithmic graph theory! brings back some good memories. anyways.

other side remark: 
in our model of the graph, we allow "degree probe" i.e., asking in constant time what the degree of a vertex is.

Anyways, that was the global algorithm. Now, let me try to guess how I'd local-ify it. 

So, in a LCA you have query access to some huge random string that is persistent.
Like your algorithm is parameterized by this random string, and just has to work for most choices of this random string.

So ok, each vertex picks whether it's a center or not. 
So now it's pretty easy to pick out the edges touching low degree vertices, and also the edges touching centers. 
Also, it's pretty easy for every vertex to choose it's cluster center that it wants to join -- just join the cluster who's cluster center has the lowest id amongst all clusters that you're connected to 

And then each vertex chooses the lowest id neighbor in each cluster as it's connection in that cluster. By which I mean, iterate over your neighbors in order, maintain a list of clusters that you've already connected to, and take an edge whenever it goes to a new cluster -- and ofc at that point you mark that cluster as having been visited.
Ah, but this is kind of expensive, because it already takes like ~$\sqrt{ n }$ time to even find the cluster center of a vertex... so this is probably not quite gonna cut it.


Anyways, let's think about it.
Type 2 query -- asking if an edge $uv$ should exist in virtue of $u$ being the lowest id neighbor of $v$ which is a center. 
This should take about time $\sqrt{ n } \log n$ because you just go through neighbors of $v$ until find one, but you're very likely to find one after like $\sqrt{ n }$ tries.

Type 3 query -- edge to adjacent cluster??

---

Anyways, that didn't quite work so we're changing things up a bit.

Let $C_u$ denote the set of centers that are in the first $\sqrt{ n }$ indices of $u$'s neighbor list.
Fact: if $u$ is high degree, it'll have between $1$ and $\log n$ many such centers.

So, now we're going to connect $u$ to all of $C_u$. 
This makes it really easy to check, given a cluster center $v$ whether $u$ is in $v$'s cluster  -- although this is because we have some kind of weird graph model where I'm allowed to ask for the index of $u$ in $v$'s adjacency class. 
It still takes like $O(\sqrt{ n })$ time to find the cluster centers of a vertex, but checking mmembership is super easy now.

now, here are the other edges that we add: 
- For each edge $uv$
- Let $S$ be the set of neighbors of $u$ which are lower id than $v$
- For each $x\in S$, compute $C_x$ and cross off $C_x \cap C_v$  as "we've already met"
- If by the end of this process there are some things in $C_v$ that haven't been crossed off, then we say that the $uv$ edge is **cool** and we add it. Else, we don't add it.

This clearly works, but it's too slow still

Now finally, here's the smarter method that's actually efficient.


Yeah, so we were just being a little bit silly above. Here's how you can do it fast:

1. For each edge $uv$
2. Let $S$ be the set of neighbors of $u$ with lower id than $v$
3. For each $x\in S$
4. For each $w\in C_v$
5. Check if $w$ is a cluster center of $x$ -- if so cross $w$ out
6. Check whether any $w\in C_v$ didn't get crossed out. 

This takes time $O(\Delta \log n)$.

Maybe later we'll fix the issue about the max-degree being too large. 
Oh lol actually we'll fix it on the homework!

## maximal independent set -- local computation algorithm

This one had some pretty neat techniques. 
The main one I liked was, somehow breaking the problem into not-super-overlapping sub-parts. 

First, we give a distributed algorithm for the problem.

```python
Each round:
	Every vertex randomly tries to color itself with pr 1/(2 Delta)
	Coloring "goes through" if no neighbors also try to color themselves
	if coloring goes through:
		kill the neighbors
```

We're going to run this algorithm for $\Delta \log  \Delta$ steps. At that point NOT EVERYONE will be handled.  
Some ppl will be killed, some will be put in the MIS. But some are still undecided. The number of undecided guys is $n/\Delta^{c}$. 
It turns out that these guys are "well spread out" , and that this is good.

So our LCA algorithm for MIS is going to work as follows --
First run Luby for $\Delta \log \Delta$. 
Then do BFS to find $v$'s live CC. 
Then brute force find lex first MIS of the live CC.

We will show: 

**Lemma**
Size of CC is less than $\poly\log(\Delta) \cdot \log n.$
This is actually pretty complicated and cool to show.

Define $A_v$ to measure whether $v$ survives all rounds
Define $\neg B_v$ to measure whether there is a round where $v$ tries to color its self and no neighbors try to.
Note that $\neg B_v$ doesn't necc imply that $v$ is in the MIS bc $v$  is allowed to "try to color itself" after having already been killed. This was kind of  a weird notation choice but whatever.

Then, if $v,w$ are distance at least $3$ apart, $B_v, B_w$ are independent rvs.

**PLAN**
- large CC --> many nodes at dist 3 apart
- nodes at dist 3 apart --> unlikely to simultaneously survive

Still a bit complicated.

CC --> just only think about a spanning tree for the CCs.

**CLAIM**

For live CC $S$, there is a tree of size at least $|S|/\Delta^{2}$ in $G^3$ such that the $G$-dist of each pair of vertices in the tree is at least $3$.

**Pf:**
greedily construct the tree

By a union bound, and the fact that there aren't too many possible trees on $\log n$ vertices, we get that it's unlikely that $G^{3}$ has any $\log n$ sized trees. So the live CCs in $G$ should be of size at most $\Delta^{2} \log n$.

#### degeneracy
Nathan and I came up with a nice algorithm for approximating the degeneracy of a graph.
Unfortunately the actual pset question was much less cool than this (it was about arboricity). 
But anyways here's our ridiculous algorithm.

First, define degeneracy of a graph to be the smallest number $D$ such that it's possible to repeatedly strip vertices of degree $D$ from the graph and by doing so end up with the empty graph.
Note that this is one nice way of quantifying sparsity of a graph.

**Observation 1**
You can compute degeneracy in time $\widetilde{O}(m)$ easily. Just repeatedly strip all the vertices with degree less than $D$. (you can binary search to guess $D$)

**Observation 2**
Now we argue that subsampling edges doesn't mess with degeneracy too much.
The argument is that, if we look at the vertex stripping order from before, then it's probably still a valid ordering. When you have lots of neighbors in a set, Chernoff says that this number shrinks predictably. And if not then we don't even care. 

# cool FG problem 

Someone told me this problem today, and I thought it was very nice. 

> Q: Given a subquadratic algorithm for OV, show how to make a subquadratic algorithm for "all vectors OV" -- for every input vector you need to output a bit that tells you whether that vector is orthogonal to one of the other vectors.

> A: 
> - First, let's find all the vectors $u\in U$ such that there are more than $n^{\varepsilon}$ many $v\in V$ with $u \perp v$.  We can do this in time $n^{1-\varepsilon}\log n$ per $u$.
> - Get rid of these vectors -- we output 1 for them.
> - Now, partition $U,V$ into $n^{3/4}$ many pieces each, randomly which will be of size $n^{1/4}$.
> - For each pair of pieces, check with our magical OV algorithm whether there is any orthogonal-ness that happens between the pairs. This takes time $n^{3/2+2(1/4-\varepsilon)}$ because of our fancy OV algorithm.
> - For each $U$ piece, we find at most $n^{1/4+\varepsilon}$ many $V$ pieces that can be orthogonal to it.
> - Now for each $u\in U$ we look at the $n^{1/2+\varepsilon}$ elements in the union of the $V$ pieces that we need to consider and see whether any of these are orthogonal to it.

Neat!

# distribution testing

### distribution testing part 1 -- L1/L2 norm
**Claim:** 
Suppose we take $T = 100n\varepsilon^{-2}$ samples from a distribution $p$ on $[n]$ and then form a vector $\hat{p}$ where $\hat{p}$ gives our empirical guesses to the Pr of each point.
1. If $p=[1/n,\dots,1/n]$ (uniform) then $||p - \hat{p}||_1 < \varepsilon$ with Pr $\ge 3/4$.
2. If $p$ is $\varepsilon$-far in $L_{1}$-norm from uniform, then $||p-\hat{p}||_1>\varepsilon$ with Pr $\ge 3/4$.

**Proof**
$$
\mathbb{E} ||p-\hat{p}||_1 \le \sum_x \sqrt{\mathbb{E}(p_x-\hat{p}_x)^{2}} = \sum_x \sqrt{\mathsf{Var}(\hat{p}_x)}.
$$
$\hat{p}_x \sim \frac{1}{T}\mathsf{Binom}(T, p_x)$.
So $\mathsf{Var}(\hat{p}_x) = \frac{1}{T}p_x (1-p_x)$.

Hence, 
$$
\mathbb{E} ||p-\hat{p}||_1 \le \frac{1}{\sqrt{ T }}\sum_x \sqrt{ p_x } \le \sqrt{ n/T } \le \varepsilon/10.
$$

Hence Markov says Pr of deviation of more than $\varepsilon$ is at most $1/10$.

Messing with the parameters a bit this should give us a tester between 
- uniform distr
- L1 Far from uniform distr

---

#### L2 distance. 

Here's the strategy: 
 - Let $s = 100\sqrt{ n }\varepsilon ^{-4}$.
- We will take $s$ samples, and let $c_{ij}$ denote whether samples $i,j$ where for the same value.
- We define $C = \frac{1}{\binom{s}{2}}\sum_{i<j}c_{ij}$.
- By linearity of expectation: $\mathbb{E}[C] =||p||_2^{2}.$
- We'll show that $\mathsf{Var}[C]$  is reasonable
	- this will imply that setting $s$ large enough, we can get a good estimate of $||p||_2^{2}$ with good probability. This will suffice to distinguish uniform from L2-far from uniform.

I think the main interesting part is bounding $\mathsf{Var}[C]$.
**Solution:** 
It's convenient to look at the centered versions of variables.

If we let $b_{ij}  = c_{ij}-\mathbb{E} c_{ij}$, then the expression we're interested in is:
$$
\mathbb{E}\left[\left(\sum_{i<j} b_{ij}\right)^{2}\right].
$$
We can break this into 3 parts: 
- If $i,j,k,l$ are all distinct then $\mathbb{E} b_{ij}b_{kl}=0$ 
- You could also imagine $i=k,j=l$.
- But the dominant term is $|\set{i,j,k,l}|=3$. 
	- but it's manageable.


Next, you can use this L2 tester to get an L1 tester. 	
Specifically, multiplicatively approximating L2 norm can give additive approx on L1 norm.

---

To summarize, 
We have this estimate $\hat{c}$ for $||p||_2^{2}$, with variance $O(||p||_2^{3}/s)$ using $s$ samples.

$$
\Pr[|\hat{c}-||p||_2^{2}|\ge \gamma] \le \frac{\mathsf{Var}[\hat{c}]}{\gamma^{2}} = O\left(\frac{||p||_2^{3}}{s\gamma^{2}}\right).
$$

If we want an additive $\varepsilon^{2}$ approximation, e.g., because we're trying to tell whether a distribution is L2-far from uniform, then we only need like $O(\varepsilon^{-4})$ queries. 

If we want a $1+\varepsilon^{2}$ multiplicative approximation then we need more like $\sqrt{ n }\varepsilon ^{-4}$ queries. This can suffice for doing L1-far testing.

## distribution testing part 2 -- closeness testing

Recall from last time -- we had a pretty nice estimator for $||p||_2^{2}$.
In particular we could get an additive approx really fast, and a multiplicative approx somewhat fast.

We used this to distinguish $p=U$ and $||p-U||_1>\varepsilon$.

Now we generalize to the following question:

> Suppose we know $q$, and get samples from $p$. Can we tell apart $q=p$ vs not?

- best query complexity is $\Theta(\sqrt{ n })$.
I think this was on the hw.

Even trickier question:

> You get samples from both $p,q$. Can you tell apart $p=q$ VS $||p-q||_1>\varepsilon$??

- this one is $\Theta(n^{2/3})$. kind of surprising how much harder this is than if $q$ is known.

Trickiest question: (tolerant testing)
> $||p-q||_1<\varepsilon$ VS $||p-q||_1>2\varepsilon$??

- apparently this one the answer is $\Theta(n/\log n)$. wow that's tough!

Anyways, back to the non-tolerant testing setting.

$$
||p-q||_2^{2} = \sum p_i^{2} + \sum q_i^{2} - 2\sum p_iq_i
$$
We already know how to estimate $||p||_2^{2},||q||_2^{2}$.
There's some similar technique that we can use to estimate the last term. 

PROBLEM -- 
- Suppose we take $m$ samples and let $x_i$ be the number of times we got element $i$. 
- $x_i,x_j$ aren't independent! more of one means probably less of others.

Solution -- 
- Don't fix the number of samples. Instead, choose it randomly from Poisson distribution.

Interesting Fact: The following two things are actually the same
- Sampling $\hat{m}\sim Poi(m)$ and then taking $\hat{m}$ samples 
- Sampling $x_i \sim Poi(m\cdot p_i)$ for each $i$ and then taking $x_i$ copies of $i$ and permuting things.


I think this is because $Poi(\lambda_{1})+Poi(\lambda_{2}) = Poi(\lambda_{1}+\lambda_{2})$.

Recall -- Poisson distribution is supposed to model something that happens with some rate over some interval, and the events are independent.


**Reducing to the low L2-norm case**

Recall -- we had some algorithm for estimating $||p||_2^{2}$.
The variance was something like $O(||p||_2^{3}/s)$ where $s$ was number of samples.

PROBLEM: \
This looks bad if $||p||_2^{3}$ is large...

SOLUTION: \
We'll transform $p,q$ into $p',q'$ which have small L2 norms, and such that $p=q \iff p'=q'$and so that $p',q'$ are far if $p,q$ are far.

**Flattening**

Recall: if $||p||_2^{2}$ then our estimator for this quantity had small variance. 
Now we give a procedure to flatten $p$.

Procedure: 
- $S = \mathsf{Poi}(m)$ samples from $p$.
- $b_i =$ number of times $i$ appears in $S$.
- Now make a new domain, with $b_i+1$ copies of $i$ for each $i$.

- New distribution: choose random $i$, then choose random copy of $i$ to ouput.


We're going to use one $S$, sampled from $p$, to transform BOTH $p,q$.
Note that if $p=q$ this works goes pretty well for us. 
On the other hand, if $||p-q||_1>\varepsilon$, then we'll show that this is still the case even after the transformation. 
Yup it's true because we basically just split up some of their probability masses in the same way, so the total L1 distance is the same.

**Claim**: $\mathbb{E}[||p'||_2^{2}] \le 1/m$

**Proof**: It just turns out that this is how the Poisson distribution works.


**Theorem**: given $b\ge \max(||p||_2, ||q||_2)$ samples, can tell apart $p=q$  vs far in $bn/\varepsilon^{2}$ samples.

**Cor** only need $b\ge \min(||p||_2, ||q||_2)$, because if the L2 norms aren't similar then we'll just notice that. 


**Full algorithm**, with running time $n^{2/3}\varepsilon ^{-4/3}$:
- Flatten using $k$ samples.
	- This results in L2 norm being $1/\sqrt{ k }$ whp
- Run tester on flattened guys

Cost:
$$
k + \frac{1}{\sqrt{ k }} n/\varepsilon^{2}
$$
nice.

## distribution testing part 3 -- monotone

- We say a distribution $p$ on $[n]$ is monotone if $p(i)\ge p(i+1)$ for all $i$.
- Want to test: monotone VS L1 far from monotone.
- Goal: $\Theta(\sqrt{ n })$ queries.

**Birge Decomposition**: 
Split $[n]$ into $\varepsilon ^{-1}\log n$ geometric intervals with lengths $\lfloor (1+\varepsilon) \rfloor, \lfloor(1+\varepsilon)^{2} \rfloor, \dots,$

**Flattened distribution**: $\tilde{q}$
- Pr of each guy is the average Pr of an element in that guys bucket 

Birge's theorem:

> If $q$ monotone, then $\tilde{q},q$ are L1 close.


Idea for an algorithm:
1. Let $q^*$ be an estimate of the Birge Flattened distribution $\tilde{q}$ of our distribution $q$
2. If $q^*$ isn't close to monotone then REJECT.
3. Else, check that $q,q^*$ are close. REJECT if not, ACCEPT if are.

Analysis: 

**Claim 1**: If $q$ is monotone, then tester passes whp.

**Proof:**
- $q$ monotone, then $\tilde{q}$ monotone, $\hat{w_j}$'s are close to $q(I_j)$'s, so $q^*\approx \tilde{q}$, so $q^*$ approx monotone.
- Also, $q\approx q^*$ because $q^*\approx \tilde{q}\approx q$.
- So we'll accept.

**Claim 2**: If tester passes whp then $q$ is $\varepsilon$-close to monotone.

**Proof**
$q^*\approx \tilde{q}$ should just always be true by Chernoff. 
So if the tester passes whp it must be the case that $\tilde{q}$ is close to monotone.

ok, but we haven't even talked about how to do tolerant testing of whether $q\approx q^*$...
Oh but we actually already know how to do this!
This is just uniformity testing!
Because the $\tilde{q}$'s are uniform.

ok. neat.


**Proof of Birge's theorem**
0. no err on length 1 intervals
1. If there are any intervals of length at most $1/\varepsilon$ then there are at least $1/\varepsilon$ size 1 intervals.
2. Let $h_i$ denote the max pr on $i$-th interval, and $\ell_i$ denote length of $i$-th interval
3. Then ERROR is at most $\sum (h_{i+1}-h_i)\ell_i \approx \sum h_i (\ell_{i+1}-\ell_i)\approx \varepsilon \sum h_i \ell_i \le \varepsilon$.  because of how we did the dyadic cuts. 
## Property Testing on Dense Graphs

I think there might be some ridiculous theorem that says something like "the properties which can be tested efficiently are exactly the hereditary properties" where I say ridiculous because it invokes Szemeredi Regularity Lemma. But I think even if this is true, it's still an interesting question "how good of a tester can I get?" This is just a preface to motivate why we're going to spend some time on testing bipartiteness even if this is a hereditary property. 
Anyways today we're going to talk about bipartiteness.
### Testing Bipartiteness
$\varepsilon$-far from bipartite means must rm $\varepsilon n^{2}$  edges to make it bipartite.

Plan: 
1. Suppose our graph really is far from bipartite. Then, for any fixed partition, if we sample a couple of edges, it's quite likely that we'll find some edges that violate that partition -- an $\varepsilon$-fraction of edges violate the partition so something on the order of $\varepsilon ^{-\Theta(1)}$ gives us a good shot.
2. However, union bounding over $2^{n}$ many partitions isn't going to fly with the above approach.
3. So we find a much smaller set of vertex partitions, which are "**dense**" in the space of all vertex partitions.


Here's our smaller set of vertex partitions: 

- Choose a random set $U$ of size $100\varepsilon^{-1}\log \varepsilon^{-1}$. 
- For every partition $U_{1},U_{2}$ of $U$ form a partition $Z_{1}\supset U_{1},Z_{2}\supset U_{2}$ of $G$ as follows:
	- If $z$ has neighbors in both $U_{1}$ and $U_{2}$ then output "bad partition"
	- If $z$ has neighbors in at most one of $U_{1},U_{2}$, then stick it on whichever side it doesn't have neighbors to. 

In order for this to be good, I think we'd approximately need the following lemma:

**Lemma that I'd want:** 
If $G$ is bipartite, then it's pretty likely that for some partition $U_{1},U_{2}$ of the set $U$, the induced full partition $Z_{1},Z_{2}$ of the graph has at most $(\varepsilon/2) n^{2}$ violating edges.

Why I want this Lemma: 
We definitely have the following thing:

**Fact:** 
If $G$ is $\varepsilon$-far from bipartite, then for any partition $U_{1},U_{2}$ of the set $U$, the induced full partition $Z_{1},Z_{2}$ of the graph has at least $\varepsilon n^{2}$ violating edges.

So if the Lemma that I wanted were true, then we just need to get a good approx for number of violating edges in each of these partitions. Which I think is pretty doable. 

Here's why I think the lemma is true:
- There are two types of vertices. 
- Dumb vertices have degree at most $.75 \varepsilon n$. tbh we can just pretend these vertices don't exist because we're allowed to cut $\varepsilon n^{2}$ edges. 
- The other type of vertex is the type of vertex where if you sample $100\varepsilon^{-1}\log \varepsilon ^{-1}$ random vertices from the graph, it's quite likely that one of those guys is connected to this vertex. Where quite likely probably means Pr 1-eps/100 here. 
- So, we don't care about dumb vertices, and non-dumb vertices should be placed on the correct side. 

And yup that's what Ronnitt did too. Very nice!

## Triangle-freeness
Today we're going to talk about testing for triangle-freeness.
Will abbreviate szemeredi reg lemma to **SRL** here. 

Recall: vertex sets $A,B$ are called $\varepsilon$-regular if for all $A'\subseteq A,B' \subseteq B$ with $|A'|\ge \varepsilon|A|,|B'|\ge \varepsilon|B|$, 
$|d(A',B')-d(A,B)|\le \varepsilon$ where $d(X,Y) = e(X,Y)/(|X||Y|)$.

**Lemma**:
If all pairs in $A,B,C$ are $\gamma$-regular, and have density at least $2\gamma$ between them, then you get $|A||B||C|\gamma^{3}/2$ triangles.
**Proof**: do some cleaning, it's basically just true.

**SRL**:
Equipartition into $k$ parts, and at most $\varepsilon\binom{k}{2}$ pairs are not $\varepsilon$-regular.


Now the property testing question:
- Distinguish between $G$ triangle free and $G$ must remove $\varepsilon n^{2}$ edges to kill all triangles.

How we do it:

**Triangle Removal Lemma**
$\forall \varepsilon,\exists \delta$ such that $G$ $\varepsilon$-far from triangle free implies $G$ has $\delta\binom{n}{3}$ triangles.

This means that the simple tester of just choosing some random triples and checking if they make a triangle works!

ok, **proof**?

1. We do a regularity equipartition into $5/\varepsilon$ parts.
2. CLEAN  we remove a small number of edges:
	1. Delete edges within parts
	2. Delete edges between irregular parts
	3. Delete edges between low density parts
3. If $G$ was far from $\triangle$-free then must still have a triangle. 
4. But now, one triangle implies cubicaly many triangles!

