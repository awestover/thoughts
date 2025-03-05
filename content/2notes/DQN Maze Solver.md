I recently spent 3 days (~10hrs/day) on coding a maze solver with RL, [code here](https://github.com/awestover/DQN-maze-solver).

I'll write a post-mortem on the project now, and then go brainstorm what's next. 

---

![[Pasted image 20250305165309.png]]

## Performance debugging

This project was somewhat weird as ML projects go, because I was working with really small neural networks: for 9x9 grids I mostly was using a 50K param CNN and a 100K param MLP. 

Here is what `profile` should look like: 
- 45% of time on backwards passes
- 22% of time on optimizer stuff
- 22% of time on forward passes
- 10% misc

Especially if you have a lot of VRAM you should be using as large a batch size as possible. This is also supposed to smooth out your gradient updates meaning that you can use a larger learning rate.

I realized that my `profile` didn't look like this several times. 

- The culprit was generally that I was doing something CPU bottlenecked, and the solution was to vectorize it. 
- Another thing I did is I decided to pre-generate all my mazes, and reuse some with small perturbations.
	- I'm a little worried that this caused some overfitting, but I haven't investigated this thoroughly.

I feel like knowing that a low-end GPU can do 30 TeraFlop/s is helpful for rough calculations. But I'm definitely not a hyperparam wizard. 

## Learning debugging

This is much harder. I didn't really succeed tbh --- my AI does do substantially better than a random agent I think. And in really simple mazes I was actually able to get it to basically be perfect. I'll think about if there were "architecture" mistakes in a bit, but first let me say what I think I learned about learning.

```python
EPISODE_MAX_LENGTH = 50
BSZ = 4096
UPDATE_TARGET_EVERY = 128
BUFFER_CAPACITY = 10**6
NUM_EPISODES = 10**4
MAZE_CACHE_SIZE = 10**6
lr=1e-5
gamma=.99
epsilon (exploration vs exploitation tradeoff) = .1+.9*exp(-3 * episode/NUM_EPISODES)
```

fp16's are supposedly hot, but i was having issues with them so i stuck to fp32's.

So, here's basically how it worked:

```
for _ in range(NUM_EPISODES):
	Run BSZ many parallel instances of maze solvers 
	(ie collect experiences by acting according to their policies, 
	and add these experiences to the replay buffer)

	(ideally you would "pipeline" this -- some agents will finish before other
	agents, you do not want them to just sit around doing nothing ever.
	I was too lazy to do this, 
	and expect it would have only been a ~3x time saver)

	interspersed with the experience gathering, you have *updates* to the 
	*policy network*. 
	1:1 ratio of experiences to updates seems pretty reasonable.

	the way an update works is you use SGD to update your parameters theta
	so that Q_policy(s,a) is close to r + gamma*Q_target(s', a')
	where a' is the action that policy net recommends, 
	s' is the new state you'd go to and r is the reward you'd get.
	(Like your loss can penalize distance between these two things).

	I updated the target net after every 128 updates of the policy net. 
	(like you just set them equal to each other)
	(I thought the idea of having the policy net gradually track the 
	target net via exp weighted decay or whatever was interesting 
	but didn't try it)
```


**ok but how do you learn?**


THE MOST important thing is probably **curriculum learning** / building a good reward signal.

The idea of curriculum learning is quite simple:
- Start having the AI solve simple tasks.
- Then once it's gotten good at those, have it try slightly harder tasks.

If your task can be decomposed into lots of little steps (many tasks can!) and the AI can master each step, then it can master the whole thing. 

My "easy mode" mazes were:
- I drilled a bunch of holes in the maze
- I started the agent near the end of the maze

This seemed to help quite a bit.

> priority replay buffer -- learn more from surprising experiences -- a nice idea. 

In terms of debugging, you should probably plot the loss. I found that I was not sufficiently concerned about the loss increasing when it was just numbers in my terminal. I would have been more concerned if I had plotted it.

irdk how to fix exploding gradients. My best take is "use a lower learning rate" and use gradient clipping.
But I can't really claim to have fixed this.

Making sure the reward signal makes sense is a good idea too.

 I think making more plots to understand the AI's strategy would've helped. I had a little thing where I could run the AI's policy on a maze -- this was somewhat helpful and at least fun. But maybe could've used something higher fidelity.

## Meta RL takeaways

> Deep RL systems are really hard to train. 

One funny thing was that at some point I gave the agent $2$ points for getting closer to the target and $-1$ point for getting further away. You'll never guess what the agent did :p.

Reward shaping seems really powerful -- sparse rewards are just not fun. But reward shaping seems supa hard to do right. And this is just begging for the agent to hack your reward signal.

RL just obviously makes evil dudes. Idk how to fix it. There's no such thing as "not doing reward shaping" --- if the thing you actually care about is human well-being, and you say "maximize profit" or "cure this disease" you are doing reward shaping in some sense. Sigh.

In some sense this means that getting RL dudes aligned is a tricky political problem?
Like even if there was a github repo with CEV sitting in it, maybe this reward signal is too weak to learn from, so alignment tax is too high.

## Takes on AI capabilities at coding:

- I got much better at prompting
	- the key is to ask for relatively small diffs when possible.
	- it's totally down to right 600 LOC for you 
		- but this usually doesn't work out too well

- it also has good ideas for fixing bugs / perf issues / learning issues

- i mostly only coded via prompting, 
	- but still felt that i needed to read code sometimes

- Poor Claude cannot run its own code 
	- unless you set it up as an agent with vivara which is a hassle
	- actually cursor supposedly has something that lets you do this -- possibly worth trying sometime

## Meta research takeaways

- My research should probably never involve actually training AI systems.
	- I thought that a simple toy thing like this would be fine, 
		- But it appears that even this was too much. 
	- This obviously stops being true once Claude is actually a drop-in replacement for a human researcher. 
	- Like there could be a situation in which I propose research directions for an AI to do, and then it does them. 
	- But standard concerns apply:
		- Easy to get fake research that looks good 
			- either bc the AI is malicious
			- or just bc this is really easy to do by accident too

- I'm going to have to be creative about figuring out how my empirical projects are going to go. 
	- I started this project as "oh i'll spend an hour to get a toy grid world set up where I can test my weird FDT question"
	- 30hrs later, I have learned that I need to start simpler, or use someone else's code for something. i bet there is a simple checkers / maze solver DQN agent online

- If I had people that I talked to about my research on a daily basis -- kind of like a "lab meeting" then someone would've told me to abandon this project earlier than I did probably.
	- In the past I've errored on the side of giving up on things too early 
	- Here I think I over-corrected. 
	- Or I just got stuck on some detail that wasn't central to the actual questions that I wanted to investigate.

---
# action items

- figure out next project!
- make a "lab grp" 
	- prolly just means find someone who's willing to spend 15mins/day helping me set research agendas. and to be accountable to.


alek out