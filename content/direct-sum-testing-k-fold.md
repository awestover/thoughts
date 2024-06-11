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

>[!bug] Q
> What should we know about Haddamard code? 
>Reed-Muller code?
>[book about coding theory](https://www.sciencedirect.com/bookseries/north-holland-mathematical-library/vol/16)

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



## open questions

- Extending to low degree polynomials
- reducing direct product testing to direct sum testing
- tensor product
- low acceptance probability regime (e.g., if you pass test with slightly non-trivial probability, what can we say?)
- derandomized direct sum testing
- one of them has been resolved https://arxiv.org/abs/2402.05217

