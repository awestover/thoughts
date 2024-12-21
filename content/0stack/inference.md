I'm tentatively planning to grind through MIT's "inference" courses. 
$$ \newcommand{\dep}{\perp\!\!\!\perp}  $$
$$ \newcommand{\E}{\mathbb{E}}  $$

Specifically, this is 
- algorithms for inference
- inference and information

These cover some things like 
- graphical models
- information theory
- inference 
- information geometry
- thinking about structure of random variables and independence and stuff
-  probably other stuff too

These topics seem 
1) pretty intellectually interesting
2) the most relevant stuff to ARC's research agenda (where I'll be interning over the summer)

I reserve the right to quit if I find some cool research projects / make up some cool research questions myself. 
However I don't anticipate quitting currently. 

I'll record my journey of doing this here. 
I'll also tentatively plan to make this into a dialogue because I love JJ+shatar+blobby, but right now I just need to write some stuff down.

**Day 0 -- Dec 12:** 
Likelihood ratio test: 
If you have two hypothesis, and want to select one to minimize the probability that you select the wrong one, then you should compute Pr(hypothesis X is true given the data) and choose whichever hypothesis maximizes this chance. 
This assumes that you have some priors on the chance of the hypothesis being correct, and that under each hypothesis you can compute Pr(data).

**Day 1 -- Dec 13** 
Read the first couple Alg for Inf notes. 
First some definitions, then a neat theorem.

**Directed Graphical Model**
![[Pasted image 20241214082103.png]]
In a directed graphical model, we can factor the probability distribution into terms of the form 
$p(x_i\mid x_{\text{parents of xi in the graph}})$. So any distribution obeys the conditional independence implied by the complete graph (which is not so impressive since that's the empty set).

But my digraph implies more independences. For instance, $x_1 \dep x_3 \mid x_{2}$ based on my picture.

The general hope for this class is that if we have a sparse graph then we can do much much more efficient things than are possible in the dense case. 

A general algorithm for listing off some conditional independences is:
- topo-sort the DAG
- $x_i$ is independent of non-parent things before it in the topo-order, conditional on $x_i$'s parents

**Bayes Ball Algorithm**
There's this pretty handy trick for checking whether a conditional independence relation is implied by a directed graphpical model -- it's called the bouncy ball method.

The method answers the question is $A\perp B \mid C$ where $A,B,C$ can all be sets of variables.
1. Shade the variables $C$.
2. Put a ball at all vertices in $A$.
3. Bounce the balls. 
4. If a ball ever hits $B$, then the conditional independence relation is false. Otherwise it is true.

The bouncing is unfortunately a little bit counter-intuitive -- it's not just "when you hit a shaded circle, bounce backwards".
Instead there are three cases:
1. Bounce if shaded on chain. else pass. 
2. Bounce if shaded up tree. else pass
3. Pass if shaded in vee. else bounce.
![[Pasted image 20241217141325.png]]
The only way I can think of to remember this is to keep these three examples in mind.

**Undirected graphical model**

Vertices for vars. 
> [!tip] Cond. indep
$A\perp B \mid C$ if when you cut  $C$, can't reach $A$ from $B$.

This is great and intuitive. 

**Theorem** (Ham-Clifford)
(1) If $p$ is a pr distr where every state has nonzero pr, and $p$ is described by undirected graphical model $G$, then $p$ can be expressed as
$$
\prod_{C\in \mathcal{C}} f(x_C)
$$
where $\mathcal{C}$ is the set of (maximal iyw) cliques in $G$.

(2) If $p$ is a pr dist that factors into terms that only look at cliques in $G$, then $p$ satisfies the conditional independences specified by cuts ($A\perp B \mid C$ if cutting $C$ separates $A,B$). 

**Proof**
(2) is a straightforward computation.
(1) is quite tricky to prove -- I'll sketch pf below.

We just prove for binary random vars. Probably could generalize. 
If rvs are binary, say $x_1^{n}\in \set{0,1}^{n}$, we can identify $x$ with $S \subseteq[n]$.
We'll conflate $p(x)$ and $p(S)$ where $S$ is the set of coordinates in $x$ with value $1$.
Now, define
$$
Q(S) = \sum_{A \subseteq S} (-1)^{|S \setminus A|}\log p(A).
$$
It turns out this function has some nice properties. 
For instance, 
$$
\sum_{B\subseteq S} Q(B) = \sum_{A\subseteq S}\log p(A)\sum_{A\subseteq B\subseteq S}(-1)^{|B\setminus A|} = \log p(S). (*)
$$
Now we're going to show that $Q(S)=0$ if $S$ is not a clique. This will conclude the proof by (\*).

To understand why, let's just consider a graph on vertices $1,2,3$ with connnections $(1,2),(2,3)$.
In this graph we have $1\perp 3\mid 2$. Say we're trying to compute $Q(123)$.
$$
Q(123) = \log \frac{p(123)p(1)p(2)p(3)}{p(12)p(23)p(13)p(\varnothing)}.
$$
By the magic of conditional indepdence we have:
$$
p(123) = p(x_3)p(x_2\mid x_3)p(x_1\mid x_2)
$$
$$
p(12) = p(\neg x_{3})p(x_{2}\mid \neg x_{3})p(x_{1}\mid x_{2}).
$$
Hence
$$
\frac{p(123)}{p(12)} = \frac{p(x_{3}\mid x_{2})}{p(\neg x_{3}\mid x_{2})}.(\spadesuit)
$$
AND
$$
p(2) = p(\neg x_{3}) p(x_{2}\mid \neg x_{3}) p(\neg x_{1}\mid x_{2}).
$$
$$
p(23) = p(x_{3})p(x_{2}\mid x_{3}) p(\neg x_{1}\mid x_{2}).
$$
So, 
$$
\frac{p(2)}{p(23)}=\frac{p(\neg x_{3}\mid x_{2})}{p(x_{3}\mid x_{2})} (\heartsuit)
$$
So  $(\heartsuit) \cdot (\spadesuit) = 1$
You can do the same thing for the terms without the $2$.
Basically we're just using spatial markov property. 
Anyways, I thought this was neat. 

**Factor Graph**
- bipartite
- one side corresponds to vars, the other side is ways to combine the vars.
If the LHS has the vars $x_{1},\dots,x_n$, and the RHS has vertices $v_{1},\dots,v_k$ then the pr dist must factor as
$$
\prod_{i=1}^{k} f(x_{N(v_i)})
$$
where $N$ is neighbors.

> remark: sometimes the models can't express all relevant properties of your prdist

 > e.g., directed models seem to be super lossy about preserving conditional independence relations!

> Q: can you convert btwn models? A: sometimes.

**Day 2 -- Dec 17**
Wrote up day 1. 

Did some of the psets. Note to self -- only do the interesting problems: my goal is to be exposed to the subject and understand the basic ideas. If something seems like a technical not interesting or intuitively useful idea, ignore it. 

some hw qs
> Q1. Undirected graphical model for sampling an independent set according to $p(I)\propto \exp(|I|)$?
> A: the graph is the graphical model. 

> Q2. Undirected graphical model for sampling a matching according to $p(M)\propto \exp(wt(M))$?
> A: orig graph is $G=(V,E)$. Make new graph on the edges $G'=(E,F)$  where $F = \set{(e,e')\mid e,e'\text{ are adjacent in G}}$



