[[scheduling]]
### $O(\sqrt{p})$ competitive decide on arrival, parallel work oblivious,  scheduler 

First we consider completion time. Then we show that this algorithm can easily be adapted  to work for awake time as well.

Algorithm:
We will consider tasks to be the same size if they have serial work differing by a factor of $2$; rounding the sizes of our tasks up by $2$x can't hurt our awake time by more than $2$x.
For the first $\sqrt{p}$ tasks of each type that we see we run them in parallel. 
All remaining tasks of this type are run in serial.

Analysis:
Let $\sigma_k$ be the largest serial task that OPT runs. 
Let $\sigma_j$ be the largest serial task that we run.
Let $f_*$ be the time when the final task arrives.
Let $m_i$ be the number of tasks of type $\sigma_i$.
We can bound our completion time by:

$$T_{US}\le f_* + \max(\sigma_j, \sum_{i\le k} \left( \pi_i/\sqrt{p}  + m_i \sigma_i/p\right) + \sum_{i>k} m_i \pi_i/p  ).$$

The huge sum is clearly an upper bound on the work that we perform, and we are either bottle-necked by a single task or by work. 

Now we give lower bounds on OPT's work:
We are considering completion time:
$$T_{OPT} \ge f_*.$$
OPT runs all tasks of type $i>k$ in parallel:
$$T_{OPT}\ge \sum_{i>k} m_i \pi_i/p.$$
The fact that we ran $\sigma_j$ in serial means that there were at least $\sqrt{p}$  tasks of commensurate type:
$$T_{OPT} \ge \sigma_j/\sqrt{p}.$$
Of course
$$T_{OPT}\ge \sum m_i \sigma_i/p.$$
Finally, 
$$\sum_{i\le k} \pi_i/\sqrt{p} \le \sqrt{p}\sum_{i\le k} \sigma_i \le (2\sqrt{p}) \sigma_k \le (2\sqrt{p})T_{OPT}.$$
Combining the observations we see that 
$$T_{US}\le O(\sqrt{p} T_{OPT})$$
### handling awake time
The above analysis was for handling completion time.
Now we show how to handle awake time. 

WLOG we assume that the input TAP doesn't result in US having any gaps. 
However, on the input TAP OPT may have gaps. 

Our solution is as follows:
Whenever we detect that OPT has finished all present tasks we put our currently running tasks "in the background". In particular this means that when more tasks more we don't remember anything about how we handled the previous tasks. And, the background tasks only run on idle processors, and the way they run is by sharing the idle processors as best they can.
With this strategy it is fairly clear that we aren't so sad about the gaps.