[[scheduling]]
$$\newcommand{\E}{\mathbb{E}}$$
My best lower bound is actually from Yau'ing [[cancel-lower-bound]]
Note: this works even for width zero, and even for cancelling schedulers
It's important to note that in my definition of a cancelling scheduler, I do not allow cancelling serial tasks, only parallel tasks.
Actually I'm not sure how important this really is. It's at the very least convenient; it makes stuff much easier to talk about. It might be really important. 

![[desmos.png]]

Let me spell this out a bit more. 

The randomized TAP is:
- Task 1: serial time $2$, parallel time $1$.
- Choose a uniformly random value $u\in [0,2]$, give work one little bit at a time until 

A strategy for facing this TAP basically has to look like as follows:
> You decide at some time $T$ to cancel task 1, as long as $u>T$.

So, to analyze your competitive ratio we have two cases:

**Case 1: $T\in (0,1)$.**
Then, 
$$\E[\C_{us}/\C_{\opt}] = \Pr(u<T) + \Pr(u>T)\frac{2+T}{\E[\min(1+u, 2)\mid u>T]} = T/2 + (1-T/2)\cdot \frac{2+T}{\Pr[u<1\mid u>T](1+T)/2 + \Pr[u>1 \mid u>T]2}$$
**Case 2: $T\in (1,2)$**
$$\E[\C_{us}/\C_\opt] = \Pr[u<T]\E\left[\frac{1+u}{\min(2,1+u)}\mid u<T\right] + \Pr[u>T](1+T/2).$$
See Desmos screenshot for simplification and optimization.

- The lower bound from [[bounded-parallelism]] works. 
- The $\phi$ lower bound notably does not work
