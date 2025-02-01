This is a fundamental result in learning theory and crypto.

Discussion based on some lecture notes by Ryan O'Donnel.

**Def**: $B$ is a hardcore predicate for OWF $f$ if any PPT algorithm $A$ has negligible advantage at guessing $B(x)$ given $f(x)$.

**Fact**: OWP + hardcore predicate --> PRG

**GL Theorem**:
If $f$ is a OWP, then 
- $g(x,r)=(f(x),r)$ is also a OWP, 
- and $x\cdot r$ is a hardcore predicate.

Apparently this follows as a simple corollary of a related thing:

**GL Theorem prime**
Given query access to $f:\{0,1\}^{n}\to [-1,1]$, 
find the large Fourier coefficients of $f$.

More specifically, given $\gamma, \delta>0$ want to output a list $L$ containing all the coefficients with weight  $\ge\gamma$, and such that at most a $\delta$-fraction of the list $L$ is coefficients of weight $<\gamma/2$.


**Lemma**
Can estimate Fourier coefficients by sampling to approx $\mathbb{E}[f\cdot \chi_S]$.

----

Let's prove GL theorem prime:

**Lemma 1**
At most $1/\gamma^{2}$ frr coeffs with weight larger than $\gamma$.

Pf:
> We have $||f||_2^{2} = \sum \hat{f}(S)^{2} = \mathbb{E} f^{2} \le 1$.


**Lemma 2**
Suppose we have a "wild card indicator string" something like this 10xx110xx0x110x. 
What this really refers to is the set of all ways of completing this thing to a binary string by replacing the $x$'s with $0,1$.
Anyways, it turns out that we can efficiently estimate the weight of one of these things!

**proof:**
Deferred

vibes are just "this is a reasonably nice expression -- so we are justified in guessing it has a nice fourier analytic interpretation".

Suppose that we had the above thing figured out. 
Then you can do the classic tree thing to find all large Fourier coefficients.

**Remark**
If your function has a $1-\varepsilon$ fraction of Fourier mass concentrated in a small number of Fourier coefficients then this means that we can learn the function.

---

how does this relate to the crypto thing?

well, if $\Pr_{x,r}[A(f(x),r) = x\cdot r] > 1/2 + 1/n^{c}$, then for many $x_{0}$ it's the case that 
$$
\Pr_r[A(f(x_{0}),r)=x_{0}\cdot r] \ge 1/2 + 1/n^{c+1}.
$$

This means that $r\mapsto A(f(x_{0}),r)$ is strongly correlated with a character / has a large fourier coefficient, so we can go rip out that fourier coefficien. We use GL to get a list of all the big fourier coefficients, and then try to find one that's consistent with the data.
Then we output that. 
This means we inverted $f$ with probability $1/n^{c}$, which is impossible.

