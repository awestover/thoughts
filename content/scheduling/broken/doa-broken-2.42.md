$$\newcommand{\bal}{\mathsf{BAL}}$$
$$\newcommand{\K}{\mathsf{K}}$$
$$\newcommand{\C}{\mathsf{C}}$$
$$\newcommand{\opt}{\mathsf{OPT}}$$
$$\newcommand{\wei}{\mathsf{WEI}}$$

-----
# towards a patch of this broken thing

Consider the following "promise" version of the bad tasks thing:

You have tasks, and all tasks either have $\pi/p < \sigma/2$ or $\pi = p \sigma$.
**Theorem** There is a $2.9$-competitive scheduler for "promise bad TAPs".
Algorithm for being $2.9$-competitive is as follows:
- always serialize bad tasks
- if you are jagged then run all (good) tasks in parallel
- if you are balanced, then make decisions like BAL would.
	- That is, serialize stuff if it fits under your waterline, parallelize otherwise.

Some observations: 
Whenever we are jagged it is because of a bad task, and we actually have
$$\C_{us}^i = \C_{\opt}^i.$$
While we are balanced we maintain the following invariant (assuming that we started with it):
$$\C_{us}^i \le \K_{\opt}^i/p + 1.5\C_{\opt}^i.$$
To see why we maintain this, observe that it's completely obviously maintained except in the case that we run a task in parallel while $\opt$ runs the task in serial. 
But, in that case we have
$$\C_{\opt} \ge \sigma+t \ge \C_{us}$$
and (crucially) because the task must be good for us to parallelize it, 
$$\pi/p \le \sigma/2.$$
So the only thing left to think about is the following question: 
> Suppose we are jagged, and then we become not jagged. Do we get the invariant $\C_{us}^i \le \K_{\opt}^i/p + 1.5\C_{\opt}^i$?

The answer is yes. To see why, we consider some cases. 

Case 1: opt runs task $i$ in serial. Then, 
$$\C_{us}^i \le \C_{\opt}^i+\pi_i/p \le 1.5\C_\opt^i.$$
Case 2: opt runs task $i$ in parallel. Then, 
$$\C_{us}^i \le \C_{\opt}^i+\pi_i/p \le \C_\opt^i+\K_\opt^i/p.$$
Either way, it's good.

-----------

# STILL BROKEN
# take 2


Suppose I have more than $\sqrt{p}$ bad tasks that are above water line. This seems like it'd be really really good. 
Otherwise, the couple of bad tasks that are present don't really mess with the waterline, so we can really say that adding a serial task doesn't bump the waterline too much.

No this is not true. the thing that is true is that OPT basically has to handle most (large) bad tasks the same way that we do. but that's not quite the same thing.


-----------------------
# the below stuff is broken but seems fixable


**Theorem**: 
There exists a $(\sqrt{2}+1)$-competitive decide on arrival scheduler for completion time. 
And it's even efficient!
**proof:**

Call a task **bad** if $\pi/p > (\sqrt{2}-1)\sigma$.

The motivation for our algorithm is the following observation:
**lemma**:
On TAPs with no bad tasks, BAL is already $(\sqrt{2}+1)$-competitive for completion time.

> We will not actually directly use this lemma in the proof of the theorem, but I'm leaving it in because it is instructive.

**pf**:
We claim that $\bal$ maintains the following invariant:
$$\C_{\bal}^i \le \K_{\opt}^i/p + \sqrt{2}\C_{\opt}^i.$$
If $\bal$ spends less work than $\opt$ on a task, then $\Delta\C_\bal \le \Delta\K_\opt/p$
The only remaining case is that $\bal$ parallelizes a task $\tau$ which $\opt$ serializes.
In this case, the task must have been large compared to $\C_\bal^i$ .
In particular we have
$$\C_\bal^i \le \sigma_{i+1}+t_{i+1}\le \C_\opt^{i+1}.$$
Furthermore, because there are no bad tasks by assumption, we have
$$\pi_{i+1}/p \le (\sqrt{2}-1)\sigma_{i+1}\le (\sqrt{2}-1)\C_\opt^{i+1}.$$
Overall we get:
$$\C_\bal^{i+1} = \C_\bal^i + \pi_{i+1}/p \le \sqrt{2}\C_\opt^{i+1}.$$
$$\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\blacksquare$$

> now we actually define the scheduler 


The "weird" scheduler $\wei$ operates as follows:
- $\wei$ serializes all bad tasks.
- If $\sigma_i+t_i < \K_\wei^{i-1}/p$ then $\wei$ runs task $i$ in serial
- Otherwise, $\wei$ runs task $i$ in parallel.
- Once $\bal'$ has chosen its desired jobs, the jobs are scheduled optimally (i.e., via prioritizing the largest serial tasks).

> **NO I don't think this really makes sense. Because then we are allowing jaggedness from non-bad tasks**

**observation**: If $\wei$ ends jagged then $\wei$'s completion time must be determined by a bad task, which was prioritized at each step, and which took $\opt$ a substantial amount of time to complete. 
In particular, if $\wei$ ends jagged then it achieves competitive ratio 
$$\frac{1}{\sqrt{2}-1} = \sqrt{2}+1.$$
Now, suppose that $\wei$ ends balanced.
Let $t_*$ be the last time before the end when $\wei$ is not saturated.
Let $S$ be the set of bad tasks which $\wei$ has present at time $t_*$.
Let $t_f$ be the time when $\opt$ finishes $S$.

**Case 1: $t_f < t_*$.**
In this case, we claim that $\opt$ can't possibly have run any of the tasks in $S$ in serial. This is true because we prioritize the tasks in $S$ at each time step since their arrival.
Thus, 
$$t_*\ge \sum_{i\in S}\pi_i/p \ge p\cdot(\sqrt{2}-1)\cdot \sum_{i\in S}\sigma_i/p.$$
In other words, the contribution of $\sum_{i\in S}\sigma_i/p \approx 0$ 
Let $F$ denote the set of non-bad tasks that arrive after time $t_*$.
Of course, 
$$\C_\bal^F \le (\sqrt{2}+1)\C_\opt^F.$$
Ah but shoot what do you do with $S'$?
**TODO: FIX THIS...**

And,
$$\C_\bal \le t_* + \sum_{i\in S}\sigma_i/p + \C_\bal^{\text{non-bad TAP SUFFIX}} + \sum_{j\in S'}\sigma_j/p.$$
So we have
and it's gg. 
**TODO: this is not quite right. TAP SUFFIX**

**Case 2: $t_f  > t_*.$**
Let $S$ be the set of bad tasks that $\bal'$ has left at time $t_*$. 
Let $S_\sigma$ be the subset of $S$ that $\opt$ runs in serial. 
Let $S_\pi$ be the subset of $S$ that $\opt$ runs in parallel.
Let $W$ be the amount of work that is left on the tasks $S_\sigma$ for $\bal$.
Key observation: $\opt$ also has (at least) $W$ work left for the tasks $S_\sigma$ at this time, because $\bal$ prioritized these tasks since they arrived.

Then, 
$$\C_\opt \ge t_* + W/p + \C_\opt^{\text{TAP suffix}}$$
While 
$$\C_\bal \le t_* + W/p + \sum_{i\in S_\pi} \sigma_i /p + \C_{\bal}^{\text{TAP suffix}}.$$
And because tasks in $S$ are bad we have
$$\sum_{i\in S_\pi}\sigma_i/p \le O(1/p)\cdot \sum_{i\in S_\pi}\pi_i/p$$
Combining stuff gives the desired result.


**todo: check that this generalizes easily to awake time**
