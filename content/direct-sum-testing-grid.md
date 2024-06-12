In this note I summarize "Direct Sum Testing: The General Case", by Dinur and Golubev, in the **grid** setting, rather than the $k$-fold setting of [[direct-sum-testing-k-fold]]

## Direct Product Test
> [!bug] Q1: How to analyze the Direct Product Test?

The direct product test works as follows:
- Given $f: [n]^d\to \F_2^d$, we want to check whether $f$ is close to $x\mapsto (g_1(x_1),\ldots, g_d(x_d))$ for some $g_i$'s.
- Sample $a,b$ conditional on $a_i=b_i$ in $3/4$ of the coordinates.
- Let $S$ be the set of coordinates where $a,b$ agree. 
- Check that $f(a)_S = f(b)_S$

> Preliminary thoughts on Q1:
> I can prove something weaker than their statement, namely that if $g$ passes the direct product test with probability $1-\epsilon$ then
> $\Pr[g_i(x)=g_i(y)\mid x_i = y_i]>1-4\epsilon.$
> I can prove this with a very simple coupling argument. But getting the full theorem seems a *bit* tricky.

## Shapka Test
This one is pretty clear. 

>[!bug]
>However, I think their exposition has some rather serious typos in it. 
>Like I'm pretty sure they define the Shapka test incorrectly. 

If 
$$
\Pr_{a,b}[f(b_1,b_2,b_3)=f(a_1,a_2,b_3)+f(a_1,b_2,a_3)+f(b_1,a_2,a_3)]>1-\epsilon,
$$
then, $f$ is close to $f^{a}$ for some $a$, where $f^{a}$ is
defined as 
$$ 
f^{a}(b)  = f(a_1,a_2,b_3)+f(a_1,b_2,a_3)+f(b_1,a_2,a_3). 
$$ 
In particular, $f^{a}$ is a direct sum. 

On the other hand, if $f$ is a direct sum $f=\bigoplus_i g_i$ then
(because we are working modulo $2$),
$$ 
f(a_1,a_2,b_3)+f(a_1,b_2,a_3)+f(b_1,a_2,a_3) = g_1(b_1)+g_2(b_2)+g_3(b_3) = f(b_1,b_2,b_3).
$$ 

## Square in Cube Test
>[!bug]  
>Their analysis of this test is **very wrong**. \
>I will endeavor to give correct analysis below.

Define $S_x(a,b)$ to be the vector obtained by taking $a_i$
whenever $x_i = 0$ and  $b_i$ whenever $x_i = 1$. 

The test direct-sum test is: \
Sample random $a,b,x,y$ and test:
$$ 
f(S_0(a,b)) + f(S_x(a,b))+ f(S_y(a,b))+f(S_{x+y}(a,b))=0.
$$ 
Let 
$$ 
\eps_{ab}=\Pr_{x,y}[f(S_0(a,b)) + f(S_x(a,b))+ f(S_y(a,b))+f(S_{x+y}(a,b))\neq0].
$$
Suppose $f$ passes the test with probability "Square in Cube" test with probability $1-\eps$. Then, $\E_{ab} \eps_{ab}=\eps$.
For each $a,b$, the BLR test implies the existence of $I_{ab}\subseteq [k],c_{ab}\in \set{0,1}$ such that
$$
\Pr_x[f(S_x(a,b))=\chi_{I_{ab}}(x)+c_{ab}]>1-\eps_{ab}.
$$

Let $F_a(b)$ be the indicator vector for $I_{ab}$. Let $c_a(b) = c_{ab}$.

> [!tip] Lemma 1
$F_a(b)$ is close to a direct product (as a function of $b$) $(g_{1}(a,b_1),\dots,g_d(a,b_d))$, and $c_a(b)$ is a constant (as a function of $b$).

Sample $b,b'$ uniformly randomly, conditional on them differing in $1/4$ of their coordinates. 
Let $x$ be $0$ on the coordinates where $b,b'$ differ, and $0$
with chance $1/3$ on the coordinates where $b,b'$ agree. 
Then, $x$ is uniformly randomly distributed. 

