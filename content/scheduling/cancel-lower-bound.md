[[scheduling]]
[[phi-cancel-width-zero]]

>[!info] Definition:
>A **cancelling** scheduler is allowed to take a task that was already started in parallel and switch it to run in serial.
> 
   In the **width zero setting** we say that serial tasks do not take up processors --- instead, they just give you a lower bound on your completion time, namely of $\sigma_i+t$ (when started at time $t$).
#### result:
**Theorem**:
For any $\eps>0$, there are no deterministic schedulers, even with cancelling
and in the width-zero setting, that achieve competitive ratio $1.5-\eps.$

TAP: 
- Task 1 has $\sigma=2, \pi/p = 1$.
- We continuously feed "infinitely small" tasks that must be parallelized, *until* the algorithm cancels task 1 and switches task 1 to serial. 

Observations: 
- For $t\in [0,1]$, our estimate of $\C_\opt$ is always $t+1$.
- For $t\in [1,2]$, our estimate of $\C_\opt$ is always $2$. 

**Claim 1**: If $\alg$ serializes task 1 at some time $t\in [0,1]$ then $\alg$ is not $1.5-\eps$ competitive.
> Proof: For any $t\in [0,1]$ we have:
> $$.5+\eps > (.5-\eps)t \iff t+2 > (1.5-\eps)(t+1).$$
That is, the completion time that $\alg$ would achieve by switching task 1 to serial at time $t$ is more than $1.5-\eps$ times larger than our projection for  $\C_\opt$.

**Claim 2**: If $\alg$ serializes task 1 at some time $t\in [1,2]$ then $\alg$ is not $1.5-\eps$ competitive.
> Proof: For any $t\in [1,2]$ we have:
> $$t>1-2\eps \iff t+2 > (1.5-\eps)2.$$
That is, the completion time that $\alg$ would achieve by switching task 1 to serial at time $t$ is more than $1.5-\eps$ times larger than our projection for  $\C_\opt$.

Combining Claim 1 and Claim 2 we deduce that in order for $\alg$ to have any chance of being $1.5-\eps$ competitive, $\alg$ must not cancel task 1 at any time during $[0,2]$. Then we have:
$$\C_\alg \ge 3 \implies  \C_\alg / \C_\opt \ge 1.5.$$
This shows that it is impossible for $\alg$ to have competitive ratio better than $1.5$. 