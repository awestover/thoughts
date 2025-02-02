# Intro

We'll use $G(n,1/2,k)$ to denote $G(n,1/2)$ plus a $k$ clique. 

**Recovery problem**:
Given $G\sim G(n,1/2,k)$, return $k$ clique with $1/2$ probability.

**Conjecture**
There are actually lots of versions of the conjecture. 
This paper will try to unify them.

One such conjecture:
> **V1**: No poly time algorithm for PC recovery if $k=n^{1/2-\alpha}$.

**remark** -- easy quasipoly time algorithm.

**Evidence for PC conjecture:** 
It's hard in some restricted models of computation.

**Why it's nice**
can prove other problems hard based on it

**decision variant:** 
> **V2:** can't get good advantage distinguishing between $G(n,1/2)$ and $G(n,1/2,k)$

some variants: 
- allow any $k\ge k_{0}$  -- they call this the "adversarial $k$" model
- let $k\sim \mathsf{Bin}(n,k_{0}/n)$.
- just have a fixed $k$

What these guys did:
- recovery-to-decision reduction

> **V3 exponentially weak decision conjecture:**\
> poly time alg can't distinguish between xxx with advantage better than $1-\exp(-n^{.01})$.

(note that this is quite a weak conjecture!)

> **V4 exponentially weak search conjecture:**\
> there is no poly time alg with success pr better than $1-\exp(-n^{.01})$ for the recovery problem.

**CLAIM**:
Suppose that for all $n,k$  we have have a magical randomized poly time decision algorithm that gets a graph and a value $k_{0}>n^{.01}$ and does the following: 
-  If the graph is $G\sim G(n,1/2,k)$ for some $k\ge k_{0}$ the alg should return TRUE with really good pr $1-\varepsilon$, $\varepsilon$ expo small
- If the graph is $G\sim G(n,1/2)$ then alg should return FALSE with really good pr $1-\varepsilon$.

Then, we could get a randomized poly time recovery algorithm.

**Proof**
Now the "delete a vertex + its neighborhood" algorithm actually behaves well.


> **V5** kologomorov complexity!\
> For any poly time algo $A$, exists some graph $G$ of high kolmogorov complexity such that $A$ is unlikely to find a $k$-clique in $G$.


**NP-hardness of max-clique:**\
> For any poly time $A$, exist graph with $n^{1-\varepsilon}$-clique such that $A$ is unlikely to even output an $n^{\varepsilon}$-clique.

(this is true assuming $\mathsf{NP}\not\subseteq BPP$.)

PC conjecture stronger bc inputs assumed incompressible.


> **V6** strong recovery\
> Pr poly time algorithm finds a kclique is negligible

interesting "detection recovery gap":
- we can detect with non-negligible advantage
	- by counting edges
- but can't recover at all!

> **V7** strong decision binomial \
> you can't get advantage better than $k^{2}/n \cdot n^{.01}$

> **V8** strong decision adversarial\
> immediate consequence of V7


>**V9** strong decision fixed $k$\
> this result has a bit of a gap but it's basically says that you can't get good advantage for most $k$

> **V10** \
> distinguishing between $k,k-1$ can't be done with good advantage!


> **V11** Partial recovery\
> You can't even partially recover.


They say something about refutation algorithms. 
Skipping

**One way functions**

- weak OWFs -- inversion pr at most $1-1/n$
- strong OWFs -- negligible inversion pr

Yao theorem: 
weak OWF --> strong OWF

unfortunately it blows up security parameter

You can make a OWF based on PC conj:

$f$ takes in a graph $G$ and an $k$ vertex subset of vertices, and outputs $G$ plus this clique.
Given the output adjacency matrix, it's hard to recover the input thing. 

> **Corollary**
> If PC OWF is a weak OWF it's also a strong OWF.
> and the security parameter doesn't blow up.


**remark**:
$G(n,p)$ with $p\approx 1$ --> planted clique conjectured to be exponentially hard!
summary:

Core results
![[Screenshot from 2025-02-02 14-11-41.png]]

aux results

![[Pasted image 20250202141200.png]]

# technical overview

Two key ideas:

- shrinking reduction -- do something on an induced subgraph
- embedding reduction -- plant our guy in a larger graph

we will use a concentration inequality on pr that a random induced subgraph satisfies some graph property for all graph properties :O


