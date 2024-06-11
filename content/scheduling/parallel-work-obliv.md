[[scheduling]]
$$\newcommand{\til}{\mathsf{til}}$$
$$\newcommand{\T}{\mathsf{T}}$$
\subsection{Oblivious to Parallel Work}
Remarkably, we do not need to know the values $\pi_i$ to create a competitive schedule.
\begin{theorem}
  There is an $O(n\log n)$ time scheduler that is $2$ competitive
  for single arrival time TAPs without knowledge of parallel works.
\end{theorem}
\begin{proof}
  We call our algorithm $\til$. $\til$ starts by sorting the tasks so that $\sigma_1\geq \sigma_2\geq \cdots \geq \sigma_n$. $\til$ runs the largest task parallel until
  $$\frac{t+\sigma_{i_*+1}}{t+\sum_{i>i_*}\sigma_i/p} = 2,$$
  and then schedules the remainder of the tasks in serial (these tasks are managed via most-work-first). Let $t_0$ be the time that $\til$ starts running serial tasks, and let $t' = \sum_{i\leq i_*}\pi_i/p$.
  $\opt$ must satisfy one of the following conditions: \case{$\opt$ schedules one of the $i_*$ largest tasks in serial: \label{case2oblivA}}
  $$\T_{\opt}\geq \sigma_{i_*}.$$ 
  \case{$\opt$ schedules all of the $i_*$ largest tasks in parallel: \label{case2oblivB}}
  $$\T_{\opt}\geq t'+ \sum_{i>i_*}\sigma_i/p.$$
  We also note that $$\sigma_{i_*+1} = t_0 + 2\sum_{i>i_*}\sigma_i/p,$$
  due to \eqref{eq:tilswitch} and trivially $\T_{\opt} \geq \sum_i  \sigma_i /p.$

  Now we analyze the performance of $\til$ by considering four
  cases.
  \case{$\til$ ends jagged.} Then $\T_{\til} \leq t+\sigma_{i_*+1}$.\\
  In \cref{case2oblivA} we have:
  $$\T_{\til}\leq t+\sigma_{i_*+1} \leq 2\sigma_{i_*} \leq 2\T_{\opt},$$
  while in \cref{case2oblivB} we have:
  $$\T_{\til}\leq t+\sigma_{i_*+1} \leq 2\left(t+\sum_{i>i_*}\sigma_i/p\right)
  \leq 2\T_{\opt}.$$

  \case{$\til$ ends balanced.} Then $$\T_{\til}\leq
  t'+\sum_{i>i_*}\sigma_i/p.$$
  For \cref{case2oblivA} we have:
  $$\T_{\til}\leq t_0 + \pi_{i_*}/p + \sum_{i>i_*}\sigma_i/p \leq
  \T_{\opt} + \sigma_{i_*}\leq 2\T_{\opt},$$
  while for \cref{case2oblivB} we have:
  $$\T_{\til}\leq t'+\sum_{i>i_*}\sigma_i/p\leq \T_{\opt}.$$
\end{proof}
Applying \cref{prop:batch} [[offline]] (batching) to \cref{thm:oblivpll2} we get:
\begin{corollary}
  There exists a $4$ competitive online scheduler without knowledge of parallel works with running time $O(n\log n)$.
\end{corollary}

