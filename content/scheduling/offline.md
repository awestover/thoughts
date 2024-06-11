[[scheduling]]
$$\newcommand{\eps}{\varepsilon}$$
$$\newcommand{\opt}{\mathsf{OPT}}$$
$$\newcommand{\apr}{\mathsf{APR}}$$
$$\newcommand{\poly}{\mathsf{poly}}$$
$$\newcommand{\T}{\mathsf{T}}$$
$$\newcommand{\C}{\mathsf{C}}$$
$$\newcommand{\bigO}{\mathcal{O}}$$
---

> [!tip] width zero model
> Sort the tasks by serial completion time. 
> Serialize the earliest ending tasks until the parallel work drops below the serial end time limit.

---
\subsection{Offline}
First we consider the offline problem where all parameters of
the tasks are known ahead of time.
Schedulers for solving the offline problem besides being of
independent interest can also be used as subroutines in online
schedulers. We pick an optimal offline strategy and refer to
it as $\opt$.

Computing an exact solution can be done trivially via brute force
simulating every possible assignment of tasks to parallel or
serial. Note that deciding whether to run a task in serial or
parallel is all that must be done: without loss of generality
$\opt$ runs the serial jobs via \defn{most-work-first}, i.e. runs
up to $p$ of the serial jobs with the most remaining work at each
time slice, and runs the parallel jobs on any remaining
processors. 

First we give a scheduler for an important special case
\defn{single-arrival-time} TAPs, i.e. TAPs where 
all tasks arrive at the beginning.
\begin{proposition}
  \label{prop:singletime}
  There exists an optimal scheduler for single-arrival-time TAPs
  $\mathcal{T}$ with running time $O(n\lg n)$.
\end{proposition}
\begin{proof}
  We begin by sorting the tasks based on their serial works.
  Then, we compute in linear time serial work prefix sums
  $\sum_{i\leq k} \sigma_i$ and parallel work suffix sums
  $\sum_{i> k} \pi_i$ for all $k \in [n]$.
  Finally we compute in $O(n)$ time $$\min_{k\in [n]}
  \max\left(\sum_{i\leq k} \sigma_i/p + \sum_{i>k}\pi_i / p, \sigma_k\right),$$
  which is the minimum achievable awake time on this input TAP (achievable at the value of $k$ yielding the optimum). 
\end{proof}

Finding an exact solution to the full offline scheduling problem seems computationally intractable, so we focus on approximating it; in particular we give a polynomial approximation scheme.
\begin{proposition}
  For any $\eps > 0$, there exists an offline $1+\eps$ competitive scheduler with running time $\poly((pn/\eps^2)^{1/\eps})$ on TAP $\mathcal{T}$.
\end{proposition}
\begin{proof}
  We call our approximation algorithm $\apr$. We loop over each contiguous subset of the tasks $\mathcal{T}_{i}^{j}$ and determine the earliest time that these tasks can be completed (potentially with a $1+\eps$ factor error). $\opt$ will have gaps (times with no tasks) in between some contiguous subsets of tasks. Let $[a_1,b_1], [a_2,b_2],\ldots$ be such that $\opt$ has a gap between $b_i, a_{i+1}$ but no gap between any tasks in $[a_i,b_i]$. Let $\C_{\opt}^{\mathcal{T}_a^b}$ denote the time to complete tasks $\mathcal{T}_a^b$ (where we start counting time from $t_a$). Then we have:
  $$\T_{\opt}^{\mathcal{T}_1^n} = \sum_i
  \C_{\opt}^{\mathcal{T}_{a_i}^{b_i}}.$$
  Using our $1+\eps$ approximations to $\C_{\opt}^{\mathcal{T}_a^b}$ which we denote by $\C_{\apr}^{\mathcal{T}_a^b}$ we can achieve a schedule with awake time at most
  $$\sum_i (1+\eps)\C_{\opt}^{\mathcal{T}_{a_i}^{b_i}} \leq
  (1+\eps)\T_{\opt}^{\mathcal{T}_1^n}.$$
  In particular, once we have computed all values $\C_{\apr}^{\mathcal{T}_a^b}$ we use dynamic programming to compute a partition $[a_1',b_1'],[a_2',b_2'],\ldots$  of the tasks that optimizes 
  $$\sum_i \C_{\apr}^{\mathcal{T}_{a_i'}^{b_i'}}.$$
  Because $a_i'=a_i,b_i'=b_i$ is one such choice of partition this will certainly give us a $1+\eps$ approximation. It remains to describe how to compute 
  $$\C_{\apr}^{\mathcal{T}_{a}^{b}} \leq
  (1+\eps)\C_{\opt}^{\mathcal{T}_{a}^{b}}.$$
  For notational convenience in the following description we use $n$ instead of $b-a$ to denote the number of tasks in our subset of tasks.

  We reduce the optimization problem into a decision problem: repeatedly deciding whether or not the tasks can all be completed in time $\C$. This is possible because we can give a multiplicative range of size $p$ on the value of $\C$: let $\C_{\max}$ be the completion time if all tasks were run via parallel implementations, and $\C_{\opt}$ be the optimal completion time. Then
  $$\C_{\max}/p \leq \C_{\opt} \leq \C_{\max}.$$
  Now we demonstrate a procedure that if ${\C>(1+\eps)\C_{\opt}}$ states that finishing in time $\C$ is possible, and if $\C<\C_{\opt}$ outputs that finishing in time $\C$ is impossible. Using this procedure we can binary search to find a power of $(1+\eps)$ such that $(1+\eps)^i \leq \C_{\opt} \leq (1+\eps)^{i+1}.$ In particular, we must try $\log_{1+\eps}(p)\leq \bigO(\eps\lg p)$ values of $\C$ to get this $1+\eps$ approximation.

  Fix $\C$. Define the \defn{boundary times} to be $\C\eps \cdot i$ for $i\in 0,1,\ldots, 1/\eps$. We round the arrival times of tasks up to the nearest boundary time. We round the serial times which are at least $\eps \C/n$ large up to the nearest multiple of $\C\eps^2/n.$ We deal with all of the serial times less than $\eps \C / n$ in serial at the end, and so treat our new TAP as not having any of these incredibly small tasks. We claim that this rounding does not increase the optimum completion time by more than a factor of $(1+3\eps)$. Delaying all the tasks by $\C\eps$, multiplying all task sizes by $1+\eps$ and then adding $n \C\eps /n$ work at the end would increase the time by at most a factor of $1+3\eps$. This is an upper bound on how much we have increased the optimum work. By using $\eps' = \eps/3$ instead, we can ensure that the resulting TAP has optimum completion time at most a $1+\eps$ factor larger than before the rounding.

  Now we show how to exactly compute the optimum completion time of the rounded TAP via dynamic programming; this will complete the proof. We can represent a schedule of a prefix of the jobs by listing the amount of serial work present at each boundary time and listing the amount of parallel work present at the first boundary. Because of how the tasks sizes are rounded we require $p\cdot n/\eps^2$ possible values of serial work at each boundary. Thus, our DP table has size $(p\cdot n/\eps^2)^{1/\eps}.$  In each cell of the DP table we store whether or not this cell is possible, and if so, what the minimum parallel work is at the boundary time corresponding to the column in the DP for this serial arrangement. To fill the DP table we loop over the boundaries, and note the consequences of putting each task in serial or parallel via the DP.

  Overall the run time is $\poly((p\cdot n/\eps^2)^{1/\eps}).$
\end{proof}
