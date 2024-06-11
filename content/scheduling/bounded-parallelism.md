#todo: does [[cancel-lower-bound]] beat this?

 One natural way one might hope to overcome the $\phi$ lower bound
 is to consider tasks with at least a certain degree of
 parallelism. In particular, a natural constraint is
 \defn{$c$-parallel TAPs} where $\pi_i/p \leq \sigma_i / c$ for
 some $c>1$; before we took $c=1$. In other words, a task run on
 all processors simultaneously achieves speedup of at least $c$
 over the serial implementation. The counter example in
 \cref{prop:evilLowerBound} is invalid for $c>\phi$, i.e. required a very
 un-parallelizable task. Of course if $c \approx p$ then we can
 trivially get a very good approximation. However, the following
 proposition shows that more modest values of $c$ will not lead to
 substantial improvement.

 \begin{proposition}
   \label{prop:lowerboundC}
   Let $\alg$ be a deterministic scheduler, and let
   $c\in [1,p)$.
   There exists a $c$-parallel TAP $\mathcal{T}$ on which $\alg$ has
   competitive ratio at least $\max(\frac{1+\sqrt{3}}{2} - \Theta(c/p), \min(c, 1+ 1/c)).$ 
   \end{proposition}
 \begin{proof}
   The $\min(c,1+1/c)$ term is the generalization of
   \cref{prop:evilLowerBound} to $c$-parallelizable tasks, i.e.
   there is a task with $\sigma=\phi, \pi/p = \sigma/c$
   potentially followed by $p-1$ intrinsically serial tasks of size
   $\phi$. Note $\min(c,1+1/c)$ quickly looses power for $c>\phi$.

   In $\mathcal{T}_A$, $c(\sqrt{3}-1)$ tasks \footnote{We ignore the
   issue that this is not an integer; appropriate 
 rounding fixes this.} with $\sigma= \sqrt{3}+1$, $\pi/p =
 \frac{\sqrt{3}+1}{c}$ arrive at time $0$; note that these tasks are maximally
 un-parallelizable among $c$-parallelizable tasks.
 TAP $\mathcal{T}_B$ starts the same as $\mathcal{T}_A$, but in
 $\mathcal{T}_B$ $p-c(\sqrt{3}-1)$ additional maximally
 un-parallelizable tasks arrive at time $1$ with serial work $\sqrt{3}.$
 We claim that $\alg$ can do well on at most one of these TAPs.
 \case{$\alg$ schedules a task in serial}\\
 Here $\alg$ performs poorly on $\mathcal{T}_A$: $\opt$ achieves awake
 time 2 whereas we achieve awake time at least $\sqrt{3}+1$,
 giving a ratio of $\frac{\sqrt{3}+1}{2}$.

 \case{$\alg$ decides at time $t\geq 1$ to run at least one of the
 tasks from time $0$ in serial} \\
 Here $\alg$'s time is at least $\sqrt{3}+2$ while $\opt$ can achieve
 $\sqrt{3}+1$; this yields a ratio of
 $\frac{\sqrt{3}+2}{\sqrt{3}+1} = \frac{\sqrt{3}+1}{2}.$

 \case{$\alg$ runs all the tasks from time $0$ in parallel}\\
 Here $\alg$'s performance is still bad on $\mathcal{T}_A$: 
 we achieve time $\sqrt{3} + 2 - \frac{c(3-\sqrt{3})}{p}$ for a
 ratio of $\frac{\sqrt{3}+1}{2} - \Theta(c/p)$.
 
\end{proof}