I'm mostly not focusing on school right now -- spending time on research / projects seems more useful and more interesting and also better for learning things. However occasionally some of the school stuff seems pretty cool actually! So I'll write down some of that stuff here to remember. 

# AIRR

So far this class is mostly about inference. 
Usually you have a set of local constraints $\phi_i$ and define a joint distribution 
$$
\Pr[X=x] \varpropto \prod_i \phi_i(x).
$$
One thing that should be obvious but wasn't to me is that scaling a single $\phi_i$ does weird things to this probability -- it "weights that constraint more". 

However, there are some situations where this behaves in a pretty reasonable way. 
For instance, the distribution $X\mid E$ is obtained by adding a factor $\phi = \mathbb{1}[E]$.

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
- **importance sampling:** 
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


**feb 25**
Here is a formula:

$$\mathbb{E}[X∣CX=d]=\Sigma C^{\top} (C\Sigma C^{\top})^{-1} d.$$

Here is an example: let $A,B$ be independent unit Gaussians
$$
\mathbb{E}[(A,B) \mid A+4B=1] = \frac{1}{17}, \frac{4}{17}.
$$

More generally, 
$$
\mathbb{E}[\alpha A \mid \alpha A+ \beta B=\gamma] = \frac{\gamma \alpha^{2}}{\alpha^{2}+\beta^{2}}.
$$


**Markov Blanket** of a vertex $v$ is the set of nodes whose value you must fix in order to make the value of $v$ independent of all the other values. 
Surprisingly, the answer is: (1) parents, (2) children, (3) AND parents of children. 
This is not so surprising if you've ever seen the fact that the V Bayes net$(X,Y)\to Z$ has the property that $X\perp Y$ but $X\not\perp Y \mid Z$.


# Inf + Info
LRTs are good. Sometimes need randomness to get a full ROC curve. 

They gave some nice motivation for what "decision rules" are:

- maybe you have priors and costs --> can choose rule to minimize E(cost).
- maybe have priors but no costs, maybe just have some tolerable FPR --> can see what ROC curve you can get
- maybe have costs but no priors --> take worst case prior ("minimax formulation")


**What if you don't have a prior?**
you can graph $P_e(p) = \bar{p} P_F(\eta)+p (1-P_D(\eta))$ where $\eta=\frac{\bar{p}}{p}$.
that's what we could achieve if prior is $p$ and we knew that. 

but ofc we don't know the prior. 
So we can instead achieve something more like $\bar{p} P_F(\eta)+p (1-P_D(\eta))$ where $\eta=\frac{\bar{q}}{q}$.

clearly in the adversarial setting we choose this line to have slope zero.

equivalently this says look for intersection of OCLRT and line $P_F=1-P_D$.


more generally with some costs, the minimax optimal decision rule (ie decision rule with best performance on whatever prior is worst for this decision rule) 
could be intersection of 
OC-LRT and $P_D=1-\frac{C_{10}}{C_{01}}P_F$.

don't even ask about the case $C_{00},C_{11}\neq 0$ like come on thats just gross.

i think you can tell if you need a mixed strat based on if the intersection of the curves is in a part of the curve that required a mixed strat

> Fact: in fact, the optimal decision rule satisfies 
> $$ \min_r \max_p \phi(p,r) =\max_p \min_r \phi(p,r) $$
> By thinking of it as a two player game. 
> This turns out to be really great because RHS is much more tractable.

Credit to Kevin for explaining all this to me much better than the lecture notes!

> Moreover, the expected cost if the prior was $p^{*}$ and you knew it should be the same conditional on the hypothesis being $H_{0}$ or $H_{1}$, where $p^{*}$ is the optimum $p$ in the RHS thing.

something to be careful of:
> If the curves don't intersect then the answer is either $0$ or $1$. hopefully you can just think about this to figure out which one is correct.

remark: minimax inequality is true in general and is pretty good.

I think I get the basic point although might read their examples at some point.

> Fact:  Every point on efficient frontier is achieved by a (possibly randomized) LRT test.

**cost formulations:**

If $|a-\hat{a}|$ then you should set $\hat{a}=\text{median}(a)$.
If $|a-\hat{a}|$ penalizes all errors the same above a small threshold you should basically use the MAP rule.


**MSE loss**: choose $\hat{x}=\mathbb{E}[x\mid y]$

fun fact about MSE estimator: 

$\hat{x}(y)-x$ is uncorrelated with every function $g(y)$


**Recall:** 
$A$ curly geq $0$ means $A$ is PSD ie $u^{\top}A u\ge 0$ for all $u$.

Fact: error covariance matrix for BLS estimator is better than for any other estimator

and something like
$$
\mathbb{E}[\Lambda_{x\mid y}(y)] \le \Lambda_x
$$
so on average more info is helpful for estimation -- although some info can hurt your estimate by being misleading.


# Networks 
- Measure "importance" of a vertex as "average importance of neighbors".
	- Can interpret these as steady-state probabilities if it's a Markov Chain.
- Can also give people some base importance.
	- This is like a Markov Chain but with some teleportation probability.

#todo -- figure out what else is happening in networks

---

pr, CIM, quantum -- so far are completely fake classes
fortunately they are approx zero work.
