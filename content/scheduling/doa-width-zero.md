Assume "**width zero**" tasks.
*We will not need to assume that all tasks arrive at the start*.
The $\tgr$ algorithm operates as follows:
- If $\sigma_i+t_i > 2\C_{\opt_i}^i$ run task $i$ in parallel
- Otherwise run task $i$ in serial.

It is clear that tasks with $\sigma_i+t_i < 2\C_{\opt_i}^i$ are safe to run if our goal is to be $2$-competitive. Thus, these are not very interesting tasks. 

It's a little annoying to say why, but WLOG tasks like this do not exist.
Subject to this WLOG we run all tasks in parallel.

**lemma**: 
Our algorithm maintains the following invariant:
For all $i, \alg$
$$\C_\tgr^i \le \C_\alg^i + \K_\alg^i/p$$
**pf**
The base case is vacuously true.
Assume that the invariant holds for $i$, we prove it for step $i+1$.
Recall that we are assuming that the tasks are such that $\tgr$ parallelizes all tasks.
If $\alg$ also parallelizes task $i+1$ then $\C_\tgr$ and $\K_\alg/p$ both increase by $\pi_{i+1}/p$ and the invariant is maintained.
Now, consider the case that $\alg$ chooses to serialize task $i+1$.
Then, it must be the case that $\sigma_{i+1}$ is rather large. In particular we have:
$$\C_\alg^{i+1}\ge \sigma_{i+1}+t_{i+1} \ge 2\C_{\opt_{i+1}}^{i+1}\ge 2\C_{\opt_{i+1}}^{i}.$$
Thus, 
$$\C_\tgr^{i+1}= \pi_{i+1}/p + \C_\tgr^i\le \pi_{i+1}/p+\K_{\opt_{i+1}}^i/p + \C_{\opt_{i+1}}^i \le 2\C_{\opt_{i+1}}^{i+1}\le \C_{\alg}^{i+1}.$$
todo: extend beyond width zero...

$$\newcommand{tgr}{\mathsf{2GR}}$$
$$\newcommand{alg}{\mathsf{ALG}}$$
$$\newcommand{\bal}{\mathsf{BAL}}$$
$$\newcommand{\K}{\mathsf{K}}$$
$$\newcommand{\C}{\mathsf{C}}$$
$$\newcommand{\opt}{\mathsf{OPT}}$$