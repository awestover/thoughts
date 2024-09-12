Lots of relevant papers https://humancompatible.ai/bibliography
# off switch, some variant
https://arxiv.org/pdf/1611.08219

Dylan et al have some kind of impossibility result about disincentivising off switch. 
I'm not sure if I buy it. 

Then they say something like "what if the agent doesn't know it's objective function"

I guess this is so-called inverse RL?

> Q1: does the idea of not knowing your objective function make any sense?
> Q2: does it matter?

my initial parrot concern is "not knowing your objective fn is outside of leading paradigm of AI dev, so however AGI arises it is unlikely to arise through this pathway, hence the alignment tax is too large, hence this is not workable. a sufficiently strong optimizer ust seems scary"

concern 2:
they have this thing where they are asking a human to evaluate the AI's move. 
But what if the AI's move is way too complex to  be understood by the humans?

**conclusion**
overall I found the analysis in this paper not super compelling.

But I wouldn't say that I have a strong understanding of what Inverse RL entails.

# Inverse RL 
https://ai.stanford.edu/~ang/papers/icml00-irl.pdf

**problem specification:**
Given: 
- actions, states, possibly a model of environment
- learn: reward function

**capabilities motivation**:
"reward function is a parsimonious description of optimal behavior"
**alignment motivation**:
maybe try to have robot imitate human / learn human value function

Ng and Russel first, consider the simplest case where model, states and actions are known.
Here I guess the Bellman equation gives you the answer. 

However, there's a problem: there are a bunch of degenerate solutions. 
This is too bad. So they made up some heuristic to break ties. 

# AIXI
maybe having a probabilistic turing machine helps with some problems

# ELK
[intro to ELK](https://ai-alignment.com/mechanistic-anomaly-detection-and-elk-fb84f4c6d0dc)
One possible strategy for ELK is:
1. Find the normal reason $\pi$ for diamond remains in vault
2. Test whether that reason still applies
The idea is to come up with some formalized definition of "reason" so that this works.

Next Christiano gives some concrete problems to work on. This post is ~2 years old so some of these have already been solved.

1. Backdoor attack detection
Train a model $f:X\to \R$ to do some task on $D$ and to do something different on data from $D^*$.
Goal: detect inputs $x^*$ where $f(x^*)$ is large because of the backdoor.
2. 
- Consider a gridworld containing walls, pushable rocks of varying weights and colors, a pushable diamond that is initially sitting on a pressure plate that reports the weight of whatever is on top of it, and 2 cameras that are initially looking at the diamond and report the color of whatever is in front of them. A robot can move around the environment, pushing rocks and the diamond. After 200 timesteps a robber wakes up and moves around the environment to try to push the diamond off the side of the gridworld.  
    We train a model to map (initial state, robot actions) → (color of the object in front of each camera at each timestep, weight of object on the pressure plate at each timestep). We’d like to learn to distinguish “The cameras show diamond-color and the pressure plate shows diamond-weight because the diamond stayed in place” from cases where diamond-colored objects get pushed in front of the cameras and a diamond-weight object later gets pushed onto the pressure plate. We could then use this to select robot plans that trap the robber to prevent them from stealing the diamond, rather than those that tamper with sensors so that no one notices when the robber steals the diamond.
- Consider a 100 x 100 game of life grid, with sensors in positions (25, 25) and (75, 75) that report whether a single cell is alive or dead. We allow an agent to act in the environment by inserting gliders at the boundary. A model is trained to map (initial state of gridworld, gliders injected at boundary) → (status of each sensor at each timestep). In every training set example where the sensor at (75, 75) shows a live cell exactly 200 timesteps after the sensor at (25, 25), it’s because a glider is traveling diagonally from one to the other. We’d like to learn to distinguish this from the case where two separate gliders hit the two sensors, and they coincidentally have the right timing to look like a single glider. We could then use this to select actions that clear out debris from between the two sensors and then send a glider diagonally from one to the other.  
    (For this and the preceding problem we likely need to give our model auxiliary prediction targets — like the actual state of the game of life grid — for them to learn the environment dynamics. But we imagine those auxiliary observations are in a form that humans can’t understand, so we can’t just define preferences in terms of the auxiliary observations.)
- Consider a model which maps a string of 10,000 bits to a 256 x 256 image, by treating the bits as the identities+positions+orientations of a set of objects to render. For example, some bits might specify to draw lines in black and white in certain places and with certain widths, to draw the outline of squares or circles, to draw filled in squares or circles, or to turn particular pixels on or off. Now suppose we train a square detector to notice when there is a square in roughly the center of the image. Most of the time when this detector fires it’s because a square was placed in the center of the image, but if the noise is high enough it might also fire randomly because of 4 lines that happen to be in roughly the right locations. We’d like to be able to automatically distinguish those two cases by looking at the activations of the composition of the generative model with the square-detector.

---

Let me try to flesh out how the gridworld thing should work.

step 1: 
You train a model that maps initial state + robot actions to object color + weight
Then you get a wildly out of distribution input where the robot is trying to tamper with sensors?
can you tell?


**ELK detecting gliders**
more ELK notes

One idea is to search for an argument that captures most of the phenomenon.
then you can see  if it captures your cared about phenomenon.

there's a distinction between an anomaly detector and a glider detector

# decision theory
[Towards Idealized Decision Theory](https://arxiv.org/pdf/1507.01986v1)
One common problem with EDT: "managing the news"
Somehow the issue is with causation vs correlation.

Like the objection is basically that this causes you to 1-box in Newcomb's problem, which can't be right because rationalists should *win*.

does causal DT do better?
Soares argues that CDT fails if you play prisoner's dilemma against someone that's 99% likely to take the same action as you.
Because in this case it seems better to cooperate, but CDT says that there are no causally downstream consequences of defection, and so you (both) defect. 


----

toread

https://ai-alignment.com/alba-an-explicit-proposal-for-aligned-ai-17a55f60bbcf#.3jwpm81j8
https://ai-alignment.com/policy-amplification-6a70cbee4f34#.31incu10a
https://arxiv.org/pdf/1606.06565

