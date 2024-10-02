#technical
In this blog post I will summarize a paper by Hadfield-Menell et al. on inverse reward design.

This is a strategy for trying to solve the problem of reward miss-specification.
The basic idea is, when the agent encounters a situation that is different from what it say during training time, it should notice this and admit uncertainty about the reward it will get from acting in the situation.

They claim that their methods fixed some reward hacking problems.

Here's the difference between this and inverse RL:  
- in IRD the agent observes some reward labels and is supposed to figure out in new situations when it's not sure about the reward
- in IRL the agent observes someone demonstrating optimal behavior and is supposed to learn from that

This seems pretty reasonable.
