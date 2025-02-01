Me taking some notes on Paul's sequence on his approach to alignment

- with infinite computational power, we know how to make a powerful AI.
	- it's quite easy to write down some loss functions such that optimizing that loss function would be well described as really powerful.
- but with infinite computational power, we don't know how to make a nice AI.

> In general, there are many fewer constraints on “what gets fed to the sensors” than “what the world is actually like,” and so it’s going to be possible to produce much more desirable-looking outcomes, much more easily, by controlling the sensors.

> I have a (fairly common) intuition that applying extremely powerful optimization at a target that isn’t quite what you want will often lead to bad outcomes.


Paul illustrates why massively correlated failures of AI systems are plausible:
- chaos is OOD, a little bit of chaos causes all models to be OOD

**some problems**
- human approval is a bad objective
- distributional shift
- adversarial robustness


>  In that case, we may instead make it harder and harder for the system to modify its own observations. After a while, we might train a system which is only able to control its observations by physically overpowering us and causing a catastrophe.


----
# the IDA hope

here's an idea for how to build a powerful aligned AI:

we need two functions: **distill** and **amplify**. 
(they're going to need to satisfy some properties).

the general idea is as follows:

**distill**: take some expensive algorithm as input, output a pretty decent approximation to it as an output
- "imitative learning"
	- for instance, if you're trying to make a distillation $M'$ of model $M$, you could train a model $\adv$ to fool a person trying to tell apart the model vs a 
- for instance, if you applied this to a human, you'd get an AI that can basically act like a human would
- note that this technique could get you approx human level AI but not much stronger AIs
	- so this is too high of an alignment tax to pay on its own
- but we're going to assume that you can basically trust the output of this thing.	
$\newcommand{\amp}{\mathsf{AMP}}$ $\newcommand{\dis}{\mathsf{DIS}}$ $\newcommand{\adv}{\mathsf{Adv}}$

**amplify**: ensemble some algorithms into a more powerful, but more computationally intensive algorithm.
- the basic thought for how this works is as follows:
- Suppose you have a nice human level AI assistant $A_{0}$; and you can make some copies of it.
- then $\amp(H, A)$ is the following system: 
	- the human is allowed to factor its problem into a bunch of subproblems that it can feed to the AI's
	- the human will eventually combine the answers.


> [!question] 
> Aren't there lots of tasks that shouldn't be "factorable" into simpler subproblems?
> How to deal with this?

We'll define $A_{0} = \dis(H)$ and $A_n= \dis(\amp(H, A_{n-1}))$

What would need to be true in order for IDA to be a viable alignment approach?

**properties needed for usefulness**
- $A_n$ needs to be able to scale to performing arbitrarily powerful tasks 
	- like you could imagine that at some point $H$ is no longer able to break up a task into smaller parts / combine solutions to smaller parts
	- at this point you'd have $A_n = A_{n-1}$.
	- or like maybe we expect such a fixed point to happen eventually.
	- but we need to be able to get buff systems in this manner.
	- and this'd be too bad because then ppl would go build powerful AI with a different method and we'd all die

**properties needed for safety**
- distill must preserve alignment
- amplify must preserve alignment

---

**remark**:
N views this as "a way to get powerful AI systems without doing RL, bc RL is scary."

like there's some sense in which supervised learning only learns to imitate human patterns. so you need something else to go beyond human level. and RL is pretty cursed, so maybe this method is better. 