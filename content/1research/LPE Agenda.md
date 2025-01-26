#todo

For more thoughts on alignment problems, see [[MAD Agenda]]

[ARC also thinks](https://www.alignment.org/blog/research-update-towards-a-law-of-iterated-expectations-for-heuristic-estimators/) that explanations could help with low probability estimation.
They give two ideas:
- Maybe $\G(\E_{x\sim D}[C(M(x))]\mid \Pi)$ gives a good estimate of the probability of catastrophe. 
- Maybe $\G(C(M(x^*))\mid \Pi, \text{trace of M on xstar})$ gives a good estimate of the probability of catastrophe, while being much cheaper to compute than $C(M(x^{*}))$.

LPE is also a pretty exciting problem because I can imagine training data, and I can tell what it'd mean to succeed.

# idea1
"Activation modelling"

# idea2
efficiently learn some dependency structure in NN
how to measure??

# Idea 3
"Analytical VAEs"

**problem**
LPE -- KL div issue in modified SAE thing

> However, if catastrophic inputs are rare, the SAE's training data is unlikely to contain any catastrophic inputs. As such, we might not learn features that are informative for estimating the probability of catastrophic inputs.
