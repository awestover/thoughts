$$\newcommand{\bal}{\mathsf{BAL}}$$
$$\newcommand{\K}{\mathsf{K}}$$
$$\newcommand{\C}{\mathsf{C}}$$
$$\newcommand{\opt}{\mathsf{OPT}}$$
$$\newcommand{\alg}{\mathsf{ALG}}$$
$$\newcommand{\balp}{{\mathsf{BAL}^\star}}$$
Let $\C^i_\alg$ denote the completion time of $\alg$ on the first $i$ tasks, and let $\K_\alg^i$ denote the work taken by $\alg$ on the first $i$ tasks.
A technical note: we assume $p>1$; otherwise the problem is not interesting. 

**Theorem:** There is a $2.5$ competitive decide on arrival scheduler for completion time. 
### Proof Setup
Our scheduler is called $\balp$. $\balp$ operates in three **modes**:
- optser-jagged mode
- optpar-jagged mode
- balanced mode

The mode of $\balp$ is determined as follows:
- If $\balp$ is balanced then it is in *balanced* mode.
- If $\balp$ is jagged, and $j$ is the latest task that arrived for which  $\C_\balp^i = \sigma_j +t_j$, then 
	- If $\opt$ ran task $j$ in serial $\balp$ is in optser-jagged mode. 
	- If $\opt$ ran task $j$ in parallel then $\balp$ is in optpar-jagged mode. 
Note that $\balp$ generally can't distinguish between being in optser-jagged mode and optpar-jagged mode; the distinction between these modes is important only as an analysis tool. 

We say that task $i$ is **bad** if it has:
$$\pi_i/p > \sigma_i/2.$$
$\balp$ makes its decisions as follows:
- Always run bad tasks in parallel. 
- When jagged, parallelize all good tasks.
- When balanced, if $\sigma_i+t_i < \C_{\balp}^{i-1}$ run task $i$ in serial; otherwise run task $i$ in parallel.

We claim that $\balp$ maintains the following invariants:
- When in optser-jag mode, $$\C_\balp^i \le \C_\opt^i.$$
- When in optpar-jag mode, $$\C_\balp^i \le 2\K_\opt^i/p.$$
- When in balanced mode, $$\C_\balp^i \le 1.5\C_\opt^i+\K_\opt^i/p.$$
Note that if we could prove that $\balp$ maintains all of these invariants at all times then it could clearly imply that $\balp$ is $2.5$ competitive.

### Actual Analysis: 
Now we prove that $\balp$ maintains these invariants at all times.

#### Base cases
First we consider the "base cases" --- that is, what happens when we transition from one mode to another. 
**Entering optser-jagged mode**:
When we enter optser-jagged mode we have 
$$\C_\balp^i = t_i+\sigma_i \le \C_\opt^i.$$
**Entering optpar-jagged mode**:
When we enter optpar-jagged mode we have 
$$\C_\balp^i = t_i+\sigma_i\le t_i + 2\pi_i/p \le 2\C_\opt^i.$$
**Entering balanced mode:**
This case is a bit more complex.
First, let's assume that $\balp$ enters balanced mode on a good task. This is the main case, it is just a silly technicality that we need to consider the other case. 
We will consider four ways to enter balanced mode, based on our previous mode and based on what decision $\opt$ makes on task $i$.
- *Coming from optser-jagged mode, $\opt$ chose to run task $i$ in serial*.
$$\C_{\balp}^i \le \C_{\opt}^i+\pi_i/p \le 1.5\C_\opt^i.$$
- *Coming from optser-jagged mode, $\opt$ chose to run task $i$ in parallel*.
$$\C_{us}^i \le \C_{\opt}^i+\pi_i/p \le \C_\opt^i+\K_\opt^i/p.$$
- *Coming from optpar-jagged mode, $\opt$ chose to run task $i$ in serial*.
$$\C_{\balp}^i \le 2\K_{\opt}^{i-1}/p+\pi_i/p \le 1.5\C_\opt^i+\K_\opt^i/p.$$
**(Note: this is the case that bottlenecks the analysis, causing us to get $2.5$ instead of $2.42$)**
- *Coming from optpar-jagged mode, $\opt$ chose to run task $i$ in parallel*.
$$\C_{\balp}^i \le 2\K_{\opt}^{i-1}/p+\pi_i/p \le \C_\opt^i+\K_\opt^i/p.$$

Now we must consider the aforementioned silly technicality: $\balp$ enters balanced mode due to a bad task. We again consider cases.
- *Coming from optser-mode*. 
$$\C\balp^i \le \C_\opt^{i-1}+\sigma_i/p \le \C_\opt^i + \K_\opt^i/p.$$
- *Coming from optpar-mode*. 
$$\C\balp^i \le 2\K_\opt^{i-1}/p+\sigma_i/p \le (1+2/p)\C_\opt^i +\K_\opt^i/p \le 1.5\C_\opt^i + \K_\opt^i/p.$$

#### Inductive steps
Now we show that when tasks arrive but don't change which mode we are in, the invariants are preserved. 

**optser-jagged mode**:
We continue to be bottlenecked by this serial task, $\opt$'s completion time cannot decrease. 

**optpar-jagged mode**:
We continue to be bottlenecked by this serial task, $\opt$'s work taken cannot decrease.

**Balanced mode**:
If $\balp$ takes the serial task, or $\balp,\opt$ both take the parallel task then $\K$ increases by the same amount for both $\balp, \opt$ so the invariant is maintained. 
Now, suppose that $\balp$ runs a task in parallel while $\opt$ runs the task in serial. 
In that case we have
$$\C_{\opt}^i \ge \sigma_i+t_i \ge \C_{\balp}^{i-1}$$
and (crucially) because the task must be good for us to parallelize it, 
$$\pi_i/p \le \sigma_i/2.$$
Combining the inequalities we have
$$\C_\balp^i \le \C_\bal^{i-1}+\pi_i/p \le 1.5\C_\opt.$$


---

**TODO**: Extend to awake time --- should be fairly straightforword.

