This document summarizes discussions with Nathan Sheffield (henceforth denoted by N; not to be confused with "Neural Net", abbreviated NN) about "solving AI safety". I'll probably use the word "I" throughout the document, but suspect N would endorse many of the things that I say.

yes i know that this is a big problem -- guess it calls for a big blog post. 

**Meta research strategy comment**

I want the future to be [[goodness(universe)|good]]. In order for this to happen, we need to figure out some solution to the [[Risks from AI -- elevator pitch|AI problem]]. This is a big problem. Some people have spent ~10 years thinking about it, and have identified some sub-problems which seem helpful for solving the original problem, while being potentially more tractable. The most obvious way to work on AI safety is probably to choose someone who seems smart, and start thinking about some node lower in the tree that they've identified as helpful. There are clear benefits to this approach, such as getting started closer to concrete questions where its clear what progress looks like, and the fact that plausibly it's better to thoroughly investigate a couple approaches then partially investigate a bunch. 

Another approach is to start by just staring at the problem, and then try to figure out how to break the problem up yourself. This also has some benefits. Hopefully by doing this, you'll discover why people thought certain parts of the tree were important to look at -- and conclude that certain parts of the tree aren't really worth looking at. Doing this exercise seems pretty important for being able to generate good concrete questions to resolve the ultimate question we care about.

More research methodology notes:
- We will try to move down the tree as rapidly as possible.
- Our main method for doing so is making strong simplifying assumptions.
- Once we make enough of these, the problem will generally either become trivially true, or obviously impossible. Rarely, the problem might become interesting (i.e., not obviously true or impossible).
- The plan is to rapidly backtrack out of impossible branches of the tree, and to similarly backtrack out of branches of the tree where our assumptions are too strong, so that the problem no longer tracks reality.
- we will aim for a mathematical theory of safety
- that is, we're not allowed to make claims like "the inductive biases of this NN will probably cause X to happen", unless we can export this to a formally specified axiom that we can assume
	- once we prove safety conditional on some axioms, we will assess whether or not these axioms reflect reality, as discussed above.

---
## 1. Some basic definitions

> I would like to construct an **efficient** **optimization algorithm** over **neural networks** that outputs **useful** and **nice** NNs. I'd then like such a NN to be **built and deployed**. I think that if this happened, it'd increase the probability of the **future being good** (compared to the baseline of everyone dying in 0-10 years).

**definitions:**

efficiency and usefulness measure the "alignment tax" of our alignment strategy.

**efficient:**\
whatever we do must be possible in polynomial time.
in fact, it's even problematic if our solution costs a large constant-factor more than the algorithms  people would otherwise use.

this is mostly an engineering problem, and we'll ignore it for now.

it may even be useful to start by relaxing the efficiency assumption and ask what we would do existentially.

**useful:**\
whoever trained the ai probably had some primary goal, beyond just making a nice ai.
I'll operationalize this by saying that the AI must cause money to end up in openai's bank account.

**optimization algorithm**\
this generally means something like gradient descent or policy-improvement (RL).
some kind of iterative procedure.

it feels like it might be important to understand how this training process works. 
I guess SLT is the community that talks about this? 
Maybe we could come up with our own way of talking about training.
Potentially [[online convex optimization notes]] could be relevant.
I think our plan for now is to ignore this until it becomes very obvious that we need to assume something about the training process.

**neural networks**\
A neural network is some gross function.

we can generally abstract this away to being a function $f:\{0,1\}^{n}\to \{0,1\}.$
for instance, an auto-regressive LLM approximates the function $\Pr[x_{n+1}=1\mid x_n, \dots, x_1]$.