By construction, $S_x(a,b) = S_x(a,b')$.
By a union bound, we have that 
$$
\Pr_{b,b',x} [\chi_{F_a(b)}+c_a(b) = \chi_{F_a(b')}+c_a(b')] > 1-2\eps_{ab}.
$$ 
#todo This means that for most $b,b'$ we have that 
$$ \Pr_{x} [\chi_{F_a(b)}(x)+c_a(b) = \chi_{F_a(b')}(x)+c_a(b')] $$ 
is large, whereby we deduce that $F_a(b)\mid_{C_b\cap C_{b'}} = F_a(b')\mid_{C_b\cap C_{b'}}$. 

Now we argue that $c_a(b)=c_a(b')$ for most $b,b'$.
Thus, by the direct product test we conclude that $F_a(b)$ is close to a direct product. 

So we get that for most $a,b$ #todo sus
$$ \Pr_x \left[ f(S_x(a,b)) = c(a)+\sum_i g_i(a,b_i)x_i \right] > 1-100\epsilon.$$ 

Observe: $S_{x}(a,b)=S_{\bar{x}}(b,a)$.
#todo For most $a,b$, we have:
$$\Pr_x \left[ \sum_{i}g_{i}(a,b_{i})x_{i}+c(a)= f(S_x(a,b))=f(S_{\bar{x}}(b,a))=\sum_{i}g_{i}(b,a_{i})\overline{x_{i}} +c(b) \right]>1-200\eps.$$
It seems like this should imply the existence of functions $\psi_i$ such that
For most $a,b$,
$$
\Pr_x\left[ f(S_x(a,b))=c(a)+\sum_i \psi_i(a_i,b_i)x_i\right].
$$

#todo: magically show that $c$ is a direct sum. 
Then, show that we actually get 
$$
\Pr_x\left[ f(S_x(a,b))=\sum_i \phi_i(a_i)\overline{x_i}+\phi_i(b_i)x_i\right].
$$

If we can show that, then we get, 
$$
\Pr_{a}\left[ f(a)=\sum_i \phi_i(a_i) \right]>1-200\eps.
$$
Which would be lovely. 


> [!bug] 
> All of these steps were highly sus. Please fix!


## A highly general agreement test thing

>[!bug] I found this part quite confusing.
>I don't even think I know what the simplicial complex they defined is...

**Theorem**: Suppose $S$ is a collection of subsets that are top faces of a $\lambda$-one-sided $k$-partite $1/k^3$-high dimensional  expander. 
Then given a family of local functions which passes the agreement test with good probability, then, ...something...

Some definitions: 
Let $X$ be the set of edges in a hypergraph.
- $X$ is a **Simplicial Complex** if it is downwards closed. (i.e., $A\subset B, B\in X \implies A\in X$.)
- $X(\ell)$ refers to edges of size $\ell+1$.
- Sometimes hyperedges are also called **faces**. 
- A $d-1$ dimensional simplicial complex has largest hyper-edge size of $d$. 
- The **top face** of a $(d-1)$-dimensional simplicial complex is $X(d-1)$.
- **Link:** the collection of faces that are disjoint from $\sigma$ whose union belongs to $X$.
- Ex: the *link* of a vertex is the set  of its neighbors. The link of an edge is the set of common neighbors of both vertices. 
- Note that the link of a face is a simplicial complex of lower dimension.
- A probability distribution on the top face induces a probability distribution on the vertices by selecting a top face and then passing to two random vertices in it. This gives a weighted graph called the **$1$-skeleton**.
- A $(d-1)$-dimensional simplicial complex is a $\lambda$-one-sided HDX if for every face $\sigma\in X(t)$  $t\le d-3$, the $1$-skeleton of the link $X_\sigma$ is a $\lambda$-one-sided expander graph, meaning the normalized eigenvalues of the random walk on the skeleton are at most $\lambda$. 

Then they compute the eigenvalues of some matrix to show that something is an HDX.
## open questions
- Can you analyze this test with Fourier analysis?
- ~~Can reconstruct the function using the Shapka scheme?~~
- Does this other test also works for direct-sum testing?
- (Alek): higher order dependencies (i.e., not just a direct-sum, but maybe a sum of things that are allowed to depend on e.g., two terms at a time)
