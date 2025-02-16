A #technical note on Neekon, Vinod, et al paper
See also [[backdoors and deceptive alignment]] [[backdoors take 2]].

Prior result of Vinod: 
- you can plant a backdoor such that 
- backdoored model is comp indistinguishable from unbackdoored

this paper: 
- defend even if undetectable?

what's going to make this possible: 
- assuming something like "goodness is simple"
- They consider G="Fourier heavy functions"
	- global mitigation
- Also consider $G=$ polynomial function in $\mathbb{R}^{n}$
	- Here they give a "local mitigation strategy"
	- rm backdoors on specific inputs

> nb: their constructions are blackbox. 
> openq: can we utilize whitebox access somehow?


motivation:
- random reduction and amplification


**Results**
- global for fourier heavy
- local for polynomials

**technical overview**

loss v0: 
$$L_D(f)  = \Pr_{(x,y)\sim D}[f(x)\neq y]$$
They care about some other loss later.

backdoor: 
an adversary is allowed to choose any $f$ that achieves low loss
it wants. and adversaries goal is to get $f$ to fail on some
specific input/inputs.

plan: 
"cannonicalization"

what we get:

- samples of points with true labels
- black box access to potentially backdoored function

- goal is to produce a new function, also with low loss, but such that the errors aren't at adversarial places. basically we want the errors to be similar to if we sampled from a "clean distribution"

- remark: one easy way to do this is to re-learn the function from scratch via the samples
	- if you have a trusted learning algorithm. sigh.
- the goal is to be cheaper than that. 

strong notion of a mitigator:
- output should have small TV distance from if you sample from clean 

they have this nice function $\lambda$ which means "the part of
the outputs that you care about"

---


**Def**:
Frr $\tau$-heavy: all frr coeffs are either $0$ or larger than $\tau$.

**Theorem:**
- Let $D$ be $O(\tau^{2})$-close to $\tau$-heavy by L2 distance.
- Suppose $\tilde{f}$ is $O(\tau^{2})$-close to $D$.
- Then there is a global mitigator $M$ 
	- That uses blackbox access to $\tilde{f}$
	- and samples from $D$
	- to output $g$
	- such that $g$ is still pretty close to $D$
	- but it's "NICE".
		- means TV distance from function sampled from 'ideal canonical dist" is negligible
		- but what does that mean? is it good?
		- ok crumb, the TV distance thing is pretty darn strong!
		- Let $\mathcal{G}^{\mathsf{can}}_D$ be the canonical distribution
		- Let $\mathcal{G}$ be the distribution output by $M$ running on $f,D$
		- The goal is that 
		- $\sum_{g\in \mathsf{supp}(G)} |\Pr[g\gets G] - \Pr[g \gets G^{\mathsf{can}}]|$ is negligible.

**Proof:**
Let $h$ be the $\tau$-heavy function that the ground truth is close to.
If $\tilde{f}$ is close to $D$, then it must have large frr coeffs in the same places as $h$. 
We use [[Goldreich-Levin Theorem]] to recover those frr coeffs. 
Then we estimate the value of these frr coeffs.
Output a function based on the values of these heavy frr coeffs.

---

**Overview of linear local mitigation --** 

- We have a bounded convex set $X$
- there is some affine function $h:\mathbb{R}^{n}\to \mathbb{R}$
- such that $D$ is $\varepsilon$-close to $h$ in some sense called "cutoff loss"
- then there is a function $g^{ideal}$, 
- such that we can learn a function which 
- still gets low loss on $D$
- and st for any particular point $x^{*}$, the pr of being more than $\delta$ far from $g^{ideal}$ on $x^{*}$ is neglig.

We could do linear regression if we took $\Omega(n)$ samples from $D$. 
Instead they're going to take $O(s)$ queries to $f$.
I think of $s$ (security param) as maybe $\sqrt{ n }$ or $n$ -- so this is pretty good I guess.

Apparently there are quite a few technical challenges to get their algorithm to work. 


---

there are some reasons why their guarantees are weak. 

to get stronger results, they assume that 
the ground truth is affine $+$ random noise rather than affine $+$ adversarial noise.

- this lets them require fewer queries
- and get random errors within bars

---

they generalize to polynomials


---
remark 
is cheaper than learning from scratch, eg for degree $\log n$ polynomials.


delta cutoff loss: 
- Pr(outputs differ by more than $\delta$)

---

#todo

Id like to read about at least high level how they did linear mitigation

no time rn tho

---
# robust mean estimation

Suppose you have some mixture distribution $D = \frac{2}{3}U + \frac{1}{3}Q$.

Where we think of $U$ as being nice thing that we care about, say $U([-1,1])$ and $Q$ as being adversarial noise, say a really large value.

There's an algorithm called "median of means". It doesn't perform well here. 
Maybe med of means is really good (converges fast) in settings where you have high variance
/ some weirdly large tail.

anyways, doesn't work here. 

they do mean of medians.

they get some decent thing, altho the convergence is slower than what youd get in a non-adversarial setting.

---
# openqs
- are there other $D$ besides $\tau$-heavy functions and linear/polynomials that are "smoothable"?
- can you do better with whitebox access to $f$

## philosophy

How, if at all, is this relevant to AI alignment?

- this is vaguely similar to a control strategy proposed by Buck
	-  namely, replacing suspicious model outputs by resampled outputs from the model
	- or from a weaker model

- you could hope that $C\circ f$ is really simple.

- remark -- an alignment strategy which requires doing an extra forward pass is probably an unacceptably high alignment tax. rip.
	- wait actually this is not obvious. bc progress is exponential
	- so if you are at the head of the race maybe your models are 10x faster than other guys
	- you can afford to lose a factor of 2
	- maybe

hmm. 

I'd actually rather deal with things that discuss neural networks.

Q: are neural networks really the right level of abstraction though?

Like, is saying $f \in \mathsf{NN}$ actually a real constraint?

If NN's are "universal" function approximators, then this feels strange. 


ok but what does this say about deceptive alignment?

im consulting 
 [[Solving AI Safety from 1st principles]]