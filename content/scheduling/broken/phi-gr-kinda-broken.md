[[scheduling]]
see also: [[phi-cancel-width-zero]]
$$\newcommand{\pgr}{\phi\mathsf{gr}}$$
$$\newcommand{\C}{\mathsf{C}}$$
$$\newcommand{\opt}{\mathsf{OPT}}$$
$\pgr$ makes decisions as follows:
- run one task in parallel at a time, in order of increasing arrival time
- if ever $\sigma_i + t_j < \phi\C_{\opt_j}^j$, and task $\tau_i$ is not yet started, start $\tau_i$ in serial.

**Theorem**
**Assume:**
- width zero
- We run all of our tasks in parallel (this is actually wlog by the width zero assumption)
- we have at most two tasks present at any point in time (one running, one waiting)
- OPT runs all tasks except the last in serial
Then we are $\phi$-competitive.

**proof**:

$$\C_\opt \ge t_n+\pi_n/p.$$
$$\C_\pgr \le t_n + (\pi_{n-1}+\pi_n)/p$$
$$\C_\opt \ge\sigma_{n-1}+t_{n-1}\ge \phi\C_{\opt_{n-1}}^{n-1}\ge \phi(t_{n-1}+\pi_{n-1}/p)$$
Combining these equations gives the desired result.

TODO: remove all of these assumptions. Especially the assumption that there are only two relevant tasks.


# another attempt, more successful

**Theorem**
**Assume:**
- width zero
- $\opt$ runs $\tau_{n-1}$ serial, $\tau_n$ parallel.
- no OPT gaps.
- lemma 0
Then $\pgr$ is $\phi$ competitive.

>[!tip] Proof
> **Case 1: We haven't started $\tau_{n-1}$ yet by $t_n$.** 
>  $$\sigma_{n-1}+t_n>\phi \C_\opt.$$
$$\C_{\opt}+(t_n-t_{n-1})>\sigma_{n-1}+t_{n-1}+(t_n-t_{n-1})>\phi\C_{\opt}$$
$$\phi(t_n-t_{n-1})>\C_{\opt}.$$
$$\C_\pgr \le \C_{\pgr}^{n-1}+\pi_n \le \phi\C_{\opt_{n-1}}^{n-1}+\pi_n\le \C_\opt+\pi_n<\phi(t_n-t_{n-1})+\pi_n \le \phi (t_n + \pi_n) \le \phi \C_\opt.$$
> **Case 2: We start $\tau_{n-1}$ before time $t_n$.** 
> $$\C_{\pgr} \le t_n + \pi_n/p + \pi_{n-1}/p \le \C_{\opt}+\pi_{n-1}/p$$
> $$\C_\opt \ge \phi \C_{\opt_{n-1}}^{n-1}\ge \phi \pi_{n-1}/p.$$
> Combined give:
> $$\C_{\pgr} \le \phi \C_{\opt}.$$


Hoping for the following result:
**Assume:**
- width zero
- $\opt$ runs $\tau_{n-2}$ serial, $\tau_{n-1}, \tau_n$ parallel.
- no OPT gaps.
Then, $\pgr$ is $\phi$ competitive. 

**Case 1:** $\tau_{n-2}$ started in parallel before time $t_{n-1}$
It's great. :)

>[!error] idea
>$\pgr$ is really sad when it gets stuck running a large parallel task. 
>So, what if we had it run parallel tasks in order of increasing parallel work or something?
>This feels somewhat smarter potentially? Although a little more annoying to write down analysis  for.



----
# this proof is currently broken

**Theorem**
Assume:
- no $\opt$ gaps
- width zero
Then $\pgr$ is $\phi$-competitive.

**Lemma** Assume $\opt$ runs $\tau_{n-1}$ in serial. Then either $\pgr$ is $\phi$-competitive, or $\pgr$ runs $\tau_{n-1}$ in serial as well.
**pf**
Assume that $\pgr$ does not run $\tau_{i}$ in serial. We will show that $\pgr$ must be $\phi$-competitive.
First, not running $\tau_{n-1}$ in serial implies that at time $t_{n-1}$ the task was deemed too large:
$$\C_\opt \ge \sigma_{n-1} + t_{n-1} > \phi (\pi_{n-1}/p+t_{n-1}). $$
Suppose that we are locked in to running $\tau_{n-1}$ in parallel at time $t_n$. Then  we would have:
$$\C_{\pgr} \le t_{n} + \frac{\pi_{n-1}+\pi_n}{p}$$
And then we are done, because we have shown above that $\pi_{n-1}/p<\C_\opt/\phi$.
Now, suppose that we are not locked in to running $\tau_{n-1}$ in parallel at time $t_n$, but we still choose not to serialize it. Then we must have
$$\C_\opt > \sigma_{n-1}> \phi\C_\opt - t_n \implies t_n>\C_\opt/\phi.$$
$$\C_{\opt} \ge \sigma_{n-1}+t_{n-1} > \phi t_n.$$
Combined these equations say:
$$\C_\opt/\phi < t_n < \C_\opt/\phi,$$
Contradiction.

**TODO** feels very tantalizingly close to a full analysis.

# another broken thing
We will prove:
> [!info] **Theorem**:
$\pgr$ is $\phi$ competitive in the width zero setting.

This will follow immediately by induction on the following lemma:

> [!info] Lemma:
> We work in the zero width setting.
> Fix integer $n$. 
> Suppose that $\pgr$ is $\phi$ competitive on all TAPs of length smaller than $n$. 
> Let $\mathcal{T}$ be a length $n$ TAP, with no $\opt$ gaps.
> Suppose that $\opt$ serializes some task $\tau_i$ with $i<n$, and parallelizes all tasks after $\tau_i$.
> Suppose that $\pgr$ parallelizes all tasks.
> Then $\pgr$ is $\phi$ competitive on $\mathcal{T}$.

> [!tip] Proof:
> $$\phi\C_{\opt_i}^i \le \sigma_i + t_i \le \C_{\opt}.$$
> $$\C_{\pgr} \le \C_\pgr^i + \sum_{j>i}\pi_{j}/p \le \phi \C_{\opt_i}^i + \sum_{j>i}\pi_j/p\le \C_\opt + \sum_{j>i}\pi_j/p.$$
> So, it suffices to consider the case that:
> $$\sum_{j>i}\pi_j/p > \C_\opt/\phi.$$
> By assumption:
> $$\C_\opt \ge t_{i+1}+\sum_{j>i}\pi_j/p.$$
> Hence, 
> $$t_{i+1} \le \C_\opt/\phi^2. $$
> ---
> Now we look at time $t_{i+1}$ to bound our completion time. Let $\tau_j$ be the task that we are running in parallel right before time $t_{i+1}$. Then,
> $$\C_\pgr^i \le t_{i+1}+\pi_j + \C_{\opt_i}$$
> XXX this is super BROKEN

tbh the stuff in [[phi-gr-kinda-broken]] is like much more promising than this.