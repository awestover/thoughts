I'm very interested in alignment research focused on **reasoning** models. This is neglected by lots of current alignment work. RL seems like the most relevant flavor of ML to understand to talk about reasoning models. Let's understand how RL works in a bit more depth than [[intro-to-rl]].

Our goal is to learn a (parameterized) policy $\pi_\theta$ maximizing our expected reward.
$$
J(\pi_\theta) = \mathbb{E}_{\tau\sim \pi_\theta}[R(\tau)] = \mathbb{E}_{\tau\sim \pi_\theta}\left[ \sum_{t=0}^{T} r_{t+1}(s_t, a_t)\right].
$$

**Policy gradient methods**: directly modify $\pi_\theta$ based on $\nabla_\theta J(\pi_\theta)$. 

> Note: it'd be a really good exercise for me to try to e.g., beat checkers using PPO.
> My guess is that this should be possible on a small compute budget (eg 1T4) with just PPO.

Note that sometimes (eg with Actor Critic) it'll still be useful to have a model of the value function.

**Obvious fact:**
$$ \nabla J(\theta) = \mathbb{E}_{\tau\sim \pi_\theta}\left[ R(\tau) \sum_{t=1}^{T}\nabla \log \pi_\theta(a_t\mid s_t)\right] $$ 
One trivial way to obtain an unbiased estimate of this is to just sample a trajectory according to $\pi_\theta$ and compute this thing on the sampled trajectory. This has terrible sample efficiency. We'd like to do "credit assignment" -- to understand which actions were responsible for getting a high reward at a much lower level of granularity than this.

The **Policy Gradient Theorem** lets us get some of this granularity:
$$ \nabla J(\theta) = \mathbb{E}_{\tau\sim \pi_\theta} \left[\sum_t[Q^{\pi_\theta}(s_t, a_t) - V^{\pi_\theta}(s_t)] \nabla \log \pi_\theta(a_t\mid s_t)\right]. $$ 
---

Here's how you turn this into an algorithm:

- We will have two neural networks. An actor $\pi_\theta$ and a critic $V_\theta$.
- We will have a sequence of **batches**. 
- At the start of a batch we freeze the parameters $\theta_{0}\gets \theta$.
- Within the batch we'll use the frozen parameters to estimate the advantage, and we'll use the non-frozen parameters when thinking about the log probs of certain actions.
- (At the beginning of the next batch we'll update the frozen parameters). 

- This trick of having frozen and non-frozen parameters is apparently very important. 
	- Feels like similar vibes to [[murphy -- Theoretical ML textbook|EM]].
- The "proximal" in PPO:
	- We don't allow $\theta$ to drift too far from $\theta_{0}$ during a batch.
	- We literally just clip the diff --- which kills some gradients.

- okay but what actually happens during a batch, how do we update $\theta$?

- We're going to optimize the following function, which is actually tractable to compute / compute gradients of (unlike $J(\theta)$):
$$L_B(\theta) = \frac{1}{|B|}\sum_{t\in B} \log \pi_\theta(a_t\mid s_t) \widehat{A_{\theta_{0}}}(s_t,a_t)$$
- You might guess that $\widehat{A_{\theta_{0}}}(s_t, a_t)$ is just 
$$
\delta_t = r_t + V_{\theta_{0}}(s_{t+1}) - V_{\theta_{0}}(s_t).
$$
- Or maybe that it incorporates some more long range stuff, e.g., $\sum_{t'>t} \delta_{t'}$.
- Instead we'll do $\sum_{t'>t} \lambda^{t'-t}\delta_{t'}$.


- Actually I was missing a couple of things that we need to add to the loss function.
- We need to train the critic. 
- We'll do this by adding a term $(V_{\theta_0}(s_t)+\widehat{A}_{\theta_{0}}(s_t,a_t)-V_\theta(s_t))^{2}$ to the loss.
- We'll also add some term that measures the entropy of $\pi$ to encourage exploration
	- we damp this term over time.


----

Random aside: apparently there is a way to do RL with transformers. 
It's called **decision transformers**.

Here's the idea. Suppose you have some task, like finding a certain vertex in a graph.
Suppose you got some samples of trajectories. Specifically, you saw some data of the form $s,a,R,s,a,R,\dots$.
Suppose that you built a good auto-regressive model of this sequence.

What I'd probably do with this: 
- Try each action, see which one model thinks is the best (ie predicts highest reward for). 

I think what DT's do is: 
- Find most likely action conditional on getting high reward. 

---

# An obvious remark about RL

An RL agent cannot read your mind --- it does not know what the reward function is. If there is some state that it has never been in, and that state has really high reward, the RL agent might just not know. Local optima are attractor basins. You can try to escape by incentivizing exploration. But if the agent has no probability of a correct action, it's never seen anything like it then tough luck -- you can't reinforce what doesn't exist. 

So anyways, hopefully the reward signal has a simple explanation.