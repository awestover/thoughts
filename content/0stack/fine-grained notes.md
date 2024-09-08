Here are some notes on fine grained complexity theory, from Ryan + Virginia's class. 

# lecture 2
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

