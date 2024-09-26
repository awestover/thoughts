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
![[Pasted image 20240904120659.png]]

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

#todo: add details
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


**sparsification lemma**
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
