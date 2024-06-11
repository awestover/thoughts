$\newcommand{\oracle}{\mathsf{ORACLE}}$
$\newcommand{\bat}{\mathsf{BAT}}$
$\newcommand{\opt}{\mathsf{OPT}}$
$\newcommand{\T}{\mathsf{T}}$

In fact, using a somewhat similar method we can get arbitrarily
close to a $2$ approximation with a very simple scheduler; note
that this scheduler is fundamentally invalid for the decide on
arrival time model. We prove a slightly more general result:

>[!info] Theorem
> There is a transformation that turns a scheduler
> $\oracle_R$ which is $R$ competitive on single-arrival-time TAPs,
> into $\bat_R$ which is $2R$ competitive on general TAPs.


>[!tip] Proof:
> $\bat_R$ operates as follows: whenever there are alive tasks but no currently running tasks, schedule a batch consisting of all alive tasks according to $\oracle_R$. Consider a TAP which causes $\bat_R$ to schedule batches at times $t_0,t_1, \ldots, t_n, t_{n+1}$. Without loss of generality we consider a TAP such that there are unfinished tasks at all times in $[t_0, t_{n+1}]$; any TAP can be split up into disjoint intervals where $\bat_R$ has alive tasks, and if $\bat_R$ is competitive on one of these intervals it is certainly competitive on their union. The awake times for $\opt$ (times when $\opt$ has alive tasks) will be a collection of sub-intervals of this time range. Let $\T_{\opt}(t_{i},t_{i+1})$ denote the awake time of $\opt$ on the interval $[t_i, t_{i+1}]$. We see that for $i\geq 1, t_{i+1}-t_i \leq R\cdot \T_{\opt}(t_{i-1},t_i)$. Intuitively, by waiting until a substantial amount of work arrives and then scheduling it all at once we avoid the possibility of making a decision that is bad because of a task that arrives when executing our decision. Also, $t_1-t_0 \leq R\cdot \T_{\opt}(t_0, t_1)$. \
> Adding this all together we achieve the desired result: $$t_{n+1}-t_0 \leq 2R\cdot \T_{\opt}(t_0,t_n). $$

By batching our offline single-arrival time scheduler from [[offline]] we get:

>[!info] Corollary
> There is a $2$-competitive online scheduler with run time $O(n\log n)$.
