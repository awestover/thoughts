I'm very interested in alignment research focused on **reasoning** models.
This is neglected by lots of current alignment work.
RL seems like the most relevant flavor of ML to understand to talk about reasoning models.
Let's understand how RL works in a bit more depth than [[intro-to-rl]].


Policy gradient method: directly modify $\pi_\theta$ based on $\nabla_\theta J(\pi_\theta)$. 

Note that sometimes (eg with Actor Critic) it'll still be useful to have a model of the value function.

$$
J(\pi_\theta) = \mathbb{E}_{\tau\sim \pi_\theta}[R(\tau)] = \mathbb{E}_{\tau\sim \pi_\theta}\left[ \sum_{t=0}^{T} r_{t+1}(s_t, a_t, s_{t+1})\right].
$$

Obvious fact:
$$ \nabla J(\theta) = \mathbb{E}_{\tau\sim \pi_\theta}\left[ R(\tau) \nabla \log \pi_\theta(a_t\mid s_t)\right] $$ 
This is already an RL algorithm.

Less obvious fact (Policy Gradient Theorem):
$$ \nabla J(\theta) = \mathbb{E}_{\tau\sim \pi_\theta} \left[\sum_t[Q^{\pi_\theta}(s_t, a_t) - V^{\pi_\theta}(s_t)] \nabla \log \pi_\theta(a_t\mid s_t)\right]. $$ 
TODO finish this
