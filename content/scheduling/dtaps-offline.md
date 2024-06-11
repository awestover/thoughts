[[scheduling]]
# Exact DTAPs offline are strongly NP-hard
$$\newcommand{\eps}{\varepsilon}$$
>[!info] Fact
>Exact 3 cover (X3C) is a classic NP-hard problem. 
> The problem is, given a 3-regular hypergraph, find a set of half of the hyperedges that hits every vertex exactly once. Or maybe I'm not quite remembering correctly. 

> [!info] Theorem
> Exactly computing the optimal awake time for a DTAP is strongly NP-hard, even in the width-zero, arrival time zero setting and when we restrict to DTAPs where all connected components are chains. 

>[!tip] Proof:
> Let $G= (U, E)$ be our X3C instance, and let $m = |E|, n = |U|$. Identify $U$ with $[n]$.
> For each hyperedge in our hypergraph, we create a chain. At the start of the chain is a task with parallel work $1$ and serial work $m/2-XXX$. 
> Then, we put "kebobs" after the first task. A kebob for hyper edge $(i,j,k)$ with $i<j<k$ is the following chain of tasks: 
> - $i$ forced serial time, 
> - $1$ forced parallel time
> - $j-i-1$ forced serial time
> - $1$ forced parallel time
> - $k-j-1$ forced serial time.
> #todo: need to change the scale... the kebobs are supposed to be tiny compared to the rest of the stuff
 >
> The claim is that if you choose to serialize  $1/2$ of the chains corresponding to an X3C solution, then you will be able to finish the TAP in time XXX, and otherwise you will not. 
> 
> To check the "otherwise you will not" clause we consider the possible things that the scheduler could do.  
> 1. The scheduler could decide to serialize the first task in more than $m/2$ of the chains. This would instantly destroy them. 
> 2. The scheduler could decide to serialize the first task in less then $m/2$ of the chains, or choose to serialize the first task in $m/2$ of the chains in such a way as to not correspond to an X3C solution. Then, we will get behind in one of the kebobs because we will have multiple units of parallel work at the same time. There is no way to recover from getting behind in a kebob.

Remark: This probably also works for the non-width zero setting. 
Certainly setting $p$ to be really huge probably does it. Maybe can even get away with $p=2$.

# An approximation scheme for simple DTAPs
>[!info] Theorem
>Suppose you have a DTAP where all connected components have size at most some constnat $L$. Then, for any constant $\eps>0$, there is a polynomial time $1+\eps$ approximation algorithm for optimizing awake time.  

>[!tip] Proof:
>First, we round the parallel sizes to be multiples of $T\eps/n$; this increases the completion time by at most $T\eps$. 
>Suppose we are trying to decide whether the DTAP can be performed in time $T(1+\eps)$ assuming it can be completed in time $T$. If there is a schedule requiring time $T$, then there is a schedule requiring time at most $T(1+2L\eps)$ where all serial tasks start at times which are integer multiples of $\eps T$, even if we round the serial task sizes up to be multiples of $\eps T$. We now show how to find some such schedule given that one exists.
> By scaling we assume $T=1$.
> Let $N = \lceil \eps^{-1}\rceil +2L$.
> The "relevant times" (i.e., times when serial tasks can start) are $\eps\cdot [N]\cup 0$.
> 
> Define a **task completion strategy** as follows:
> - Assign each task an interval in which it promises to finish. This interval is a sub-interval of $[0,N]$ where both endpoints are integral.
> - The assigned intervals must have the property that each task's interval starts after all the tasks that it depends on have promised to finish.
> 
> We say that a task completion strategy is **good** if it is possible to complete all tasks during their promised completion intervals.
> 
> **Claim 0**: We can efficiently check if a task completion strategy is good. 
> **proof**: Given a task completion strategy it is pretty clear how you should run the tasks: prioritize things that have promised to finish earlier. If this simple greedy strategy fails then it is impossible for the task completion strategy to be good. 
> 
> **Claim 1**: The existence of a schedule where serial tasks start at relevant times that achieves completion time $N$ implies the existence of a good task completion strategy. 
> **proof**: 
> Make the promised completion interval of each task be the smallest integer-endpoint interval in which it completes. This is clearly a good task completion strategy.
> 
> **Claim 2**: The existence of a good task completion strategy implies the existence of a schedule with completion time at most $N$.
> **Proof:**
> By definition of a good task completion strategy we can run the tasks so that they all complete within their promised completion intervals, which in particular all end before $N$.
> 
> **Claim 3**: We can efficiently determine whether there is any good task completion strategy. 
> **Proof**: 
> Here's a pretty silly way to do it:
> DP\[current chain index, amount of work promised in each of the $binom{N}{2}$ possible intervals\]
> To update for a new chain:
 (1) Iterate over all $2^L$ possibilities for which tasks in the chain to serialize and which ones to parallelize.
(2) Iterate over all $\binom{N}{2L}$ ways to choose promise intervals for the tasks in the chain. 
(3) For any such combinations which are reasonable (serial works fit in the intervals), add the promised parallel works to the appropriate intervals.
> 
> Combining all the claims, we see that to check whether the DTAP can be completed in time $N$ we can check for the existence of a good task completion strategy, which can be done efficiently. 


# An algorithm for another simple class of DTAPs

> [!theorem] 
> Suppose the DTAP is a union of $k$ chains. 
> Fix constant $\eps>0$. There is a $1+\eps$ approximation for the awake time of this DTAP with running time $n^{O_{\eps,k}(1)}$.

>[!tip] Proof:
>
>Suppose we are trying to decide whether the DTAP can be performed in time $T(1+\eps)$ assuming it can be completed in time $T$. By rescaling we may assume $T=1$. 
> If the DTAP can be completed in time $1$, then there is a schedule with completion time at most $1+2\eps$ where all tasks start at times which are multiples of $\eps/n$ even if we round up the works of all tasks to be multiples of $\eps/n$. 
> 
> Now we do a DP. The index for our DP is the current time, our progress into each chain, and our progress into the task currently live from the chain. This gives running time $(n/\eps)^{O(k)}$.


# next up
We were trying the following problem:
> [!info] Q
> Can you do a $2+\eps$-approximation for chains?

>[!tip] Attempt
>Let $1$ be the goal end time. 
**observation**: WLOG can assume no tasks of size $<\eps/n$
We can geometrically discretize. This isn't super great yet. 

---
# old thoughts (not width zero model)
Q: is DTAPs offline NP-hard?
consider the chain case
 - Nathan gave a simple algo for solving 2 chains on 2 processors with forced pll/ser decision:
    - just start by giving a processor to each and then shift some parallel work off of the more full one till they are balanced. 

- not clear about less restricted versions
- A/B variant: (very loosely related)
- AAAABBBBABABABAB ABABABBABAB shortest common super string: n^2 time
- but what if not unary? (I still don't know yet)