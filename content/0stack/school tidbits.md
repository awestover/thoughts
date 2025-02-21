I'm mostly not focusing on school right now -- spending time on research / projects seems more useful and more interesting and also better for learning things. However occasionally some of the school stuff seems pretty cool actually! So I'll write down some of that stuff here to remember. 

# AIRR
BP is cool. Think of BP on a tree as "collapsing factor subtrees".

A variable does:
- product of messages from all touching factors

A factor does:
- marginalize out the children ( product of all messages from child vertices )

sum-product works. 
max-product does too.

---

Sampling in a Bayes net:
- just do it -- toposort

Conditional sampling in a Bayes net:
- rejection sampling -- works, but slow
- importance sampling: 
- If $\mathsf{suppport}(Q) \supseteq \mathsf{support}(P)$:
$$
\mathbb{E}_{x\sim P}[f(x)] = \mathbb{E}_{x\sim Q}\left[ \frac{P(x)}{Q(x)}f(x) \right].
$$
- They recommend: use $Q$ which is "do ancestral sampling but with the observations fixed".

#todo: question for me: do we really expect this to converge faster than just sampling from $P$?
Maybe could run an experiment about this with some Bayes nets. It seems pretty weird.

**Gibbs Sampling**
- Repeatedly:
	- Choose a random variable 
	- Resample its value conditional on the other variables settings
	- Note that in a factor graph it suffices to look at the neighbors to do this computation
	- So it should be relatively cheap. O(number of factors the var is involved in).

#todo: prove that MC induced by Gibbs sampling converges to correct joint dist. can we say how fast it converges?

#todo: how is this different from Metropolis-Hastings alg?

# Inf + Info
LRTs are good. Sometimes need randomness to get a full ROC curve. 

They gave some nice motivation for what "decision rules" are:

- maybe you have priors and costs --> can choose rule to minimize E(cost).
- maybe have priors but no costs, maybe just have some tolerable FPR --> can see what ROC curve you can get
- maybe have costs but no priors --> take worst case prior ("minimax formulation")

# Networks 
- Measure "importance" of a vertex as "average importance of neighbors".
	- Can interpret these as steady-state probabilities if it's a Markov Chain.
- Can also give people some base importance.
	- This is like a Markov Chain but with some teleportation probability.

#todo -- figure out what else is happening in networks


pr, CIM, quantum -- so far are completely fake classes
fortunately they are approx zero work.
