In this note I summarize Alon et al's paper on testing low degree polynomials.

### intro

>[!theorem]
>There is a test with $O(1/\epsilon + k4^k)$ queries, that 
>(1) given a degree-$k$ polynomial over $\mathbb{F}_2$ always accepts, 
>(2) given a polynomial which is $\epsilon$-far from a polynomial rejects with probability at least $2/3$.
>
>The exponential dependence on $k$ is unavoidable (in contrast to the situation for larger fields).

>[!info] Remark:
> Over large fields, you an reduce multivariate degree-$k$ testing to univariate degree-$k$ testing. 
> The reduction works by restricting the function $f$ to several random lines, and then doing univariate degree-$k$ testing. 

> [!bug] Q: 
> Univariate degree-$k$ testing can be accomplished by just sampling $k+2$ points and trying to use the value at $k+1$ of them to guess the value at the final point?

Description of their test: 
Repeat the following, many times:
1. Select $y_1,\ldots, y_{k+1}$. 
2. Evaluate $f$ on all non-empty sums of subsets of the $y_i$'s, and then add up all of these evaluations. 
3. Reject if the sum is non-zero.

**def**: self-corrector: \
If $f$ is close to a degree-$k$ polynomial, then we can recover the value at an arbitrary point $x$ with good probability via a few random queries.


**Reed-Muller code**: the set of evaluations of degree at most $k$ polynomials.\
**Minimum distance of a code**: how close are the closest distinct codewords?\
Reed-Muller has minimum distance $2^{n-k}$: two non-equivalent degree-$k$ polynomials can agree on at most $k$ points. 

It turns out that the following test can help check whether a string in  $\{0,1\}^{2^n}$ is a Reed-Muller code or not: \
Check if it's orthogonal to a random dual code. 

>[!bug] Q
>Is this what Alon et al are doing?

---

### Prelims:
$\mathcal{P}_k$: degree-$k$ polynomials with no free term. \
$\Delta(f,g)$ is the set of coordinates on which $f,g$ disagree. \
**one-sided tester**: always accepts if in the class. 

**Def:** 
$$T_f(y_1,\ldots, y_\ell) = \sum_{\varnothing\neq S\subseteq [\ell]} f\left(\sum_{i\in S} y_i\right).$$

$$T_f^{y_1}(y_2,\ldots, y_\ell) = f(y_1)+T_f(y_1,\ldots, y_\ell).$$

### characterization of low degree polynomials

**Claim**: $f\in \mathcal{P}_k$ iff $T_f(y_1,\ldots, y_{k+1})=0 \quad \forall y_i$.

> [!tip] Proof
> 
> First, we do a sanity check on $f=x_1x_2$.
> The sum is 
> $$T_f(x,y,z) = x_1x_2 + y_1y_2 + z_1z_2 + (x_1+y_1)(x_2+y_2)+\cdots + (x_1+y_1+z_1)(x_2+y_2+z_2).$$
> There are four $x_1 x_2$ terms, and two $x_1 y_2$  terms, and so on. 
> It's pretty easy to check that everything cancels like it's supposed to. 
> 
> To actually prove this, we can do the following: 
> WLOG let $f$ be a monomial. Fix some $y_1,\ldots, y_{k+1}$. 
> Let $B$ be the set of $S\subseteq [k+1]$ such that $f(\sum_{i\in S}y_j)=1$. The cardinality of $B$ is even. Thus, when you add up over all $S$ you will have cancellation.
>
Now we want to establish the other direction: If $T_f\equiv 0$ then $f$ is a degree $k$ polynomial.
First, setting each $y_i$ to be the zero-vector, we observe that $f(0)=0$.
Assume for contradiction that $f$ is higher degree. 
Without loss of generality, let $I=[s]$ with $s\ge k+1$  be a minimum cardinality set such that $f$ has a non-zero coefficient on the monomial $x_I$. 
Set $y_i = e_i$ for $i\in [k]$ and set $y_{k+1}=\sum_{j=k+1}^s  e_j$.
We claim that  
(1) $a_I \prod_{i\in I}x_i$ vanishes on everything except $\sum_{i\in I} y_i$ 
(2) all other monomials $m'$ have $T_{m'}(y_1,\ldots, y_{k+1})=0$ for all $y$.
> 
To see (1), observe that there is a zero in the product if we do $\sum_{i\in S}y_i, S\neq I$ , but if $S=I$ then we are good. 
To see (2), observe that if $m'$ is degree $k$ then we are done by our proof that $T_m\equiv 0$ for degree $k$ guys, and that if $m'$ is degree larger than $k$ but not just $x_I$, then $m'(y)=0$ for all $y$'s because there is always a zero in the product. But then we get $T_f(y)=1$, a contradiction. So our assumption that our polynomial had degree larger than $k$ must have been false. 

