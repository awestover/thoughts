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

loss: 
$$L_D(f)  = \Pr_{(x,y)\sim D}[f(x)\neq y]$$

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

strong notion of a mitigator:
output should have small TV distance from if you sample from clean 

they have this nice function $\lambda$ which means "the part of
the outputs that you care about"

---

Theorem:
global mitigation for fourier heavy

proof:
GL theorem


#todo understand this

