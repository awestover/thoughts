Theoretical ML textbook

### 5 info theory

KL(p||q) is the expected weight of evidence for p over q, given true distribution $p$

weight of evidence: $\log (p(x)/q(x))$

A weird form of KL divergence that I don't get yet:

$$
KL(P||Q) = \sup_\phi \mathbb{E}_{x\sim P} \phi(x) - \log \mathbb{E}_{x\sim Q}[\exp(\phi(x)].
$$


data processing for KL:
KL decreases if you process rvs

Fisher information matrix:
$$
F = \mathbb{E}[(\nabla \log p_\theta(x))(\nabla \log p_\theta(x))^{\top}]
$$
KL divergence between $p_\theta, p_{\theta+\delta}$ is approximately $.5 \delta ^{\top}F\delta$ by a taylor expansion.


Bregman divergence:

$B_f(w||v) =$ how far off is the first order Taylor expansion of $f$ around $v$ from the true answer at $w$?
i.e., $f(w) - (f(v)+ (w-v)^{\top}\nabla f(v))$.

Entropy is defined as $\log K - D_{KL}(p, u)$.

## 7.5.3 EM 

I found the books exposition extremely confusing. 
The below discussion will clarify what's going on a lot!

**Setup**

We are going to realize some phenomenon several times. 
Let's suppose the phenomenon is $(y,z)$ where $y$ is the temperature, and $z$ is Alek's happiness level.
Let's realize this for $N$ days, generating $(y_i,z_i)$ pairs. 
Let's suppose we just observe $y_i$ and not $z_i$.
We're trying to fit a model $p_\theta$ to explain the data.
Our loss function is 
$$ \ell(\theta)= \sum_{i} \log p_\theta(y_i) = \sum_i \log \int_{z_i} p_\theta(y_i,z_i).  $$ 

Suppose you wanted to optimize this function.
It might be hard because the integral could be intractable.

So, we are going to have a fancy technique for computing this.

First, we need to define the ELBO:

$$
L_\theta(q,y) = \mathbb{E}_{z \sim q} \log \frac{p_\theta(y,z)}{q(z)}.
$$
It's immediate by Jenson's inequality that
$$ \mathbb{E}_{z \sim q} \log \frac{p_\theta(y,z)}{q(z)} \le \log p_\theta(y). $$ 
 And it's also clear that setting $q^* = p_\theta(z\mid y)$
$$ L_\theta(q^{*},y) = p_\theta(y). $$ 

We also define 
$$ L_\theta(\set{q_i},\set{y_i})  = \sum_i L_\theta(q_i,y_i). $$ 

ok, so why do we care?

It's going to give us this "EM" algorithm. Which apparently is pretty good. Although I'm not sure yet what it's theoretical guarantees are in terms of how fast it converges. 

**E step**
- Choose $q^*  = \mathsf{argmax}_q L_\theta(q)$

**M step** 
- Choose $\theta^{*} = \mathsf{argmax_\theta} L_\theta(q)$.

If $p_\theta$ satisfies some conditions then these are both tractable problems -- whereas the original problem might not have been tractable.


**EXAMPLE**

Suppose $x_{1},\dots,x_n$ are sampled iid from $\mathcal{N}(\mu,\Sigma)$. 
The MLE estimate of the $\mu,\Sigma$ should be the sample mean + covar.

Now suppose we have **missing data**.
#todo


### VI

## VAEs

