$$\newcommand{\phic}{\textsf{CAN}}$$
$$\newcommand{\C}{\mathsf{C}}$$
$$\newcommand{\opt}{\mathsf{OPT}}$$
$$\newcommand{\eps}{\varepsilon}$$
$$\newcommand{\par}{\parallel}$$
$$\newcommand{\ser}{\circledcirc}$$
[[scheduling]]
In this section we prove the following theorem.
**Theorem**
There is a $\phi$-competitive cancelling scheduler in the width-zero setting.

Our scheduler is $\phic$:
- run one task in parallel at a time, in order of increasing arrival time
- if we ever have $\sigma_i + t_j < \phi\C_{\opt_j}^j$ then run task $\tau_i$ in serial (which potentially requires cancelling $\tau_i$).

Without loss of generality (see [[scheduling]]) we assume that there are no $\opt$ gaps. 

Let $\widetilde{C}_\phic$ be the time when $\phic$ finishes its parallel tasks. 
Note that because $\phic$ only serializes tasks when it is "safe" to do so, in order to prove that $\C_\phic\le \phi\C_\opt$ it suffices to show that $\widetilde{\C}_\phic \le \C_\opt$.

**Lemma 0**: 
Without loss of generality we may consider only TAPs in which $\phic$ cannot serialize any task $\tau_i$ at time $t_i$. 
> **Proof**: Suppose there is a TAP $\mathcal{T}$ where we serialize $\tau_i$ at time $t_i$. Then we can make a "harder" TAP by removing task $\tau_i$. More precisely, if $\phic$ fails to be $\phi$-competitive on $\mathcal{T}$ then it also fails to be $\phi$-competitive on $\mathcal{T}'$, the TAP obtained by removing $\tau_i$ from $\mathcal{T}$.

**Lemma 1**
Without loss of generality there are no tasks $\tau_i$ that $\opt$ parallelizes, while $\phic$ serializes. 
> **Proof**: Suppose there is a TAP $\mathcal{T}$ where $\opt$ parallelizes $\tau_i$ but $\phic$ runs $\tau_i$ in serial (eventually, possibly after cancelling). Then we can make a "harder" TAP by inflating task $\tau_i$'s serial work to be $\infty$. More precisely, if $\phic$ fails to be $\phi$-competitive on $\mathcal{T}$ then it also fails to be $\phi$-competitive on $\mathcal{T}'$, the TAP obtained by removing $\tau_i$ from $\mathcal{T}$. We can see this because this inflation operation does not increase $\C_{\opt_i}^i$ for any $i$, but restricts $\phic$'s choices, which can only lead to $\widetilde{C}_\phic$ increasing. 

**Lemma 2**:
Without loss of generality $\opt$ serializes at least one task. 
> **proof**: $\phic$ is clearly $\phi$-competitive if $\opt$ runs all tasks in parallel.

These three lemmas allow us to isolate the difficult case, namely that the TAP is of a form not ruled out by the lemmas. We now analyze this difficult case in the following lemma.

**Lemma 3**
Let $\mathcal{T}$ be a TAP where
- (Lemma 0) $\phic$ never immediately serializes a task
- (Lemma 1) $\phic$ serializes a subset of the tasks that $\opt$ serializes
- (Lemma 2) $\opt$ serializes at least one task.
Then, $\phic$ is $\phi$-competitive on $\mathcal{T}$.
> [!tip] **proof**:

 Let $i$ be the last index such that $\opt$ runs $\tau_i$ in serial. By assumption, $\phic$ runs all tasks $\tau_j$ with $j>i$ in parallel (because $\opt$ parallelizes these tasks). 
 Then, 
 $$
 \widetilde{C}_\phic \le \widetilde{C}_\phic^{i}+\sum_{j>i}\pi_i/p.
 $$
 We know that $\opt$ runs $\tau_j$ in parallel for all $j>i$. Thus:
 $$
 \C_\opt \ge t_{i+1} + \sum_{j>i} \pi_i/p.
 $$
Using this in the earlier inequality gives:
 $$
 \widetilde{C}_\phic \le \widetilde{C}_\phic^{i} -t_{i+1} + \C_\opt.
 $$
Now, because $\phic$ is allowed to cancel we have:
$$
\widetilde{C}_\phic^i \le t_i + \C_{\opt_i}^i.
$$

 Because of Lemma 0 we may assume that at time $t_i$ $\phic$ thought $\tau_i$ was too large to serialize. 
 However, we are assuming that $\opt$ parallelizes $\tau_i$. Using these two facts together we have:
$$
\C_\opt \ge \sigma_i  + t_i \ge \phi  \C_{\opt_i}^i.
$$
Combined with our earlier bound we have:
$$
\widetilde{C}_\phic^i \le t_i + \C_\opt/\phi.
$$
Combined with our earlier analysis this shows:
$$
\widetilde{C}_\phic\le \phi\C_\opt + t_i-t_{i+1}\le \phi\C_\opt.
$$
#todo This last step feels a bit wasteful potentially? Maybe we can get more out of it. Like what I mean is that if $t_i \approx t_{i+1}$ then maybe we can do some different analysis. But this is good enough  for now.
[[1.5-cancel-thoughts]]
$\blacksquare$

---
This supercedes [[phi-cancel-broken]]
The lower bound in [[cancel-lower-bound]] is our best lower bound against improvement (it is not tight).
Hopefully this can be extended to [[phi-gr-kinda-broken]]