I'm mostly not focusing on school right now -- spending time on research / projects seems more useful and more interesting and also better for learning things. However occasionally some of the school stuff seems pretty cool actually! So I'll write down some of that stuff here to remember. 

$\newcommand{\E}{\mathbb{E}}$
$\newcommand{\var}{\mathsf{Var}}$
# AIRR

I actually enjoy this class quite a bit. So far this class is mostly about inference. Usually you have a set of local constraints $\phi_i$ and define a joint distribution 
$$
\Pr[X=x] \varpropto \prod_i \phi_i(x).
$$
> Important fact: **scaling a potential doesn't impact the pr distribution that you get out of this**.

> Another observation: the distribution $X\mid E$ is obtained by adding a factor $\phi = \mathbb{1}[E]$.

> BP is cool. Think of BP on a tree as "collapsing factor sub-trees".

> Another important fact: scaling messages in BP doesn't change the marginals or MAP assignment.

#### Belief Propagation

**Message from Variable to Factor:**
For a variable $x_i$, the message it sends to a connected factor $f$ is the product of all incoming messages from other factors connected to $x_i$ (excluding $f$):

$$ \mu_{x_i \to f}(x_i) = \prod_{f' \in \text{neigh}(x_i) \setminus \{f\}} \mu_{f' \to x_i}(x_i)
$$