there are two settings you can think about, as described in [Paul's blog post](https://ai-alignment.com/low-stakes-alignment-f3c36606937f).
- The low stakes setting
- The high stakes setting

Paul's post says that if we had a good objective function (by which I think he approximately means if we could solve ELK) then SGD-ing this would actually work (namely, we would achieve bounded regret) if the AI needs to take a large number of actions to cause a catastrophe. 

**a good future**:\
in [[goodness(universe)]] i've written some fluffy poetic description of what i like about the current universe and hope for the future universe. i don't feel very comfortable giving this to an ASI and telling it to optimize it -- nor is there an obvious way to do so (beyond "asking nicely", which circularly assumes alignment in the first place).

the best attempt I've seen at defining "a good future" is [Paul's blog post](https://ordinaryideas.wordpress.com/2012/04/21/indirect-normativity-write-up/) . the gist of the idea is:

The goodness of the future is a value between $[0,1]$ computed as follows:

> Let Alek think about the future for 100 years, with all data and help that he needs. he then gets to assign a score to the future based on how much he approves of it. 

- it seems pretty tricky to code this objective function up. 
- it also is pretty expensive to get good labeled data for this function.
- if we approximate this function by asking Alek to write down on a slip of paper whether or not he approves of the robots actions, then there is a predictable failure mode. in general the "pointing to things" problem seems tricky. 

as discussed in [[AI xrisk]], I think it's pretty unlikely atm that the future looks good. 
as stated there, the main reason i think this is because i feel that humans are really vulnerable -- at a high level of tech (eg current level) its not so hard to wipe ourselves out / get wiped out by autonomous tech that we develop.

if we had some strategy for building a nice ai, that was competitive, and we thought was likely to be adopted, then i think this'd increase our chances. 

One big concern about working on AI stuff is that it'll accelerate the rate of dev of dangerous tech, and so even if we don't get wiped by AI, we'll get wiped by something else. Under this view, we should focus solely on achieving global coordination. [Paul discusses here why he thinks alignment is still important](https://ai-alignment.com/handling-destructive-technology-85800a12d99#.7q9gvxl0p) despite this consideration.


**nice**:\
a nice AI is one that tries to make the future good. 

for the most part im okay with operationalizing this as doesnt annihilate humanity / do anything that id consider equally bad or worse (if i knew all the facts and could reflect). 

**built and deployed**:\
having an awesome alignment strategy isnt enough. 
you need it to get adopted. 

there's also a question of how universally it needs to be adopted. 

some people, (like Boaz Barak on his blog), have suggested that "humanity can survive an unaligned ASI" as long as we have some nice ASI to protect us. Boaz's claim seems to be based on the observation that really unaligned people (terrorists / Kim Jung-Un / Stalin / Hitler etc) haven't been that good at killing people (which is factually incorrect for the last two at least but whatever). My guess is that Boaz doesn't just doesn't understand "anthropic bias" -- the reason we don't live in a world where there are widely available easy ways to cause massive harm is because there are no observers in that world to observe that world. my understanding is that there's compelling reason to believe that near-future tech could give really large advantages to attackers compared to defenders. for me this is intuitive just from the observation that we care about matter being arranged in a **very very** particular way, but there are lots of ways to arrange matter which don't contain humans. humans need food, are susceptible to disease / biological attacks, dont last v long if you shoot them or nuke them, etc. 

you could hope that there is one project, say openai, that obtains a "decisive strategic advantage" compared to other ai projects in the next couple years -- and that if they made a nice AI it could then prevent the creation of other not nice AIs. this seems like a much better way to defend against not nice AIs than "letting them be built" and then trying to cover some massive attack surface. 

it's not clear to me that there will be a single project with a decisive strategic advantage that can stop prevention of other similarly powerful AIs. 

its quite possible that, even if there's a pretty large gap between two AIs capabilities, an attacker still has enough of an advantage that they still win.

one reason to be concerned about whether a solution will be adopted conditional on its existence is this is that there are lots of problems where humanity has good evidence that there's a problem, and has some viable solutions, but doesn't implement the solutions. for instance, 
- climate change
- gain of function research on coronaviruses --- scientists are still working on this after covid19 -- if you dont want to develop powerful synth bio viruses, then maybe dont do it is a good approach.

some reasons to be optimistic about this:
- it seems at least fairly likely that there will be 1-2 nationalized projects that have fairly decent leads
	- pre-training is rlly expensive, 
	- there's been some open source success at post-training
	- but hopefully at some point someone will realize that you don't open source docs on how to build nukes, and that we shouldnt opensource code for building powerful AI either 
- there are some reasonable ppl that might listen if you have a good alignment strategy that meets all the above criterion. for instance, ai safety community is fairly well connected to AI labs. maybe ai safety community couldn't get ai labs to slow down, but they could get them to adopt a fairly cheap alignment strategy.

> [!tip] summary
> I want an **efficient** **algorithm** that creates a **useful + nice** **NN**, and then I want this algorithm be **adopted**.\
> I believe this increases the probability of the future being good.

> [!caution] Assumption 1
> I'm going to ignore the issue of "getting the strategy adopted" for the rest of this post.\
> More precisely, I'm going to assume that 
> 1. there's a single international AI project, 
> 2. whoever is in charge of it is down to adopt whatever strategies I propose, as long as their alignment tax is small, 
> 3. whoever is in charge of the project isn't evil / it's not an issue if my alg advances ai capabilities / makes it more possible for someone to be an eternal dictator or whatever .
>    
>    
> I'd be excited to see ppl try to remove this assumption. I may as well in the future.

> [!caution] Assumption 2
> The first ASI's will look like some kind of transformer / RL thing -- discussed here [Prosaic alignment](https://ai-alignment.com/prosaic-ai-control-b959644d79c2).
> This seems extremely likely to me, and is what I care to focus on. 

okay so this is kind of a big / tricky / confusing problem. 

let's make some assumptions and flesh out some details to get crisp easier problems!

## 2 relaxing usefulness

If you don't require the AI to be useful, "just don't build ASI" solves the problem.

I think having an effective global pause (while also pausing hardware and algorithmic improvements) would increase our chances of success --- please work on global coordination!

as a side note, i wonder if i could somehow "go viral" in china by talking about xrisk -- or in the us. 

anyways, is seems like there's a pretty low chance of this being a viable solution. 
so we can't relax usefulness.

[this is discussed a bit more here](https://ai-alignment.com/prosaic-ai-control-b959644d79c2)

## 3 relaxing efficiency vNathan

seems like the first thing to do is relax efficiency

do we have some way in theory of pointing to what a nice + useful NN is?

quick summary of discussion with N about this. 
i think i waited too long to write it down so its fairly distorted. 
will improve after thinking about it more and next discussion

#### 3.0 what is goodness?

we had two ways of cashing out goodness:
- something "verifiable" -- i.e., assume existence of a "catastrophe detector" (altho we can't filter all model outputs using this due to cost I guess)
- assume goodness is "simple" / has "small description" / is "smooth"

#### 3.1 suppose goodness was simple

we have some class $F$ of functions that we care about.
we assert the existence of a nice + useful NN $f\in F$, and we expect $f$ to be "simple".
we'll aim to learn $g\approx f$ from some training data $D$ consisting of some $(x, f(x))$ pairs.

i think we were assuming that we can't craft synthetic inputs but instead that we just have some data lying around.

the way that we intuitively were thinking of this is that $f$ is going to be an LLM, and it gets as input a user query, and it's catastrophic if it ever responds with instructions for building a supervirus.

anyways, 

> Suppose that there is some set $F'$ of functions which fit the training data. 
> Suppose that among these, the one with the smallest circuit is the one that generalizes to being nice off distribution or whatever. 
> Then we could look for the smallest circuit that fits the data and win. 

> you could do something similar if you thought that good NNs were likely the best fit to data. 

## 4 Paul's decomposition of the problem

We'll break into two cases -- suggested in a Paul blog post.
### 3.1 low stakes setting

> **low stakes setting**: any particular action can only cause a small amount of harm. so it's okay to train on bad stuff that happens and then update the model.

I took some notes on the low stakes setting [[low vs high stakes alignment|here]].
### 3.2 high stakes setting

> **high stakes setting**: the AI can cause a catastrophe with a small number of actions (e.g., 1).

Paul gives some definition of high stakes alignment [here](https://ai-alignment.com/learning-with-catastrophes-59387b55cc30), which I took notes on [[low vs high stakes alignment|here]].


### 3.2.1 discussion with Duru

[defining C? via amplification](https://ai-alignment.com/informed-oversight-18fcb5d3d1e1)


----

# Some problems to keep in mind

Here is a list of some problems that I expect we will need to face at some point. 
They'll get incorporated into the appropriate sections as needed. 

We should be skeptical of approaches that don't address these issues, and of simplifying assumptions that get rid of these issues. 

- Dealing with the fact that train and deploy distributions can be **really different**.
- deceptive alignment
- "generalization"
- multiple things fit the data
- defining in what sense we expect "goodness" to be "simple" 
- dealing with the fact that 
- handling the possibility that evilness is simple (evan's example)


----

## Some things I'd like to read

- https://www.lesswrong.com/s/EmDuGeRw749sD3GKd