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


bregman divergence:

$B_f(w||v) =$ how far off is the first order taylor expansion of $f$ around $v$ from the true answer at $w$?
i.e., $f(w) - (f(v)+ (w-v)^{\top}\nabla f(v))$.

Entropy is defined as $\log K - D_{KL}(p, u)$.



