$\newcommand{\eps}{\varepsilon}$
$\newcommand{\F}{\mathbb{F}}$
$\newcommand{\E}{\mathbb{E}}$
$\newcommand{\RM}{\mathsf{RM}}$
# An Alternate Analysis of BLR test
Here we give a purely combinatorial analysis of the BLR test  (i.e., no Fourier Analysis). 
This proof is quite nice, and generalizes to some other situations.

In this post I will focus on characteristic $2$, although I think this should generalize with a bit more care about the negative signs. 

Fix $\eps\in (0,1/1000)$.
Suppose that $f:\F_2^n\to \F_2$ has $\Pr_{x,y}[f(x+y)=f(x)+f(y)]\ge 1-\eps$.
Define the **corrected** version of $f$ to be $g(x)=\text{plurality}_a(f(a)+f(x-a))$.
We will show that $f$ is close to $g$, and that $g$ is linear. 

We note that another interpretation of this is, if $f$ is a slightly corrupted version of linear function $L$ then we can recover the values of $L$, namely $L=g$.

**Lemma 1:**
For all $x$, $\Pr_{a}[f(a)-f(x-a)=g(x)]\ge 1-4\eps$.

**proof:**
Fix $x$. Sample $a,b,y$ randomly.
Consider $f$ applied to all of the elements of this table:

|       |         |           |
| ----- | ------- | --------- |
| $x$   | $x-a$   | $a$       |
| $x-b$ | $y$     | $x+y-b$   |
| $b$   | $x-a+y$ | $a+x+y-b$ |

By a union bound, with probability $1-4\eps$, the final two rows and the final two columns are "passing instances of the BLR test". That is, $f(x-a)+f(y)=f(x-a+y)$ and so on.
Now, add up *all* entries in the square. 
By viewing the sum in two different ways (i.e., using the fact that the last two columns and last two rows all sum to zero when you apply $f$ to them), we find:
$$
f(b)+f(x-b)=f(x-a)+f(a).
$$
We have shown 
$$
\Pr_{a,b,y}[f(b)+f(x-b)=f(x-a)+f(a)] \ge 1-4\eps.
$$
Thus, there exists $b$ such that 
$$\Pr_a[f(x-a)+f(a) = f(b)+f(x-b)]\ge 1-4\eps.$$
Clearly this implies that $g(x)=f(b)+f(x-b)$, and then we have the desired result about $g$ agreeing with $f(x-a)+f(a)$ with good probability. 
$\square$


**Lemma 2**: $g$ is linear.

Fix $x,y$. Sample $z,z'$ randomly.
Consider the following table.

| $g(x)$   | $g(y)$    | $g(x+y)$      |
| -------- | --------- | ------------- |
| $f(z)$   | $f(z')$   | $f(z+z')$     |
| $f(x-z)$ | $f(y-z')$ | $f(x+y-z-z')$ |

With probability $1-14\eps>0$ we have that all columns sum to zero and the final two rows sum to zero. That is, 
$$
g(x)=f(z)+f(x-z), g(y)=f(z')+f(y-z'), g(x+y)=f(z+z')+f(x+y-z-z').
$$
and
$$
f(z)+f(z')=f(z+z'), f(x-z)+f(y-z')=f(x+y-z-z').
$$
Thus, by the probabilistic method there exist $z,z'$ such that the above 5 events all happen. 
But if all columns sum to zero and the final two rows sum to zero then the first row sums to zero as well. That is:
$$
g(x)+g(y)=g(x+y).
$$
$\square$

**Lemma 3:** $g$ is close to $f$. 

**Proof:**
By a union bound,
$$
\Pr_x[g(x)\neq f(x)] \le \Pr_a[g(x)\neq f(x)+f(x-a)]+\Pr_a[f(x)\neq f(a)+f(x-a)]\le 5\eps.
$$
$\square$

In conclusion, $f$ is close to the linear function $g$. 

# Basic Coding Theory

A **linear code** of length $N$ over $\F_2$ is a linear subspace of $\F_2^N$.
Example: $\RM_2(m,d) = \{f:\F_2^m\to \F_2 \mid \deg(f)\le d\}$; or really it's the truth tables of these functions. This is a linear subspace of $\F_2^{2^m}$ because the sum of two degree $d$ functions is degree $d$.
Remark: any $f:\F_2^m\to \F_2$ can be written as a multilinear polynomial. 

### Dual Code
Given a code $V$, the dual code is $V^\perp$ --- the set of all orthogonal codes.

> [!info] Example
> BLR test. Code: $\RM(m,1)$. 
> Test: define $h$ to be the indicator function for the set $x, y, x+y$. 
> The set of all possible $h$'s is the set of minimum weight dual codes. 
> Then, sampling a random minimum weight dual code $h$ and checking $\langle f, h\rangle=0$ is exactly the BLR test. 

Another perspective: 
$$
\sum_{x\in\F_2^m} \prod_{i\in S} x_i = 1[S=[m]].
$$
That is, $\langle x_S, 1\rangle$ is $0$ unless $x_S$ is the "full monomial", in which case the inner product is $1$. 

So, we have:
$$
\RM(m,d)^\perp = \RM(m,m-d-1).
$$
Because if you take an inner product of a degree $d$ and a degree $m-d-1$ polynomial you always get zero, but if you have a degree $\ge m-d$ polynomial, then there are some degree $\le d$ polynomials that it doesn't dot to zero with. 

### Cube perspective

Restrict function to a cube, check its degree. 
Inner product with indicator function of a cube. 
Note: indicator function of a dimension $d$ cube is a degree $m-d$ polynomial!
