$$\newcommand{\eps}{\varepsilon}$$
In this note I summarize "Direct Sum Testing: The General Case", by Dinur and Golubev.

## Direct Product Test
> [!bug] Q1: How to analyze the Direct Product Test?

> Preliminary thoughts on Q1:
> I can prove something weaker than their statement, namely that if $g$ passes the direct product test with probability $1-\eps$ then
> $\Pr[g_i(x)=g_i(y)\mid x_i = y_i]>1-4\eps.$
> I can prove this with a very simple coupling argument. But getting the full theorem seems tricky.

## Shapka Test
This one is pretty clear. 
If 
$$\Pr_{a,b}[f(b_1,b_2,b_3)=f(a_1,a_2,b_3)+f(a_1,b_2,a_3)+f(b_1,a_2,a_3)]>1-\eps,$$
then, $f$ is close to $f^{a}$ for some $a$, where $f^{a}$ is
defined as 
$$ f^{a}(b)  = f(a_1,a_2,b_3)+f(a_1,b_2,a_3)+f(b_1,a_2,a_3). $$ 
In particular, $f^{a}$ is a direct sum. 

On the other hand, if $f$ is a direct sum $f=\bigoplus_i g_i$ then
(because we are working modulo $2$),
$$ f(a_1,a_2,b_3)+f(a_1,b_2,a_3)+f(b_1,a_2,a_3) = g_1(b_1)+g_2(b_2)+g_3(b_3) = f(b_1,b_2,b_3).$$ 

## Square in Cube Test
>[!bug] Q2: Am I understanding this test right? 

Define $S_x(a,b)$ to be the vector obtained by taking $a_i$
whenever $x_i = 0$ and  $b_i$ whenver $x_i = 1$. 
The test direct-sum test is: 

> Sample random $a,b,x,y$ and test:
> $$ f(S_0(a,b)) + f(S_x(a,b))+ f(S_y(a,b))+f(S_{x+y}(a,b))=0.$$ 

Suppose this test passes with probability $1-\eps$.
Then, for the vast majority of $a,b$ we have
$$ \Pr_{x,y}[f(S_0(a,b)) + f(S_x(a,b))+ f(S_y(a,b))+f(S_{x+y}(a,b))=0]>1-\eps.$$ 
By the BLR test, this implies the existence of $I_{ab}\subseteq [k],c_{ab}\in \set{0,1}$

Let $F_a(b)$ be the indicator vector for $I_{ab}$. Let $c_a(b) = c_{ab}$.
We claim that $F_a,c_a$ are close to direct products. 

Sample $b,b'$ uniformly randomly, conditional on them differing in $1/4$ of their coordinates. 
Let $x$ be $0$ on the coordinates where $b,b'$ differ, and $0$
with chance $1/3$ on the coordinates where $b,b'$ agree. 
Then, $x$ is uniformly randomly distributed. 

By construction, $S_x(a,b) = S_x(a,b')$.
By a union bound, we have that 
$$ \Pr_{b,b',x} [\chi_{F_a(b)}+c_a(b) = \chi_{F_a(b')}+c_a(b')] $$ 
is quite large.
This means that for most $b,b'$ we have that 
$$ \Pr_{x} [\chi_{F_a(b)}(x)+c_a(b) = \chi_{F_a(b')}(x)+c_a(b')] $$ 
is large, whereby we deduce that $F_a(b)\mid_{C_b\cap C_{b'}} = F_a(b')\mid_{C_b\cap C_{b'}}$.

>[!bug] Q: I'm not so sure about the constant thing.

At this point we deduce that $F_a,c_a$ are close to direct products. 

By this point we have deduced that for a $1-\eps$ fraction of
$a$'s, there exist functions $g^{a}_1,\ldots,g^a_k,c^a_1, \ldots,
c^a_k$ such that 
$$ \Pr_x [f(S_x(a,b)) = \bigoplus_i (g_i^a(b_i)x_i+c^a_i(b))] > 1-100\eps.$$ 

We finally claim that the functions $g,c$ do not depend very much
on $a$, so we can select a single global $g,c$ that do basically
just as well. 

This is by a coupling argument plus union bound.

Let $a,a'$ be two arbitrary points where $g^a,c^a$ are good
approximations of $f$ locally. 
Sample $x$ uniformly randomly. 
Observe that for any $x$,
$$ S_x(a, a') = S_{\overline{x}}(a', a).$$ 
So, 
$$ \Pr[g^a = f(S_x(a,a')) \land g^{a'} = f(S_{\overline{x}}(a',a))] $$ 
is very high.
That is, $\Pr[g^a = g^{a'}]$ is quite high. 
So we win. 

#todo: this is kinda sus.

