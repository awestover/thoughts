We use the notation described in [[direct-sum-testing-grid]].
Define $a_x b$ to mean the string with $i$-th bit equal to $a_i$ when $x_i=0$  and $b_i$ when $x_i=1$.
As a first step towards analyzing test 8, we'd like to prove the following theorem:

> [!tip] Theorem
>  Suppose $f:\F_2^d\to \F_2$ has
>  $$
> \Pr_{a,x,b}[f(a)+f(b)=f(a_xb)+f(b_xa)]\ge 1-\eps.
> $$
> Then, $f$ is close to a linear function.

**thoughts**
It's pretty easy, and highly instructive, to see that the statement is true when $\eps=0$.
For instance, if $\eps=0$ we have
$$
\forall x,a: f(a) = f(0)+f(a_x 0) + f(0_x a).
$$
So, we can take $f(a)$ and decompose it into $\sum_i a_i\cdot (f(e_i)+f(0))$.
And this clearly makes our function linear. 

Ok, but what if $\eps>0$?
It feels like, by renaming the coordinates you should be able to assume: 
$$
\Pr_{b,x}[f(b)=f(0_xb)+f(b_x0)]
$$

It almost feels like you should have 
$$
f(a)+f(b)=f(S_b(a,b))+f(S_b(b,a))
$$
If you did it'd be great, because then there must exist $z,c$ such that 
$$
S_b(a,b)=c, S_b(b,a)=0_zc, a+b = c_z 0.
$$
Like basically what has happened here is that we've swapped $b$'s ones up to $a$. 
So now, the set of indices where $a+b$ has ones, and the set of indices where $S_b(b,a)$ has ones unioned together equal the set of indices where $S_b(a,b)$ has ones.

So then we'd "basically" have:
$$
f(a)+f(b) = f(c)+f(0_z c) = f(c_z 0) = f(a+b).
$$
And then we'd pass the BLR test and then we'd win. 

But of course all of these dependent choices are highly illegal, so this is baloney.

---

Maybe a way to analyze their test 8 if we repeat it twice?
$\rho_{ab}(x)+\rho_{ab}(y)+\rho_{ab}(x+1)+\rho_{ab}(y+1) = \rho_{a'b'}(0)+\rho_{a'b'}(x')+\rho_{a'b'}(y')+\rho_{a'b'}(x'+y')$
doesn't seem promising to me: the $x,x+1$ stuff are too different.  

---

# Test 8

Note: 
The following natural conjecture is false:
Conjecture (known to be false): For all $a$,
$$
\Pr_{x,y,b,c}[f(a_xb)+f(b_xa) = f(a_yc)+f(c_ya)].
$$

Remark: I'd be interested in a proof that "pass test 8 with probability $\eps$ implies is $\sqrt{\eps} d 100$ close to linear". (even if eventually I hope that the answer is it is $\eps$-close)


Observation: 
For $1-\sqrt{ \eps }$ fraction of $(a,b)$, we have that $f(a_xb)+f(b_xa)$ is $\sqrt{ \eps }$ close to $f(a)+f(b)$.

Important observation: 
If $f$ passes test 8 with probability 1 then it must be linear. 

--- 

