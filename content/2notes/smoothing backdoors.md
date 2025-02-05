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

