$$\newcommand{\B}{\mathsf{B}}$$
$$\newcommand{\alg}{\mathsf{ALG}}$$
$$\newcommand{\opt}{\mathsf{OPT}}$$
$$\newcommand{\tgr}{\mathsf{TGR}}$$
$$\newcommand{\K}{\mathsf{K}}$$
$$\newcommand{\C}{\mathsf{C}}$$
[[scheduling]]
**remark**: N conjectures that this same analysis also works if you receive the tasks in an arbitrary order (i.e., you don' t receive them at the times when you are allowed to start them, you receive them in some other order). I haven't fact checked this.

**Theorem**:
There is a $2$-competitive decide on arrival scheduler for completion time.
**Proof of theorem**
The scheduler is $\tgr$. It makes decisions as follows:
- If $\sigma_i+t_i > 2\C_{\opt_i}^i$ run $\tau_i$ in parallel.
- Otherwise, run $\tau_i$ in serial.
$\tgr$ schedules the tasks by allocating processors to the serial tasks with the most remaining work first, and then allocating processors to parallel jobs if there are any left-over processors.
We say that $\tgr$ is **saturated** at time $t$ if $\tgr$ has no idle processors at time $t$.

First, observe that if $\tgr$ is unsaturated right before finishing, then its completion time was determined by a serial task, and thus $\tgr$ is $2$-competitive on this TAP. Now consider the case that $\tgr$ is saturated right before finishing.
Let $t_*$ be the final time when $\tgr$ is unsaturated (we may have $t_*  = 0$).

Let $\widetilde{\K}_\alg^i$ denote the work that $\alg$ takes on the first $i$ tasks that arrive after time $t_*$.
**Lemma**: $\tgr$ maintains the following invariant for all $i\ge 0$, and all schedulers $\alg$:
$$
\widetilde{\K}_\tgr^i \le (C^i_\alg-t_*)p+\widetilde{\K}^i_{\alg}.
$$
**Proof of lemma**
The base case $i=0$ is true because $\widetilde{\K}_\tgr^0 = 0$, and $\C_\alg^0 \ge t_*$.
If $\alg$ takes the same or more work than $\tgr$ on a task then the invariant is clearly maintained. 
The only remaining case is that $\alg$ runs task $i$ in serial while $\tgr$ runs task $i$ in parallel.
In this case $\sigma_i$  must be quite large:
$$\C_{\alg}^i \ge \sigma_i + t_i > 2\C_{\opt_i}^i.$$
Thus, 
$$\widetilde{\K}_\tgr^i \le \widetilde{\K}_\tgr^{i-1}+\pi_i/p\le (C^{i-1}_{\opt_i}-t_*)p+\widetilde{\K}^{i-1}_{\opt_i}+\pi_i/p.$$
$\opt_i$ must have run the task in parallel so we have
$$
\widetilde{\K}_\tgr^i \le (C^{i}_{\opt_i}-t_*)p+\widetilde{\K}^{i}_{\opt_i}\le 2(C^{i}_{\opt_i}-t_*)p\le 2(\C_\alg^i/2 - t_*)p\le (\C_\alg^i-t_*)p.
$$
$\square$

Now we use this lemma to establish the theorem.
For any scheduler $\alg$, let $\B_\alg$ denote the work that $\alg$ has left at time $t_*$.
We can upper-bound $\C_\tgr$ using the lemma as follows:
$$
\C_\tgr = t_*+\frac{\widetilde{\K}_\tgr+\B_\tgr}{p} \le \C_\opt+ \frac{\widetilde{\K}_{\opt}+\B_\tgr}{p}.
$$
We can lower-bound $\C_\opt$ as follows:
$$
\C_\opt \ge t_* + \frac{\B_\opt + \widetilde{K}_\opt}{p}.
$$
Combining the inequalities gives:
$$
\C_\tgr \le 2\C_{\opt}-t_* + \frac{\B_\tgr-\B_\opt}{p}.
$$
Thus, to show that $\tgr$ is $2$-competitive, it is sufficient to show that
$$
\B_\tgr \le \B_\opt + pt_*.
$$
This is quite easy to establish. 
Let $S$ denote the set of tasks that $\tgr$ has present at time $t_*$; obviously $|S|<p$.
Then, 
$$
\B_\tgr = \sum_{i\in S} \sigma_i - (t_*-t_i).
$$
On the other hand, 
$$
\B_\opt \ge \sum_{i\in S}\min(\sigma_i - (t_*-t_i), \pi_i-p\cdot(t_*-t_i)).
$$
Thus, 
$$
\B_\opt+pt_* \ge \sum_{i\in S}\min(\sigma_i - (t_*-t_i), \pi_i) \ge \B_\tgr,
$$
as desired.


This supercedes [[doa-2.5]], [[doa-broken-2.42]], [[doa-width-zero]], [[2gr-doa-broken]]
