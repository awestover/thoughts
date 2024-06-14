$\newcommand{\F}{\mathbb{F}}$
$\newcommand{\E}{\mathbb{E}}$
$\newcommand{\eps}{\varepsilon}$
In this note I summarize "Direct Sum Testing: The General Case", by Dinur and Golubev, in the **grid** setting, rather than the $k$-fold setting of [[direct-sum-testing-k-fold]]

## Direct Product Test
> [!bug] Q1: 
> Is their an elementary (i.e., no fancy HDX tools allowed) analysis of their Direct Product Test?

The direct product test works as follows:
- Given $f: [n]^d\to \F_2^d$, we want to check whether $f$ is close to $x\mapsto (g_1(x_1),\ldots, g_d(x_d))$ for some $g_i$'s.
- Sample $a,b$ conditional on $a_i=b_i$ in $3/4$ of the coordinates.
- Let $S$ be the set of coordinates where $a,b$ agree. 
- Check that $f(a)_S = f(b)_S$

> Preliminary thoughts on Q1:
> I can prove something weaker than their statement, namely that if $g$ passes the direct product test with probability $1-\epsilon$ then
> $\Pr[g_i(x)=g_i(y)\mid x_i = y_i]>1-4\epsilon.$
> I can prove this with a very simple coupling argument. But getting the full theorem seems a *bit* tricky.

Their direct product test follows from some more general theorem and showing that something is an HDX. [[some-HDX-definitions]]. The somehow computing the eigenvalues of some matrix helped them out. 

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

Throughout the analysis we will use the following basic fact from probability theory many times:

> [!fact] 
> Let event $E$ be a function of $x,b$. \
> Suppose that for all $b$, $\Pr_x[E(x,b)]> \rho_b.$\
> Then, $\Pr_{x,b}[E(x,b)] > \E[\rho_b]$.


Define $S_x(a,b)$ to be the vector obtained by taking $a_i$ whenever $x_i = 0$ and  $b_i$ whenever $x_i = 1$. 
We can also write this as $a\bar{x}+bx$ where the vector multiplication is done elements wise.

Define $\Delta(a,b) \subseteq [d]$ to be the set of indices where $a,b$ differ. 

> [!tip]  "Square in Cube" test (for being a direct sum)
> Sample random $a,b\in [n]^d$ and $x,y\in \F_2^{d}$ and test:
> $$ 
> f(S_0(a,b)) + f(S_x(a,b))+ f(S_y(a,b))+f(S_{x+y}(a,b))=0.
> $$ 
> 
> (Note that this clearly passes on direct sums)

> [!tip] Theorem 1
> If $f$ passes the Square in Cube test with probability $1-\eps$, then there is an absolute constant $C<2^{100}$ such that $f$ is $C\eps$-close to a direct sum.

Let 
$$ 
\eps_{ab}=\Pr_{x,y}[f(S_0(a,b)) + f(S_x(a,b))+ f(S_y(a,b))+f(S_{x+y}(a,b))\neq0].
$$
Let $\eps_a = \E_b [\eps_{ab}]$. Suppose $f$ passes the Square in Cube test with probability $1-\eps$. Then, $\E_{ab} [\eps_{ab}]=\E_a [\eps_a] = \eps$.


It is valuable to re-express $\eps_{ab}$ as follows:
$$ 
\eps_{ab}=\Pr_{x,y}[(f(a) + f(S_x(a,b)))+ (f(a)+f(S_y(a,b)))\neq f(a)+f(S_{x+y}(a,b))].
$$

For each $(a,b)$, the **BLR test** implies the existence of a character $I_{ab}\subseteq \Delta(a,b)$ such that
$$
\Pr_x[f(a)+f(S_x(a,b))=\chi_{I_{ab}}(x)]>1-\eps_{ab}.
$$

Let $F_a(b)\in \F_2^{d}$ be the indicator vector for $I_{ab}\subseteq [d]$. 

> [!tip] Lemma 1
> $F_a(b)$ is close to a direct product $(g_{1}(a,b_1),\dots,g_d(a,b_d))$

**Proof of Lemma 1:** \
Define distribution $b'\sim D_{b}, b'\in [n]^d$ as follows: 
- Sample $b'$ uniformly randomly, conditional on having $b_i'= b_i$  hold for exactly $3d/4$ of $i\in [d]$. 

Define distribution $x\sim X_{bb'},x\in \F_2^d$ as follows:
- $x_i=0$ for all $i\in \Delta(b,b')$,
- If $b_i=b_i'$ then $x_i=1$ with probability $2/3$ and $0$ with probability $1/3$.

Observe:\
For any $x_{0}\in \F_{2}^d$, $\Pr_{b,b'\sim D_{b},x\sim X_{bb'}}[x=x_{0}]=1/2^d$. \
That is, $x$ is uniformly distributed, and pairwise independent of $b,b'$, although of course the triple $(x,b,b')$ is dependent. 

By construction, $S_x(a,b) = S_x(a,b')$ (whenever $b_i,b_i'$ differ, we choose $a_i$).

