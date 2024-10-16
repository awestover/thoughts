In this post I talk about the existence of my objective function.
Some related and important questions include: [[Can I change my objective function? Should I?]] and [[Should I change my objective function?]] But it's impossible to measure diffs to my objective function without actually knowing what it's starting out as. 

**Fact 1:** 
Alek is an agent with preferences. Alek prefers some states to other states. 

For the duration of the post we shall use $J_A(\theta)$ to denote Alek's objective function, where $\theta$ is "state".

**Fact 2:**
Alek is an agent that acts. Alek's actions have an impact on $\theta$, which in turn impacts $J_A(\theta).$

**Fact 3:**
There is no such thing as a no-op action. 
Time increments. Entropy rises. No moment ever recurs. There are ([[Will humans beat death in the next 30 years?|presumably]]) no replays. 

---

Fact 1 is probably the most intuitive, but it still has some complexity.
One challenge relating to fact 1 is that I don't have direct access to $J_A(\theta)$.
At best, I can obtain noisy approximations to $J_A(\theta)$. 
Factors contributing to the noise I must fight when trying to determine $J_A(\theta)$ include:
- how much I [[sleep|slept]] last night
- how much [[food-water|food and water]] I've been consuming
- how I have responded to, and perceived, the events of the day
- the set of [[cognitive distortion list|cognitions]] that I've been letting be prominent in my mind
- the set of [[stimuli]] that I have exposed my brain to 

But in some sense modelling my understanding of $J_A(\theta)$ as being "I get $J_A(\theta)+\mathcal{N}(0,\Sigma)$" is overly optimistic. A more realistic model might be that my brain runs processes like [[avoid_discomfort.cpp]] that are each allowed to shift mess with the output of my query to $J_A(\theta)$ and they may do so adversarially for their own nefarious purposes.

Fact 2: 
I have [[responsibility]].

Related to Fact 3:
[[wrong exists]], and it's totally fair game for "status quo" to be wrong. 
Furthermore, [[procrastinating choices]] is a choice, and often wrong. 


**See** [[goodness(universe)]] for a description of what my objective function actually is.