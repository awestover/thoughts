[[scheduling]]
see also: [[phi-cancel-width-zero]], [[phi-gr-kinda-broken]]
$$\newcommand{\gr}{\mathsf{gr}}$$
$$\newcommand{\C}{\mathsf{C}}$$
$$\newcommand{\opt}{{\mathsf{OPT}}}$$

# EDIT: THIS STUFF IS BROKEN WRONG WRONG WRONG
#  Like the WLOGs are not actually wlog, although they feel fairly reasonable

# 1.84-competitive scheduler
First we show how to get $1.84$, where this is a root of the polynomial $x^3 = x^2+x+1$.
For convenience we write $1.9$ instead of $1.84$. 

$\gr$ makes decisions as follows:
- run one task in parallel at a time, in order of increasing arrival time
- if ever $\sigma_i + t_j < 1.9\C_{\opt_j}^j$, and task $\tau_i$ is not yet started, start $\tau_i$ in serial.

Usually we work with an arbitrary fixed and "not silly" sequence
of schedulers $\opt_i$. Not silly generally means that for all
$j<i$, $\opt_i^j \le \opt_i^{j+1}$. In this note we will assume a
stronger property of $\opt$: namely, that it is an "aggressive
parallelizer". That is, after coming up with some arbitrary
schedule, it repeatedly finds the earliest task such that
parallelizing that task doesn't sacrifice it's completion time,
and parallelizes that task. It keeps doing this until it can't be
done anymore. So, in this note when I say $\opt_i$, I'm talking
about this scheduler. 

We will prove the following theorem:
**Theorem 1:**
$\gr$ is $1.9$ competitive on TAPs with no $\opt$ gaps in the zero width setting.

> [!info] **Lemma 1:**
> WLOG the TAP is such that $\gr$ never serializes any task, and
> such that $\opt$ serializes a prefix of the tasks, and then
> parallelizes a suffix of the tasks, and such that the parallel
> work for $\opt$ all arrives immediately before it is supposed
> to be completed (in particular the parallel work arrives in
> "infinitely thin" slices).

>[!tip] proof:
> Suppose $\gr$ serializes a task. Then we could make a harder TAP by removing that task. 
> 
> Clearly chopping up the work from the final task can only make
> that task more difficult. Now, suppose that there is some task
> that $\opt$ parallelizes. We claim that by pushing this task to
> the end, we make the TAP harder. In fact, we will argue that
> this operation does not change $\C_{\opt_i}^i$ for all $i$, and
> hence the only result is that $\gr$ is less capable of
> serializing things. The fact that $\C_{\opt_i}^i$ does not
> increase when we push the serial tasks to arrive at appropriate
> times is true by the no $\opt$ gap property. 

The theorem will follow immediately by induction on the following claim:
>[!info] **Lemma 2**:
> Fix $n$. Assume that $\gr$ is $1.9$ competitive on all TAPs of
> length smaller than $n$. Fix a TAP $\mathcal{T}$ of length $n$,
> subject to the WLOG assumptions specified in Lemma 1. $\gr$ is
> $1.9$  competitive on $\mathcal{T}$.

!tip] **Proof**
Let $\tau_a$ be the first task that arrives as part of the tiny
slices of tasks, and let $\tau_{a-1}$ be the final task that
$\opt$ serializes.  Let $\Pi$ be the total parallel work that
will arrive for $\opt$.\
WLOG: $\Pi + t_a = \sigma_{a-1}+t_{a-1}=  \C_\opt.$\
We have:
$$
\C_\gr = \C_\gr^a + \Pi = \C_\gr^a - t_a + \C_\opt.
$$
We further have:
$$
1.9\C_{\opt_a}^a = 1.9\C_{\opt_{a-1}}^{a-1}\le \sigma_{a-1}+t_{a-1}= \C_\opt.
$$
**Case 1** At time $t_a$, we believe that $\opt$ runs tasks $\tau_1, \ldots, \tau_a$ all in parallel. 
$$
\C_\opt^a = \pi_1 + \sum_{1<j\le a} \pi_j = \C_\gr^a.
$$
Thus, 
$$
\C_\gr \le \C_\opt^a +\C_\opt \le (1+1/1.9)\C_\opt.
$$
>
**Case 2**: At time $t_a$, we believe that $\opt$ runs task $\tau_1$ in serial, and tasks $\tau_2,\ldots, \tau_a$ in parallel.
$$
\C_\opt^a = \max(\sigma_1, \sum_{1<j\le a}\pi_j).
$$
Then 
$$
\C_\gr^a = \pi_1 + \sum_{1<j\le a}\pi_j \le \C_\opt^a + \pi_1 \le (1+1/1.9)\C_opt^a \le (1/1.9+1/1.9^2)\C_\opt.
$$
Then
$$
\C_gr \le (1+1/1.9 + 1/1.9^2)\C_\opt - t_a \le 1.9\C_\opt.
$$
>
**Claim**: Cases 1 and 2 are exhaustive; in other words, $\opt_a$ must run $\tau_2, \ldots, \tau_a$ in parallel. 
> > [!tip] **proof:**
> Because $\gr$ runs all tasks in parallel by assumption, for each $j\in [2,a]$ we have:
$$
\sigma_j+t_a > 1.9 \C_\opt^a.
$$
But, wlog $\C_\gr^a - t_a > .9\C_\opt$,\
So $.9\C_\opt  + t_a < \C_us^a < 1.9\C_\opt^a.$

But then, 
$$\sigma_j+t_a > .9 \C_\opt + t_a,$$
so $\sigma_j > .9\C_\opt.$
But we know  $\C_\opt^a < \C_\opt/1.9< .9\C_\opt < \sigma_j.$
So $\opt$ runs $\tau_j$ in parallel.

# very sus argument for a $\sqrt{3}$ competitive scheduler

>[!bug]
Assume that there are only $3$ tasks. Assume that $\opt_2$ looks like it'll serialize the first task. In fact, just assume $\pi_2 = \sigma_1$. We'll remove all these assumptions tomorrow. 
Of course we are going to assume that $\opt_3$ actually does serial, serial, parallel. 
Assume $\C_\opt^{\pi_1}$ ($\C_\opt$ estimate at the time when the first task is completed) is $\pi_2+\pi_1$; i.e., that we didn't realize that $\opt$ is going to serialize $\tau_2$ yet. 
> 
If we made all these assumptions then, we'd have:
$\C_\opt = \sigma_2 > .8\pi_1 + 1.8\pi_2 \approx .8 \sigma_1/1.8 + 1.8\pi_2  \approx (.8/1.8+1.8)\pi_2 = (.8/1.8+1.8)\sigma_1 \approx (.8+1.8^2)\pi_1$
> 
And of course, this means 
$\C_\gr \le (1/(.8/1.8+1.8)+1/(.8+1.8^2)+1)\C_\opt.$
Optimizing gives you $\sqrt{3}$. 
Anyways, not clear exactly how impressive this is. There are a lot of assumptions to peel away. But it's a start I guess.