Then, by a union bound (and averaging away $b$ in our earlier expression) we have that 
$$
\Pr_{b,b'\sim D_{b},x\sim X_{bb'}} [\chi_{F_a(b)}(x) =f(a)+f(S_x(a,b)) = f(a)+f(S_x(a,b')) = \chi_{F_a(b')}(x)] > 1-2\eps_{a}.
$$
Let 
$$
\delta_{abb'} =  \Pr_{x\sim X_{bb'}} [\chi_{F_a(b)\oplus F_a(b')}(x) \neq 0].
$$
Let $\overline{\Delta}(b,b')$ denote the set of indices $i$ where $b_i=b_i'$. For vector $x\in \F_2^d$, and set $S\subseteq [d]$, let $x\mid_S$ denote the restriction of $x$ to coordinates $S$. Then,

>[!tip] Claim 1
>If $\delta_{abb'}<1/3$ then $F_a(b)\mid_{\overline{\Delta}(b,b')}= F_a(b')\mid_{\overline{\Delta}(b,b')}$.
>
>**Proof of Claim:**\
>Assume for sake of contradiction that there exists  $i\in \Delta(F_a(b), F_a(b')) \cap \overline{\Delta}(b,b')$.
>Sampling $x\sim X_{bb'}$ we have a $1/3$ chance of $x_i$ being $0$ and a $2/3$ chance of $x_i$ being $1$, independent of the values of $x_j$ for $j\neq i$.
>But then $\chi_{F_a(b)\oplus F_a(b')}(x)$ doesn't take on any value in $\F_2$ with probability greater than $2/3$, contradicting $\delta_{abb'}<1/3$.
>Thus, $\Delta(F_a(b), F_a(b')) \cap \overline{\Delta}(b,b') = \varnothing$.

Now, by Markov's Inequality we have that 
$$
\Pr_{b,b'\sim D_{b}}[\delta_{abb'}>1/3] \le 3\E_{b,b'}[\delta_{abb'}] < 6\eps_a.
$$
From this we conclude
$$
\Pr_{b,b'\sim D_{b}} [F_a(b)\mid_{\overline{\Delta}(b,b')}= F_a(b')\mid_{\overline{\Delta}(b,b')}] > 1-6\eps_a.
$$
By the direct product test we conclude that $F_a(b)$ is $O(\eps_a)$-close to a direct product $(g_1(a,b_1),\dots,g_d(a,b_d))$. 

Combining these facts we have:
$$ \Pr_{x,b} \left[ f(a)+f(S_x(a,b)) = \sum_i g_i(a,b_i)x_i \right] > 1-O(\eps_a).$$ 
This concludes the proof of Lemma 1 $\square$.

---
Now we conclude the proof of the theorem.

Observe that $S_{x}(a,b)=S_{\bar{x}}(b,a)$. Thus, by Lemma 1 and a union bound,
$$\Pr_{a,b,x} \left[ \sum_{i}g_{i}(a,b_{i})x_{i}= f(a)+f(S_x(a,b))=f(b)+f(S_{\bar{x}}(b,a))=\sum_{i}g_{i}(b,a_{i})\overline{x_{i}} \right]>1-O(\eps).$$
Define 
$$
\lambda_{ab}=\Pr_x\left[ \sum_i (g_i(a,b_i)+g_i(b,a_i))x_i \neq \sum_i g_i(b,a_i) + f(a)+f(b) \right].
$$
Clearly, if $\lambda_{ab}<1/2$  then we must have 
$$
g_i(a,b_i)=g_i(b,a_i)
$$
for all $i$, and also 
$$
\sum_i g_i(b,a_i)=f(a)+f(b).
$$
By Markov's inequality:
$$
\Pr_{a,b}[\lambda_{ab}>1/2]< 2\E_{a,b}[\lambda_{ab}]< O(\eps).
$$
Thus, we have:
$$
\Pr_{a,b}\left[ f(a)=f(b)+\sum_i g_i(b,a_i)  \right] > 1-O(\eps).
$$
Then, by the probabilistic method there exists $\beta$ such that 
$$
\Pr_a\left[ f(a)=f(\beta)+\sum_i g_i(\beta,a_i) \right]>1-O(\eps).
$$
That is, $f$ is $O(\eps)$-close to the direct sum $a\mapsto f(\beta)+\sum_i g_i(\beta,a_i)$.
$\square$

## open questions
- Can you analyze this test with Fourier analysis?
- ~~Can reconstruct the function using the Shapka scheme?~~
- Does this other test also works for direct-sum testing? [[thoughts-on-test-8]]
- (Alek): higher order dependencies (i.e., not just a direct-sum, but maybe a sum of things that are allowed to depend on e.g., two terms at a time) 
	- Kai: that sounds gross?
	- It's called Junta degree
	- people think about it
Low-Degree Testing Over Grids
arXiv:2305.04983v1 cs.CC 8 May 2023
Prashanth Amireddy* Srikanth Srinivasan† Madh
Can you improve their analysis?
best you could hope for
reject with probability q\*distance
sometimes can get this.
optimal testing
they don't really give any bound...

- is the test self-correcting? [[square-in-cube-self-correcting]]

