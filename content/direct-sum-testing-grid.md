$\newcommand{\F}{\mathbb{F}}$
$\newcommand{\E}{\mathbb{E}}$
$\newcommand{\eps}{\varepsilon}$
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
Let $\eps_a = \E_b [\eps_{ab}]$.

Suppose $f$ passes the test with probability "Square in Cube" test with probability $1-\eps$. Then, $\E_{ab} [\eps_{ab}]=\E_a [\eps_a] = \eps$.
For each $a,b$, the BLR test implies the existence of $I_{ab}\subseteq [k],k_{ab}\in \set{0,1}$ such that
$$
\Pr_x[f(S_x(a,b))=\chi_{I_{ab}}(x)+k_{ab}]>1-\eps_{ab}.
$$

Let $F_a(b)$ be the indicator vector for $I_{ab}$. Let $k_a(b) = k_{ab}$.

> [!tip] Lemma 1
> $F_a(b)$ is close to a direct product (as a function of $b$) $(g_{1}(a,b_1),\dots,g_d(a,b_d))$, and $k_a(b) = k(a)$ (i.e., is constant as a function of $b$).

Define distribution $b'\sim D_{ab}$ as follows: \
Sample $b'$ uniformly randomly, conditional on having $b_i'= b_i$  hold for exactly $3d/4$ of $i\in [d]$. 

Sample $x\sim X_{bb'}$ as follows:\
- Let $x$ be $0$ on the coordinates where $b,b'$ differ
- and $0$ with chance $1/3$ on the coordinates where $b,b'$ agree. 
Then, $x$ is marginally uniformly randomly distributed. 

By construction, $S_x(a,b) = S_x(a,b')$.
By a union bound, we have that 
$$
\Pr_{b,b'\sim D_{ab},x\sim X_{bb'}} [\chi_{F_a(b)}+k_a(b) = \chi_{F_a(b')}+k_a(b')] > 1-2\eps_{a}.
$$
Let 
$$
\delta_{abb'} =  \Pr_{x\sim X_{bb'}} [\chi_{F_a(b)}+k_a(b) \neq \chi_{F_a(b')}+k_a(b')].
$$
Then intuitively  --- and they have a lemma somewhere that formalizes this --- we have the following: 
> If $\delta_{abb'}<1/3$ then $F_a(b)\mid_{C_b\cap C_b'}= F_a(b')\mid_{C_b\cap C_b'}$ and $k_a(b)=k_a(b')$.

By Markov's Inequality we have that 
$$
\Pr_{b,b'\sim D_{ab}}[\delta_{abb'}>1/3] \le 3\E_{b,b'}[\delta_{abb'}] < 6\eps_a.
$$
From this we conclude
$$
\Pr_{b,b'\sim D_{ab}} [F_a(b)\mid_{C_b\cap C_b'}= F_a(b')\mid_{C_b\cap C_b'}] > 1-6\eps_a,
$$
and 
$$
\Pr_{b,b'\sim D_{ab}} [k_a(b)=k_a(b')] > 1-6\eps_a.
$$
By the direct product test we conclude that $F_a(b)$ is $6\eps_a$-close to a direct product $(g_1(a,b_1),\dots,g_d(a,b_d))$. \
By a simple coupling argument ( #todo: spell out details) we deduce that $k_a(b)$ is $24\eps_a$-close to being a constant (when viewed as a function of $b$) $k(a)$. 
Combining these facts we have that 
$$ \Pr_{x,b} \left[ f(S_x(a,b)) = k(a)+\sum_i g_i(a,b_i)x_i \right] > 1-31\eps_a.$$ 

Observe that $S_{x}(a,b)=S_{\bar{x}}(b,a)$. Thus,
$$\Pr_{a,b,x} \left[ \sum_{i}g_{i}(a,b_{i})x_{i}+k(a)= f(S_x(a,b))=f(S_{\bar{x}}(b,a))=\sum_{i}g_{i}(b,a_{i})\overline{x_{i}} +k(b) \right]>1-62\eps.$$
Define 
$$
\lambda_{ab}=1-\Pr_x\left[ \sum_i (g_i(a,b_i)+g_i(b,a_i))x_i=\sum_i g_i(b,a_i) + k(a)+k(b) \right].
$$
Clearly, if $\lambda_{ab}<1/2$  then we must have 
$$
g_i(a,b_i)=g_i(b,a_i)
$$
for all $i$, and also 
$$
\sum_i g_i(b,a_i)=k(a)+k(b).
$$
By Markov's inequality:
$$
\Pr_{ab}[\lambda_{ab}>1/2]< 128\eps.
$$
Thus, ( #todo justify this; it is intuitive but not obvious; we are passing from random function to deterministic function) there exists $\psi_i$ such that 
$$
\Pr_{a,b}[g_i(a,b_i)=\psi_i(a_i,b_i) \;\forall i]>1-128\eps.
$$
Therefore, by a union bound we have:
$$
\Pr_{a,b,x} \left[ f(S_x(a,b))=k(a)+\sum_i \psi_i(a_i, b_i)x_i \right] > 1-256\eps.
$$

#todo: Now we magically show that $k$ is a direct sum probably using the $\sum \psi_i(a_i,b_i) = k(a)+k(b)$ thing. 
Then, #todo also invoking magic we show that we actually get 
$$
\Pr_{a,b,x}\left[ f(S_x(a,b))=\sum_i \phi_i(a_i)\overline{x_i}+\phi_i(b_i)x_i\right]> 1-O(\eps).
$$

If we can show that, then we get, ( #todo would we actually?) 
$$
\Pr_{a}\left[ f(a)=\sum_i \phi_i(a_i) \right]>1-O(\eps).
$$
Which would be lovely. 


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