**Day 3 -- Dec 18**
Talked more about converting between directed and undirected graphical models. 
Talked about conditions for perfect conversion. 
Talked about "minimal I maps"  i.e., converting to a different graphical  model which doens't imply any false structure but would if you removed any edges. 

main takeaways: 
- digraph to undigraph conversion is perfect if moralization (connecting parents) doesn't add edges. 
- undigraph has a perfect digraph model iff is "chordal" i.e., all cycles of len > 3 have a chord. 

**Lec 6 -- Gaussian Graphical Models**
Gaussian: 
- $Au+b$ for $u$ iid Gaussians
- $a^{\top}x$ is Gaussian for all $a$
- PDF = $\frac{1}{Z}\exp(-\frac{1}{2}(x-\mu)^{\top}\Lambda ^{-1}(x-\mu))$ covariance rep
- could also write PDF as $\frac{1}{Z'}\exp(-.5 x^{T}Jx+h^{T}x)$ information rep
 
Interesting fact:
$\E[x_{1}\mid x_{2}]$ is linear in $x_{2}$ for Gaussians.

Talked about how to marginalize and condition wrt covariance rep and information rep.

$$
x_i \perp x_j \iff \Lambda_{ij}=0.
$$
$$
x_i \perp x_j \mid x_{rest}\iff J_{ij}=0.
$$info matrix gives easy way to make graphical models. 
- undirected model: add edge whenever $J_{ij}\neq 0$
- directed model: idk

Gauss-Markov Process
$$
x_{i+1} = Ax_i + Bv_i.
$$
where $x_1,v_i$ are iid Gaussians.

There's this thing called the Schur complement. Seems important.
- block matrix inversion 
- computing marginals / conditioning

**Lec 7** Now that we've defined graphical models we're finally going to start actually doing some inference. 

Recall -- we care about computing posterior beliefs and MAP estimates. 
Today we think about these problems for undirected graphical models.

Naive alg for marginalization: 
$$
p_{x_{1}}(x_{1}) = \sum_{x_{2},\dots,x_n} p(x).
$$
Time: $|\mathcal{X}|^{n-1}$.
But if we have a nice factorization then we can break up the sum and it'll have some repeated parts. 

Elimination Alg -- will give an exact solution! but may be somewhat computationally expensive. 
![[Pasted image 20241218153216.png]]
oh this is treewidth -- interesting.

**Plan going forward**
I think lectures 8-15 might be good, but mostly look like simple algos like the one above. 
I think it'd be semi-helpful to know about them, but expect to gain more utility from these other ones

So I'm planning to skip ahead for now to lectures 16,17,18,19 about loopy BP and VI some fancy markov chain stuff, lec 20-24 (end) also seems interesting

could also just try some psets