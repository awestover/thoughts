$\newcommand{\eps}{\varepsilon}$
In this note I summarize a paper by Dinur et al on direct sum testing, in a different sense than in [[direct-sum-testing-grid]].

# introduction 
We say that $f: \binom{[n]}{k}\to \mathbb{F}_2$ is a **$k$-fold direct sum** if 
there exists $a\in \mathbb{F}_2^n$ such that 
$$f(x) =\sum_{i\in S} a_i.$$

> [!tip] Result: 
> three query test: 
> accept if is kfold-dir-sum, 
> if  $\epsilon$-far then reject with probability at least $\epsilon/1000$. 
> 
> Specifically, there test is as follows:
> Sample $x,y\in L_k^n$  randomly conditioned on $x+y\in L_k^n$. Then check whether
> $f(x)+f(y)=f(x+y)$.

Equivalent formulations:
- linearity testing on the $k$-slice of the hypercube. 
- "essentially" tensor power testing
- they also have a result about checking if a $\pm 1$ matrix has rank $1$. 

Tensor power: 
$$f(z) = \prod_{i} b(z_i).$$
Tensor product:
$f(z) = \prod_{i=1}^k b_i(z_i).$

> [!bug] 
> In this paper they only are able to do tensor product testing in some restricted setting, namely $k=2$. 
> Is tensor product testing exactly the problem that [[direct-sum-testing-grid]] resolves?

---

# technical overview

> [!info] Reducing Theorem 1.1 for $k<n/2$ to the case $k=n/2$.
> Key Observation:  The test $T_k^n$ actually performs the test $T_k^{2k}$ on a random size $2k$ set $u\subset [n]$.
> 
> By the $k=n/2$ case, we know that for each $u$ where the test succeeds with good probability we obtain a linear function on some restricted local domain. 
> Then, we will show that these restricted local linear functions can be stitched together to obtain a global linear function. Specifically we will do this using direct product testing stuff. 


> [!tip] new analysis of BLR
> Let $L$ be a linear function of minimum distance from $f$. 
> Let $B_L$ be the $x$'s where $L,f$ disagree, and $G_L$ be the $x$'s where $L,f$ agree. 
> Let $\delta  = B_L/2^n$.
> Then,
> $$\epsilon = \Pr[f(x)+f(y)\neq f(x+y)] = \Pr[x,y,x+y\in B_L] + 3\Pr[x\in B_L, y,x+y\in G_L].$$
> crucial to remember that $x,y,x+y$ are not independent :)
> 
> So anyways, we can establish a dichotomy: either $\delta< O(\epsilon)$ or $|\delta-1/2| < O(\epsilon)$. 
> And then we can rule out this second possibility. 
> 
> And the idea is that somehow this analysis is going to still work even when we have the cube slice rather than all of the hypercube. 

Once we have this dichotomy, we will argue inductively to rule out the undesirable case. 
We fix the last coordinates of the functions. 
Then we add these functions. This function passes linearity test. 
Then we go back to original place and find that $f$ agrees with something on $3/4$ of input, contradiction.

Some how we replace the $x,y$ independence clause with the clause "Johnson / Kneser graph has nice expansion".

# yet-another-analysis of BLR
To prove their main result they modify the proof of a new analysis of the BLR linearity test.

**Lemma 1:**\
Suppose $f$ passes the BLR test with probability $1-\eps$. Then the closest linear function to $f$ is *either* at distance $O(\eps)$ or at distance $1/2\pm O(\eps)$.
**Proof:**
Write down some quadratic. It has two roots: one near $0$ and one near $1/2$.

Next, we will rule out the case of $f$ being far from all linear functions via a neat inductive argument. Specifically, we will prove the following two claims, and then chain them together inductively to get the result:

**Lemma 2:**\
1 function n-1 --> 3 functions n-1

**Lemma 3:**\
3 functions n-1 --> 1 function n

Note: it's kind of impressive that the errors don't blow up on them. 
Like instead of going from $\eps$ to $2\eps$ and so on, they always boost back up to the same value. This is because of their earlier dichotomy theorem.

**Proof of Lemma 2:** 
Given $f_{1},f_{2},f_{3}$ satisfying $\Pr_{x,y}[f_{1}(x)+f_{2}(y)=f_{3}(x+y)]>1-\eps$ , 
Let $g=f_{1}+f_{2}+f_{3}$.
By union bound $g$ is $3\eps$ close to passing BLR. Thus, $g$ is (by inductive hypothesis) close to some linear function $\phi$. 
Then, you can show 
$$
\Pr_{x,y} [f_{1}(x)+f_{2}(y)= \phi(x)+\phi(y)] > 1-\eps,
$$ and this implies that $f_{1}+\phi$ is basically constant. And then you can show that at least one of $f_{1},f_{2},f_{3}$  must be linear. 

**Proof of Lemma 3**
Define $f_{1},f_{2},f_{3}$ by randomly the final coordinate. Then one of them is close to a linear function $\psi$.  So you can extend it to a linear function on $n$ bits that agrees with $f$ at least $3/4$ of the time.
But, we earlier showed a near-far dichotomy. So you can't just be distance $3/4$. If you're this close, you're actually all the way close. 

---

# Theorem in the case of $\eps=0$

They first prove the theorem in the case $\eps=0$. 
It's fairly involved. But you can kind of see where you will need to plug in expansion properties of a graph. 

**Lemma**: 
Let $G_i$ be the graph on vertices $\{x\in L_{k}^n \mid x_i=1,x_{i+1}=0\}$, and edges between $x,y$ iff $x+y\in L_k^n$.
$G_i$ is connected. 
**proof**: in fact it has diameter $4$. There's a pretty simple way to construct paths between any two pairs of vertices by just looking at their overlap.

And, the connectivity of $G_i$ basically lets you propagate some equalities and it's very nice. 
## open questions

- **Extending to low degree polynomials**
- reducing direct product testing to direct sum testing
- tensor product
- **low acceptance probability regime (e.g., if you pass test with slightly non-trivial probability, what can we say?) Note: 50% is not the threshold**
	- wait does this already solve it? https://arxiv.org/abs/2402.05217
- derandomized direct sum testing


PS: I'm officially very interested in the two bolded open questions. Just because the techniques used in this paper seem pretty neat. 
I'd *really* like to understand how the expansion stuff works and be able to use it in my own work!
