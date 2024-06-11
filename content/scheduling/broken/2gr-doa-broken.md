$$\newcommand{tgr}{\mathsf{2GR}}$$
$$\newcommand{alg}{\mathsf{ALG}}$$
$$\newcommand{\bal}{\mathsf{BAL}}$$
$$\newcommand{\K}{\mathsf{K}}$$
$$\newcommand{\C}{\mathsf{C}}$$
$$\newcommand{\opt}{\mathsf{OPT}}$$

# NOPE THIS IS BROKEN
The $\tgr$ algorithm operates as follows:
- If $\sigma_i+t_i > 2\C_{\opt_i}^i$ run task $i$ in parallel
- Otherwise run task $i$ in serial.

We remark that we currently don't have an efficient algorithm for computing this, but we do have a PTAS. 

**lemma**: 
Our algorithm maintains the following invariant:
For all $i, \alg$
$$\C_\tgr^i \le \C_\alg^i + \K_\alg^i/p$$
**pf**
The base case is vacuously true.
Assume that the invariant holds for $i$, we prove it for step $i+1$.

**case 1: ALG serial, 2GR serial:**
There are two sub-cases here. 
First, we could have $\C_{\tgr}^{i+1} = \sigma_{i+1}+t_{i+1}$.
This is super great, because we know $\C_{\alg}^{i+1}\ge \sigma_{i+1}+t_{i+1}$.
Otherwise, we have that $\tau_{i+1}$ is not bottle-necking $\tgr$.
If we end unsaturated then we actually just have $\C_\alg^{i+1} = \C_\alg^i$;
this is not a case we need to worry about.
If instead we are saturated from now till the end, then we will have 
$$\C_{\tgr}^{i+1} \le \sigma_{i+1}/p + \C_{\tgr}^i \le \C_\alg^i +\K_{\alg}^i/p+\sigma_{i+1}/p \le \C_\alg^{i+1} +\K_{\alg}^{i+1}/p.$$
**case 2: ALG parallel, 2GR parallel:**
$$\C_{\tgr}^{i+1} \le \C_\tgr^i + \pi_{i+1}/p \le \C_\alg^i +\K_{\alg}^i/p+\pi_{i+1}/p \le \C_\alg^{i+1} +\K_{\alg}^{i+1}/p.$$
**case 3: ALG serial, 2GR parallel:**
In this case, $\tgr$  thought that the serial job looked rather large. In particular we have
$$\C_\alg^{i+1}\ge \sigma_{i+1}+t_{i+1} \ge 2\C_{\opt_{i+1}}^{i+1}\ge 2\C_{\opt_{i+1}}^{i}.$$
Thus, 
$$\C_\tgr^{i+1}\le \pi_{i+1}/p + \C_\tgr^i\le \pi_{i+1}/p+\K_{\opt_{i+1}}^i/p + \C_{\opt_{i+1}}^i \le 2\C_{\opt_{i+1}}^{i+1}\le \C_{\alg}^{i+1}.$$

**case 4: ALG parallel, 2GR serial:**
This case is the most challenging. 

We have the following facts:
$$\C_{\tgr}^{i+1} \le \max( \C_\tgr^i+\sigma_{i+1}/p, \sigma_{i+1}+t_{i+1})$$
$$\frac{\sigma_{i+1}+t_{i+1}}{2} \le \C_{\opt_{i+1}}^{i+1}.$$
$$\C_\alg^{i+1}\ge \max(\C_{\alg}^i, \C_{\opt_{i+1}}^{i+1}) \ge \max\left(\C_{\alg}^i, \frac{\sigma_{i+1}+t_{i+1}}{2}\right).$$
We would like to show that the following quantity is non-negative:
$$\star = \C_{\alg}^{i+1}+\K_{\alg}^{i+1}/p - \C_\tgr^{i+1}.$$
Using our above inequalities we have:
$$\star \ge \max\left(\C_{\alg}^i, \frac{\sigma_{i+1}+t_{i+1}}{2}\right) + \K_\alg^{i+1}/p - \max(\C_{\tgr}^i+\sigma_{i+1}/p, \sigma_{i+1}+t_{i+1}).$$

## ok maybe you can say something like if we get this branch of the max then XXX?

### I don't think anything good is going to happen here...


%% ## lol a potential replacement algorithm, it's probably really bad:

$$\sigma_i+t_i \le 2\K_{\opt_{i}}^{i}/p \implies \text{serial, else parallel }$$
nah this is very bad.  %%


**todo: make sure this generalizes to awake time. I'm pretty sure we just do the standard pause and run in background story** 



### new idea
$$\sigma+t \le \C_\opt + \K_\opt/p$$
TRY IT

$$\sigma+t \le \C_\alg + \K_\alg/p \quad \quad \forall \alg$$