Where:
- $\text{neigh}(x_i)$ denotes the set of factors connected to $x_i$. 
- $\mu_{f' \to x_i}(x_i)$ is the message from factor $f'$ to variable $x_i$.

**Message from Factor to Variable:**
For a factor $f$, the message it sends to a connected variable $x_i$ is the product of the factor's value (which depends on all its connected variables) and the messages from all other connected variables (excluding $x_i$), marginalized over all possible values of those variables:
$$
\mu_{f \to x_i}(x_i) = \sum_{\{x_j : j \neq i\}} f(x_1, x_2, \dots, x_k) \prod_{j \in \text{neigh}(f) \setminus \{x_i\}} \mu_{x_j \to f}(x_j)
$$

Where:
- $f(x_1, x_2, \dots, x_k)$ is the factor function that depends on the variables $x_1, x_2, \dots, x_k$ connected to the factor $f$.
- $\text{neigh}(f)$ denotes the set of variables connected to factor $f$.


> Max-product lets you find MAP assignments

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

#todo: question for me: when do we expect this to converge faster than just sampling from $P$?
Maybe could run an experiment about this with some Bayes nets. this seems pretty weird / magical.

**Gibbs Sampling**
- Repeatedly:
	- Choose a random variable 
	- Resample its value conditional on the other variables settings
	- Note that in a factor graph it suffices to look at the neighbors to do this computation
	- So it should be relatively cheap. O(number of factors the var is involved in).

Q: Can we say how fast it converges?
- Vibes are that it's about $n\log n$ for most reasonable $n$-bit systems.

Remark: Metropolis-Hastings alg is something with a proposal distribution $q(x'\mid x)$ which proposes a new sample given some old samples. 

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

**Conditioning Gaussians full**

- note to self -- if there is a midterm and I get to bring notes, I should print this out
- another note: always submit regrade requests when you're actually right

It turns out that if you have $X,Y$ are jointly Gaussian then $X\mid Y$ is also Gaussian and there are some simple formulas for the mean and covar of $X\mid Y$.
These formulas are:
$$
\mathbf{z} = \begin{pmatrix} \mathbf{x} \\ \mathbf{y} \end{pmatrix} \sim \mathcal{N}\left( \begin{pmatrix} \mu_x \\ \mu_y \end{pmatrix}, \begin{pmatrix} \Sigma_{xx} & \Sigma_{xy} \\ \Sigma_{yx} & \Sigma_{yy} \end{pmatrix} \right)
$$
$$
\mathbb{E}[\mathbf{x} | \mathbf{y} = \mathbf{y_0}] = \mu_x + \Sigma_{xy} \Sigma_{yy}^{-1} (\mathbf{y_0} - \mu_y),
$$
$$
\mathsf{Covar}(\mathbf{x} | \mathbf{y} = y_{0}) = \Sigma_{xx} - \Sigma_{xy} \Sigma_{yy}^{-1} \Sigma_{yx}.
$$
The fact that conditioning, marginalizing, and adding jointly Gaussian rvs gives Gaussian rvs is very  nice. 


**Markov Blanket** of a vertex $v$ is the set of nodes whose value you must fix in order to make the value of $v$ independent of all the other values. 
Surprisingly, the answer is: (1) parents, (2) children, (3) AND parents of children. 
This is not so surprising if you've ever seen the fact that the V Bayes net$(X,Y)\to Z$ has the property that $X\perp Y$ but $X\not\perp Y \mid Z$.


---

**Gibbs Sampling**

Question: why does it work?

Answer:

You can show that 
$$
\Pr(x) \Pr(x\to x') = \Pr(x') \Pr(x'\to x)
$$
ie that $\Pr(x)$ is a fixed point of the Markov chain.
Thus, if the Markov chain is guaranteed to converge to a unique value, then it must be this value.

Gibbs sampling is only going to work if the chain is ergodic anyways so fine.

---

**$\alpha,\beta$ algorithm / forwards backwards / sum-product on HMM:**

$$
\alpha_t(s_t) = \Pr(s_t, o_{1:t}); \alpha_t(s_t) = \Pr(o_t\mid s_t) \sum_{s_{t-1}} \Pr[s_t \mid s_{t-1}]\alpha_{t-1}(s_{t-1}).
$$
$$
\Pr[s_t\mid o_{1: t}] \propto \alpha_t(s_t).
$$

$$
\beta_t(s_t) = \Pr[o_{t+1:T}\mid s_t] = \sum_{s_{t+1}} \Pr[o_{t+1}\mid s_{t+1}] \Pr[s_{t+1}\mid s_t] \beta_{t+1}(s_{t+1}).
$$
Getting $\Pr[s_k \mid o_{1:t}]$ -- called "smoothing" -- ie updating estimates of the past based on new observations is more tricky than just computing pr dist over the next state. 
In particular it requires storing history of forwards messages.

Here's how you do it: if you care about smoothing at lag L
- Store the last $L$ forwards messages  $\alpha_{t-L},\dots, \alpha_{t-1}$.
- When a new observation $o_t$ arrives:
	- Compute the new forward message $\alpha_t$ (just one step of the forward algorithm)
	- Run the backward algorithm from the current time t back to whatever past time points we want to smooth
	- Combine the stored forward messages with the new backward messages
- Cost: $O(L)$ where $L$ is the "lag"

#todo: can you prove bounds on when particle method works?

**Particle methods for sampling**

[[particle filter]]

I really like these "smart sampling" approaches. It feels like maybe they should say something interesting about neural networks. But idrk.

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


---

**Exponential families**

$p_y(y;x) = \exp(\lambda(x) t(y) -\alpha(x) + \beta(y))$
- $\lambda$: natural parameter
- $t$: natural statistic
- $\beta$ log base function
- $\alpha$ -- log-partition function

- note not unique
- not everything is expressible in this way -- supposed to be bad if domain depends on param

> "canonical" exp family is one with $\lambda(x) = x$.
> natural param space -- set of $x$ such that the dist is normalizable

> **natural exp family** $\lambda(x) = x, t(y)=y$


> Proposition: **Log partition fn generates cumulants**:
> - $\alpha'(x) = \E[t(y)]$
> - $\alpha''(x) = \var[t(y)]$.
> - If $\var[t(y)]>0$ follows that $\alpha'(x) = \E[t]$ monotonic and thus invertible fn of $x$

> Score function of a canonical expo fam $S(y;x) = \frac{\partial}{\partial x}\ln p_y(y; x) = t(y)-\E[t(y)]$.

> Fisher info of canonical expo fam: $J_y(x) = \var[t(y)]$.

In the general (non-canonical case) we have the following expressions:
- $\alpha'(x) = \lambda'(x) \E[t(y)]$
- $\alpha''(x) = \lambda'(x)^{2} \var[t(y)] + \lambda''(x) \E[t(y)].$
- $\frac{d}{dx}\E[t(y)] =\lambda'(x) \var[t(y)]$
- and so on

---

**Minimum-variance unbiased estimators** (MVU)

- a valid (doesn't depend on param; just on visible data) unbiased estimator with uniformly lower variance than all other estimators
	- may well not exist


**Cramer-Rao bound:**
Suppose $p_y(y;\cdot)$ is positive and differentiable on $X$ and satisfies
$$
\E \left[\frac{\partial}{\partial x} \ln p_y(y;x)\right] = 0.
$$
(which it seems like any moderately reasonable distribution will satsify)
then for any unbiased $\hat{x}$, $\lambda_{\hat{x}}(x) \ge \frac{1}{J_y(x)}$
where $J_y(x) = \E[S(y; x)^{2}]$ fisher info
and $S(y;x) = \frac{\partial}{\partial x} \ln p_y(y;x)$.

remark -- large fisher info means we expect to be able to better resolve the value of $x$ from observations.

If you are tight with Cramer Rao bound then you get $\hat{x}(y) = x + \frac{S(y;x)}{J_y(x)}$ where the dependence on $x$ should be fake to be valid.

**Remark:**
- if tight then unique!
- clear by the fact that we can just write down the above expression for what it is.

---

in the below ML means Maximum liklihood not machine learning

Sometimes the ML estimate happens to be "efficient" -- ie make the cramer rao bound tight -- ie be the MVU estimator.
note that you can generally find the ML estimator by finding which $x$ makes $\frac{d}{dx} \ln p(y;x) = 0$.

note: "ML estimate commutes with invertible maps".

---

**Sufficient statistics**
- We say $t(y)$ is  a sufficient statistic if $p(y\mid t; x_{1}) = p(y\mid t; x_{2})$ for any $x_{1},x_{2}.$
- equivalently, the condition is $L_y(x) \propto L_{t(y)}(x)$ where $L_y(x) = p(y;x)$

Neyman Factorization Theorem: $t$ is suffic statistic iff exists functions $a,b$ such that 
$$
p_y(y;x) = a(t(y),x)b(y).
$$

- Example: $y_{1},y_{2}$ iid dist as $N(x,1)$, then $\frac{y_{1}+y_{2}}{2}$ is suffic statistic.
- Example: $t(y)$ is suffic statistic for $p(y;x) = \exp(\lambda(x) \cdot t(y)-\alpha(x)+\beta(y)).$
- Example: if $x$ is in finite set then listing $p(y;x)$ for all possible $x$ is a suffic stat.

minimal suffic statistic: 
- a suffic stat $t$ is minimal, if for any suffic stat $s$, there exists a function $g$ such $g(s)=t$.

ex: likelihood ratio is minimal suffic stat

**Bayesian setting**

- $p(y|t,x) = p(y|t).$
- $p(x \mid y)=p(x\mid t)$
- $p(y\mid x) = p(t\mid x) p(y\mid t)$.
- $x \perp y \mid t$
- $t(y_{1})=t(y_{2})\implies L_{y_{1}}(x) \propto L_{y_{2}}(x).$

minimal suffic  statistic: 
- $L_{y_{1}}(x) \propto L_{y_{2}}(x) \implies t(y_{1})=t(y_{2}).$

If $Y$ is finite, clear that minimal suffic stat exists. 
Can find by normalizing the likelihood functions, and then sorting the likelihood functions into buckets based on which ones are the same


ppl don't know how to check whether a suffic stat is minimal efficiently.
But, 

a suffic stat is **complete** if for any $\phi:t(Y)\to \mathbb{R}$ with $\E[\phi(t(y))]=0$ for all $x$ has $\phi(t(y)) = 0$.
(not necc condition)

**Theorem**: Complete suffic stats are minimal.

- you can use this to show that $t(y)$ for exp fam is suffic stat

remark: 
- completeness is saying that $L_{t=1},\dots,L_{t=M}$ are all lin indep

- a suffic stat is minimal iff for each distinct $t_{1},t_{2}$ we have $L_{t_{1}},L_{t_{2}}$ are lin indep


[[EM reprise]]

----

**information geometry:**

im sick atm so these notes will not be high quality sorry about that 

it seemed like some pretty neat stuff though

- **linear family**: the set of distributions $p$ that satisfy $\E_{y\sim p} [t_i(y)] = \mu_i$ for some set of $t_i,\mu_i$
	- We write $L_t(p)$ to be the linear family of distributions which agree with $p$ on the mean things.
- **exponential family**: we've already defined these, but we'll specifically define
	- $E_t(p)$ as the set of distributions $q$ which can be written as 
$$
q(y)= p(y) \exp(x\cdot t(y) - \alpha(x) )
$$
for some parameters $x$.

Interestingly, these types of families turn out to be an "orthogonal basis" for the information geometry.

Define the I-projection as follows:
$$
\mathsf{argmin}_{p\in \mathcal{P}} D(p||q).
$$

It turns out that in general we have
$$
D(p||q) \ge D(p||p^{*})+D(p^{*}||q).
$$

And if $\mathcal{P}$ is linear family then this becomes an **equality**
and if you're interested in which things have I-projection onto $\mathcal{L}_t(p^{*})$  equal to $p^{*}$ it turns out that it's exactly $E_t(p^{*})$.

As a cherry on top, if you look at things locally the info geometry behaves like euclidean space.


# Networks 
- Measure "importance" of a vertex as "average importance of neighbors".
	- Can interpret these as steady-state probabilities if it's a Markov Chain.
- Can also give people some base importance.
	- This is like a Markov Chain but with some teleportation probability.

#todo -- figure out what else is happening in networks

