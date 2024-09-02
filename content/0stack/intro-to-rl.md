As per plan dictated in [[live research options]] (and fortunately also because I'm feeling intellectually interested) I am devoting some resources to learning about ML. In this post I'll explain the basics of RL. Will start with a bunch of definitions.

Ref: Sutton Barto RL textbook 

## chapter 1: intro

This book has two parts:
- Part 1: tabular methods
- Part 2: approximate solution methods

Tabular methods is for when the state space is relatively small (e.g., can fit reasonably in memory). o/w need approx methods.

## Chapter 2: bandits

$k$-armed bandits is arguably the simplest possible setting for RL: there's just one state. 
Various flavors of the problem, e.g., stationary/non-stationary arm rewards.

To make it an interesting problem let's say you don't know the rewards. 

A simple strategy: the $\varepsilon$-greedy strategy explores random arm with Pr $\varepsilon$, o/w it greedily chooses the best arm. 
## Chapter 3: defining a MDP and some notations 

- **RL**: a suite of methods for approximately solving the Bellman Equation
- **MDP:** States, actions, and transition rules. Specifically, p(new state, reward | old state, action).
	  - NB: this should obey the markov property: the transitions just depend on your current state and action, not your state two moves ago.
	  - If the transitions do depend on information from the past, then your state should include that info too. E.g., your state might need to be all the moves that have ever happened. This is generally probably pretty unfortunate though. 
- **Our goal**: optimize the time-discounted reward $\sum_{t\ge 0} \gamma^t R_t$
	- "Episodic" --> you eventually reach a terminal state and that just get 0 reward forever after.
	- "continuing" --> no time limit. Then, it's usually a good idea to have $\gamma < 1$ and $\sup_{t\ge 0}R_t <\infty$.
- **Policy**: $\pi(a \mid s)$: probability distribution over actions given the state
- $v_\pi(s)  = \sum_{a} \pi(a\mid s) \sum_{s',r} p(s',r\mid s, a) (r+\gamma v_\pi(s'))$: value of a state, with respect to a policy. This is your expected reward if you employ strategy pi starting at state s.
- $q_\pi(s,a) = \sum_{s',r}p(s',r\mid s,a)(r+\gamma v_\pi(s')).$ This is sometimes called q-function or action-value function.
- **optimal policy:**  A policy that maximizes v_pi(s) for all states s
	Note: It's useful to think of policies as partially ordered by their value functions.
	In particular, we say that a policy dominates another policy if it has higher value function on every single state. It turns out that there is an optimal 
	policy (although not necc unique).
- By *math* it turns out that you can actually solve for the optimal policy.  Like you get a bunch of equations and stuff. 

**But there's a catch! Actually, there are three separate catches.** 
1. Uncertainty about transitions.
2. Too computationally expensive. 
3. "Markov Property"
   (If you're pedantic this is not really a thing. We're interested in solving MDP's. By definition they have the markov property. But ykwim.)

---

## chapter 4: DP
Usually too expensive but very foundational; other stuff is just trying to approx this. 

> Q1: Given a policy $\pi$ how to compute $v_\pi$?

- In principal, if you knew all the transitions then you could invert some huge matrix. 
- But, maybe that matrix is kind of big. 
- And you have bigger problems if the transitions are unknown.
- So here's an iterative method that converges to the correct answer:

$$
v_{k+1}(s) = \sum_a \pi(a\mid s) \sum_{s',r} p(s',r\mid s,a)[r+\gamma v_k(s')].
$$
Base conditions is put value zero at all terminal nodes. 

An interesting optimization that they propose is using the  newly computed values for $v_{k}$  as soon as they're available. Apparently this is still rigorous. And it's pretty easy to see how this could vastly accelerate convergence. E.g., propagation in a path could happen in like 1 round rather than n rounds.   


> [!tip] The Policy Improvement Theorem
> Suppose $\pi,\pi'$ are deterministic policies satisfying $q_\pi(s, \pi'(s))\ge v_\pi(s)$ for all $s$. Then $v_{\pi'}(s) \ge v_{\pi}(s)$ for all $s$. (i.e., $\pi'$ is at least as good as $\pi$).

So I guess this gives you an algorithm for improving a suboptimal policy: 
- Choose a local place where modifying the strategy is obviously a good idea, and then do that local modification.
- Ok apparently you can actually do this simultaneously in parallel at all vertices and that is still legit. Yup this makes sense. Like the whole idea is that the new strategies made by these local updates DOMINATE the old strategies. They're better at every single freaking state. So it's not an issue to do this update in parallel. 

> Alek: "oh there's no way that doesn't just get stuck in a local optima really easily. I guess you can explore starting from a variety of different places..."\
> JJ: "nope lol if policy improvement doesn't work then you have found a 'fixed/stable point' which is guaranteed to be the optimal policy".\
> Alek: dang that's pretty op
> JJ: Yeah, it turns out it gets even better. This process of repeatedly evaluating a policy (computing value functions) and then doing policy improvement with those is guaranteed to converge in a finite number of steps if your MDP stuff is finite, because it has to be a strict improvement until it starts becoming equality which is when you're optimal.

Another note: 
Something that is usually done to speed this up is to just interleave the policy evaluation and policy improvement steps. 
I.e., you don't need to wait for perfect policy evaluation before doing the policy improvement stuff. Apparently this works p well.

Now, I'll work through one of the exercises. I think the optimal policy here is pretty non-obvious, but my little RL mouse could smell the cheese and just went for it. 

```python
"""
Problem description:

Gambler has some money 0<X<100. 
At each round, gambler can bet any amount of money <= X. 
- Reward of +1 for winning the game.
- Reward of 0 for everything else. 
No time discounting or whatever, let's just maximize sum_{t\ge 0} R_t

Although I might play around with some stuff like 
"""

import matplotlib.pyplot as plt
from random import random as rand
N = 100
# the state space is range(N)
# specifically, all terminal states (e.g., >= $10 or <= $0) are represeted by 0.
# besides this, i represents having $i
pcoin = .6 # pr[heads]
gamma = 1

"""
a couple of helper functions for queries about the state
"""
def terminal(capital):
    return capital <= 0 or capital >= N

def terminal_reward(capital):
    return capital >= N

def new_capital(capital, pi):
    stake = pi[capital]
    if rand() < pcoin:
        return min(capital+stake, N)
    else:
        return max(0, capital-stake)

"""
some helper function for propagating values
"""
def propagate_value(capital, stake, val):
    # win stake
    new_capital = min(N, capital+stake)
    r = new_capital == N
    winval = r + gamma * val[new_capital%N]

    # lose stake
    new_capital = max(0, capital-stake)
    r = 0
    loseval = r + gamma * val[new_capital%N]

    return pcoin*winval + (1-pcoin)*loseval

"""
Iterative procedure for updating values.
NB: it's guaranteed to converge to the correct value, and strictly increase until then.
"""
def update_values(val,pi,nsteps):
    for step in range(nsteps):
        for capital in range(1,N):
            val[capital] = propagate_value(capital, pi[capital], val)
    return val

def find_optimal_policy():
    # Note: Doing a couple of searches from random starting locations would be
    # smarter. But this method is guaranteed to converge and the state
    # space is small enough that this should be chill.
    pi = [1 for i in range(N)] 
    val = [0 for i in range(N)]

    NROUNDS = 1000
    VALUE_STEPS = 10

    for round in range(NROUNDS):
        # estimate the new value
        val = update_values(val, pi, VALUE_STEPS)

        # update the policy to be the greedy policy based on the current value function
        # this is guaranteed to be a better policy 
        # (assuming that we had a decent estimation of the value function) 
        for capital in range(1,N):
            best_stake = 1; best_val = 0
            for stake in range(1, capital):
                alternate_val = propagate_value(capital, stake, val)
                if alternate_val > best_val:
                    best_stake = stake
                    best_val = alternate_val
            pi[capital] = best_stake

    pi[0]=0
    return pi



"""
***************
PLOTTING STUFF
***************
"""



"""
estimate the value of a policy by just running the policy a bunch of times
"""
def empirical_value(pi, TRIALS=1000):
    v_estimate = [0 for i in range(N)]
    for trial in range(TRIALS):
        for i in range(N):
            capital = i
            while not terminal(capital):
                capital = new_capital(capital, pi)
            v_estimate[i] += terminal_reward(capital)
    for i in range(N):
        v_estimate[i] /= TRIALS
    return v_estimate

def estimated_value(pi):
    return update_values([0 for i in range(N)], pi,1000)

def plot_policy(policy,col,name):
    plt.step(range(N),policy,where='mid',c=col)
    plt.scatter(range(N),policy,label=name,c=col)

def plot_value(value,label,col,ls):
    #  plt.step(range(N),value,label=label,where='mid')
    plt.scatter(range(N),value,c=col,ls=ls)
    plt.plot(range(N),value,label=label,c=col,ls=ls)

# Define policies
pi_opt = find_optimal_policy()
pi_all_in = [i for i in range(N)]
pi_silly = [1 for i in range(N)]
pi_vee = [i if i <= N//2 else N-i for i in range(N)]

print("PIOPT", pi_opt)

# PLOT
COLORS = ["r", "g", "b","c"]
POLICIES = [pi_vee, pi_opt]
NAMES = ["vee", "OPT"]

fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(20, 10))

# Plot policies in the first subplot
for pi, col, name in zip(POLICIES, COLORS, NAMES):
    plt.sca(axes[0])  # Set the current axis to the first subplot
    plot_policy(pi,col,name)

axes[0].legend()
axes[0].set_title("Policies")

# Plot value functions in the second subplot
for pi, col, name in zip(POLICIES, COLORS, NAMES):
    plt.sca(axes[1])  # Set the current axis to the second subplot
    plot_value(empirical_value(pi), name + " EMP", col, '-')
    plot_value(estimated_value(pi), name + " EST", col, '--')

axes[1].legend()
axes[1].set_title("Value Functions")

fig.suptitle(f"pcoin={pcoin}")
plt.tight_layout()
plt.show()

```

> chatgpt being sassy: 
> "The function name `propogate_value` should be `propagate_value` (correct spelling) unless you intentionally named it this way."

![](images/gamble60.png)
![](images/gamble50.png)
![](images/gamble40.png)
So you might be concerned about the fact that the curve labelled "OPT", trained by my RL agent isn't consistently outperforming this random baseline that I came up with. 
So I think I accidentally came up with a pretty reasonable baseline (for the case $p<.5$ at least).
I'm pretty happy that my RL agent at least approximately converges to this. 
So maybe there is some minor bug in my code preventing us from actually getting optimal...
I'll take a look sometime, but for now going to forge ahead. 
Honestly it's also possible that OPT looks suboptimal, but this is simply an artifact of how I'm estimating the value functions.
Believing this will at the very least help me sleep at night.

**chapter 4 --- to be continued**
## chapter 5: monte carlo
## chapter 6 temporal-difference learning
## chapter 9 on-policy prediction with approximation

