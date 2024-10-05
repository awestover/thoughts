This is mostly just a way of keeping track of progress here. 

**todo: read this**: https://transformer-circuits.pub/2021/framework/index.html

# chapter 0: fundamentals

### CNNs
Just got some practice with pytorch / what is a module.

# chapter 1: transformers
**#1 Transformer from scratch**
Transformers are actually pretty simple. 
The main idea is "residual streams can carry information around".
Attention allows you to copy paste info from other residual streams.

**#2 SAEs**
Model represents more than $n$ features in $n$-dim vec space. 
How does it do this?
Intuition (thanks to Gabe for sharing this with me): in high dimensions you can have lots of "nearly orthogonal" vectors.

It's pretty clear why this is bad for interpretability.

> Q: Why would the model do this?\
> A: More features more power.

I've written about this in more length in [[superposition]].


# chapter 2: RL
**#1 Intro to RL.**
It was basically just the first couple chapters of [[intro-to-rl]]. Talked about bandits, tabular RL, and DP / monte carlo /exact techniques for solving MDPs. 

**#2 qlearning + DQN**
