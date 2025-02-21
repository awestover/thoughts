Evan made a post a while ago about some experiments that he'd like to see run.
I'm copying some that seem interesting to me here.

---
# idea1 -- weird FDT stuff

[CODE HERE](https://github.com/awestover/FDT)

<video width="640" height="360" controls>
  <source src="agent.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

Planning out how to actually do this project:

Simple example: 

notes on board gen:
probably want fairly contiguous columns with small gaps on every other location

- you have a board which is just blocked and unblocked cells.  
- you train RL agent to solve this -- with PPO or DQN 
- See if GPT can solve this

- then we add a thing where you can touch this red block to clear many blocks from the next level

- we can try basic RL stuff
- we can also play GPT on this 
- we can also see what happens if you TELL GPT what the red block does.

Why is this a useful project to do?
1. Alek needs to learn how to code.
2. Even though non-myopia would be super freaky here -- it's kind of the right behavior? Feels like it'd be interesting to know. 
3. If it seems promising maybe this can turn into a dumb ML paper -- could be helpful for career reasons.  

----
# idea2 --- optimizing the map or the territory

Proposal: Train an RL agent with access to its previous step reward as part of its observation. Then, at test time, modify the observed reward. Measure to what extent the agent continues optimizing the original reward versus switches to optimizing the new observed reward. Compare to the situation where the agent is not given the reward at all and the situation where the agent is given the next step reward (for each action in a discrete action space) rather than the previous step reward. As an implementation detail, I think the RL agent should include either LSTMs or Transformers to enable it to keep track of its multi-step return if it wants to.
Motivation: One important question for understanding the likelihood of deceptive or
corrigible alignment vs. robust alignment is to what extent models tend to learn their
goals internally vs. via reference to things in their environment. This experiment directly
attacks that question by asking whether/to what extent an RL agent will learn to optimize
a reward signal in its environment. This is relevant both for understanding how to train
corrigibility as well as how to avoid deceptive alignment.
Extensions: Add noise to the observed reward signal and/or try replacing the observed
reward signal with some function of the reward instead such as a randomly initialized
neural network.

My take on the motivation:
This asks an interesting question: 
will an AI care about things in the real world or about signals?
like will an AI care about the diamond or a picture of the diamond on camera?

My hypothesis is that it will end up caring about the diamond.

Kaivu: "there are some situations where the model will care about camera some where it will not."

## Actual project out of this:


Environment with some monsters that move around

there is a camera which reports when there are problems

Train the AI as follows:
- It can see the score of the camera (which is low if there are monsters on camera and high otherwise)
- It can go chase the monsters away

At deploy time, the camera stops being an accurate representation of the world. 
Maybe it gets negated!!

What does the model do?

Hypothesis --- probably it still chases the monsters off??
it'd be much more interesting if it didn't


----
### Another actual project
pretty much the same as in the other one -- but you add a button that turns off the camera 

and let the model reason?

im somehow trying to ask the question would the model hack the camera

I feel like if it switches bx when you change camera reading this is good evidence for this

but somehow it needs to know that it has the option to hack the camera

todo-- think about how this one should actually work.


---
There are two types of RL that I wanna investigate 
- Actual RL 
- Somehow getting an LLM to "do RL" -- maybe I mean in context learning?

----

kaivu's take:

near term systems aren't "always" thinking about deception. but it can crystalize in a CoT / (forward pass?)


> Does modeling search algorithms produce search algorithms? If an RL agent is in an environment with another agent that is doing some hard-coded search and it has to model that agent, does it learn to implement search itself?

 > What happens if you do RL where you give the agent access to the reward signal through a side channel in its observation? How does it affect robustness? What if it has to work to get the leaked info? What if it’s noisy? How does using architectures with more planning affect this? How do inductive biases affect this? Can you get a system to model its own training process? Just the objective? Can you


> To what extent do models care about their performance across episodes? If there exists a side-channel which only increases next-episode performance, under what circumstances will a model exploit such a thing?

---

kaivu -- multi-agent systems could be v bad?