### Analysis of the test

It is now clear that test always accepts degree-$k$ functions.
Now, we must show that it rejects functions which are $\epsilon$-far from degree-$k$ with probability at least $2/3$. 

Define $g$ as a sort of "majority function".
Namely,
$$g(y) = \text{the most likely value of }T_f^y(y_2,\ldots, y_{k+1}) \text{ on random }y_2,\ldots, y_{k+1.}$$
Define $\eta = \Pr_{y_1,\ldots, y_{k+1}} T_f(y_1,\ldots, y_{k+1}) \neq 0$.

**Lemma 2:** $$\mathsf{dist}(f,g)\le 2\eta.$$
**proof:** 
If $\eta$ is small, then $g(y)$ is basically just $f(y)$.

**Claim 4:**
Technical claim.
![[Pasted image 20240610094604.png]]

**proof:** modulo 2 stuff cancels.

**Lemma 3:**
For every $y$, 
$\Pr_{y_2,\ldots, } [g(y)=T_f^y(y_2, \ldots)]\ge 1-2k\eta.$

**proof:** 
Fix $y$. Let $\gamma = \Pr_{y_2,\ldots, } [g(y)=T_f^y(y_2, \ldots)]$; our goal is to show that $\gamma$ is very close to $1$. 
Let $\delta = \Pr_{y_2,\ldots,y_{k+1},z_2,\ldots, z_{k+1}}[T_f^y(y_2,\ldots)=T_f^y(z_2,\ldots)] = \gamma^2+(1-\gamma)^2.$
Now, we do a "hybrid argument". That is, we interpolate between $y_2,\ldots$ and $z_2,\ldots$. 
For each of the $k$ terms we apply Claim 4, leveraging the symmetry of $T_f$.
Their paper typos this part, but the idea is pretty clear. Once you apply claim 4 you just get that the input is uniformly random. Then you union bound to assert that Pr it is ever non-zero is at most $2k\eta$. So we get $\delta \ge 1-2k\eta$, which implies $\gamma \ge 1-2k\eta$. 

**Lemma 5:**
If $\eta<\frac{1}{100k2^k}$, then $g\in \mathcal{P}_k$.

**proof**: 
You do a union bound over ~$2^k$ things, each of which fail with probability at most $2k\eta$. 
You thereby exhibit (by the probabilistic method) the existence of a certain set of $z_{i,j}$'s, which demonstrates that $g$ has the desired property ($T_g=0$). 

**Lemma 6**: 
If $\eta$ is small, then $\eta$ is large. 

**Proof**:
Let $X_S$ be a random variable indicating whether $f(\sum_{i\in S} y_i) = g(\sum_{i\in S} y_i)$. 
The $X_S$'s are pairwise independent. 
$\mathbb{E}\sum X_S = (2^{k+1}-1) d \ge \mathsf{Var}[\sum X_S]$.
Thus, by standard probability tools there is a good chance that $\sum X_S = 1$, as claimed. 

**Theorem 1**: 
The test works. 

**proof:** Either $\eta$ is large, or it is small, in which case it is kind of large. 
So we can get away with $1000/\eta$ tries of the algorithm.


---
### Self-correcting and lower bound

> [!bug] Q
> They pull out some fancy coding theory here. Not sure what's going on. 
