I think **mechanistic anomaly detection** (MAD) is an important open problem relevant to AI safety, which might be tractable. In this post I'll try to: 
- Define MAD.
- Explain why solving MAD would improve AI safety.
- Summarize approaches that people have been trying to solving MAD, and the limitations of these approaches.
- Propose some concrete mathematical conjectures that, if proven or refuted, would constitute progress towards solving MAD.
- Make progress on these problems.

If you're in the Boston area and interested in collaborating on this agenda, please reach out -- I'd love to talk. I plan to look for some people to work on this once I've made progress on defining the problems.
$\newcommand{\G}{\mathbb{G}}$ $\newcommand{\E}{\mathbb{E}}$ $\newcommand{\L}{\mathcal{L}}$

----
# 0: What is MAD?

> [!caution] 
> MAD is *different* from detecting "off-distribution" **inputs** or "weird outputs".
>
> MAD is about detecting weird **internal mechanisms** when processing an input.

The rational for this is as follows:
- First, I'm concerned about the case where the output "looks pretty normal (or pretty good)" but for a bad reason -- e.g., sensor tampering, or hacking a data center to put nice numbers in it, etc. If the output looked really bad, then the AI would get high loss, which it might not want.
- Second, tons of inputs in deployment are probably somewhat different than what you got during training. If our solution involves flagging a large amount of behavior as suspicious, this is not an okay solution.
- Also, I'm interested in the "worst-case" behavior on particular inputs, so talking about the distribution that the input is drawn from doesn't really make sense. 
- Finally, my prototypical example for a mechanistic anomaly is as follows:

> [!example] 
> NN computes $f(x) = [x=2^{17}] \bigvee \left(\sum x_i \equiv  0 \mod 7\right)$, where $x\sim [2^{20}]$.\
> Here $2^{17}$ is not an "off distribution input", but it is an input where the activations of the model should look quite **abnormal** and so $f(x)=1$ is a mechanistic anomaly here.

More generally, I'd really like to catch the following cases:
- The model has a little "if statement" that says something like
	- "If year=2027, kill all humans" (or maybe more trickily, if input=factorization of RSA-2048)
	- or, "If I roll a billion sided dice and it comes out as 1, kill all humans".
The idea is, a model should only be allowed to perform egregiously pretty rarely, or under some specific condition (or we'll catch it during training and then we'll not deploy that AI).

For building intuition, here are some other examples of what MAD could look like: (from a post by [Paul](https://www.alignment.org/blog/mechanistic-anomaly-detection-and-elk/))

> [!example] 
> Suppose NN computes $f(x) = A(x) \lor B(x)$, and $D$ is a distribution where $A(x)$ happens sometimes, but $B(x)$ doesn't happen (or happens rarely enough that we won't see it during training, but will during deployment).
> 
> Goal: an explanation $E$ such that $E$ explains $f$'s behavior on $D$, but not on some weird input where $B(x)\land \neg A(x)$.
> 
> - $f$ = “A character in a story screams”  
> 	- $A$= “the model thinks the character is scared”  
> 	- $B$= “the model thinks the character is excited”  
> - $f$ = “Asked whether at least one of <proposition 1> or <proposition 2> is true, the model says ‘yes’”  
> 	- $A$= “the model thinks <proposition 1> is true”  
> 	- $B$= “the model thinks <proposition 2> is true.”  

> [!caution]
> There is a technique called **probes** (proposed by Anthropic) that kind of addresses a similar situation, but it does not solve MAD (nor do I think it can be easily adapted to solve MAD).
> 
> A **probe** is a simple classifier that you train on the activations of a NN.
> The prototypical example of why a probe might be good is: 
> - Train a probe on examples of a model lying and telling the truth.
> - This gives a classifier that we can later use to tell if a model is lying.
> 
> I think probes seem like a pretty reasonable approach to distinguishing between "model does X for reason A" and "does X for reason B" in cases where you have a bunch of labelled examples of model doing $A$ and $B$.
> 
> But it doesn't even make sense to talk about having a probe for a weird behavior that you haven't seen before. 
> If you trained a probe to distinguish between $f(x) = 1$ and  $f(x)=0$ where $f(x)=A(x)\lor B(x)$ on a distribution where $B(x)$ is always false, then there's no reason to why this probe would necessarily stop firing on an input with $A(x)=0,B(x)=1$. 
> The general approach that we usually have to solving this problem is "complexity penalties" that will favor dropping the $B(x)$ thing, but this doesn't make sense for probes.
> 

**Remark:** Just solving the above concrete problems empirically would already be quite neat! 
Those above problems are pretty nice because it's possible to know if you've succeeded or not.
Hopefully, we can turn the theoretical problem of getting a general solution to the problem into a similar format where we have a crisp problem statement and we know how to verify a solution!

Specifically how you'd set up this task is: 
- You make a distribution D, where sometimes the model does $X$, and sometimes not, but it's always for reason $A$, never for reason $B$.
- You find an explanation for why the model sometimes does $X$ on inputs from $D$.
- You evaluate your explanation on inputs from $D$. 
- You evaluate your explanation on inputs where the model does $X$ for reason $B$ --- hopefully, your explanation does not explain why the model performs action $X$ on these inputs!!

# 1: Why Care About MAD?
Plausibly if your AI model usually acts normally (e.g., during training) but then wants to act super egregiously at some later point (e.g., a takeover attempt), then there are **different mechanisms** driving these two behaviors.

The classic example of this (from the ELK report) is to distinguish between the following two mechanisms (with reference to a diamond security system):
1. Mechanisms that cause the AI to get low loss because the diamond is in the room.
2. Mechanisms that cause the AI to gets low loss because it tampered with its sensors to make it look like the diamond is in the room.

The hope is that the activations in a model "look really weird" when it's doing something crazy.

There are definitely some things to be worried about here:
1. Can we flag egregiously bad behavior without having a large False Positive Rate?
2. What do you do if you notice a mechanistic anomaly? (Turning off your AI might itself be catastrophic if you were relying on it for something important).

My intuition is that MAD should be possible, if not "in the limit", at least for fairly powerful AI's.
My intuition is also that solving MAD would be substantially improve the "AI situation".

# 2: High-Level Approaches For MAD
In this section I'll give the high-level approaches that people have being thinking about for addressing MAD. I'll get into the details, and problems, in the next section.

> [!tip] Idea 1 -- Downgrading
> Some method of "**downgrading**" your NN by throwing away some parts of it, making it **much simpler**, while still having pretty reasonable performance on average. 
> 
> Hopefully the downgraded network does a good job of predicting the loss of the NN usually, but when the network gets low loss for an anomalous reason, the scrubbed network **shouldn't** predict this -- and we flag this as an anomaly.
> 
> There are two ideas for hashing this out -- "heuristic estimation" and "causal scrubbing".

> [!tip] Idea 2 -- Distilation/Smoothing
> Get a simpler *but possibly unrelated* version of your NN that has similar average case performance. Hope that it doesn't replicate behavior on anomalous inputs.

> [!tip] Idea 3 -- Backdoor Detection
> Have some way of deciding what "weird" inputs to your network are.
> I think of this as trying to notice if you're in a weird conditional if branch of the network, in some abstract sense.

I think all seem like somewhat reasonable approaches. 
# 3: Detailed Approaches, and Issues
Next I'll summarize attempts to formalize this, and some issues with current methods.
### 3.1 Downgrading via Causal Scrubbing
Causal scrubbing (due to Redwood) is (supposed to be) a principled method of performing ablations. The idea is basically that we're going to identify a couple of important causal structures / correlations in the activations, and then claim that everything else is basically just acting independently / that we can forget about the structures not captured in our "explanation".
I've described this a bit [[Causal Scrubbing Notes|here]].

> [!bug] Problem 
> One issue is that, for instance, random functions are not compressible.
> If you want to non-trivially reduce the complexity of a random decision tree for instance, you will completely lose all of your ability to predict it. 

There are a lot of open questions here:
> [!question] q2.0
> - How would you find such an explanation?
> - Can you efficiently test how good an explanation is?
> - Are there even nice small explanations for things?

> [!bug] Concern 11
 > One concern is that while causal explanations of model behavior seem to make a lot of sense in some abstract feature space, do they really make sense in the basis that data is stored within a NN?
 > Some mild reason for optimism is the notion of **natural bases** [[superposition]] (the idea that there might be some pressure for features to be squished into individual neurons), and the fact that SAEs are maybe kind of okay at picking apart interesting concepts in LLMs. My vague understanding is that it's not so unreasonable to hope that there's a single neuron responsible for detecting curves of a single type. Yes we have tons of polysemanticity -- and the mech interp ppl are working on this maybe -- but features being spread across a bunch of neurons seems less common.
 > 
 > But this is still kind of concerning.
 > 
 > okay, here's the deal:
 > 1. I think scrubbing authors mentioned that some kind of "creating new nodes / splitting nodes" in your NN might be necessary to make it admit good interpretations.
 > 2. It really should be simpler to check whether banana is in the text than to be an LLM. That's why a distillation should exist. 
 > 3. Finding it is a problem that I'm deferring for now. Let's just suppose that it was handed to us --- what'd we do then?
>  4. But for the record maybe you could find it by ablating parts of the model and seeing what stuff changes the answers and what stuff doesn't! :O.
>   Or something. idk


> [!bug] Concern 15
> Not clear that verifying a solution is easier than generating a solution: 
> maybe a solution you generate yourself you can trust to not have subtle backdoors or whatever.


The biggest theoretical difficulty in my opinion is 


> [!question] q2 most important part
> **How could you inefficiently test how good an explanation is**?
	> - (The authors of [[Causal Scrubbing Notes|scrubbing]] mentioned several problems with their current approach, such as the fact that dropping considerations can help)
		> - they propose looking into add/join operations as a fix, but it's tricky

The authors mentioned three problems:
1. Suppose there is an inhibitory part of your circuit; if you ignore both an inhibitory part of the circuit and an activatey part of the circuit, then these can maybe cancel out. 
2. Splitting an error term and forgetting to correlate them reduces variance. 
3. Sneaking in information can improve your explanation.


> [!bug] Concern 14
> (1) + (2) seem like similar problems --- you're explanation is too good because you're ignoring a correlation. 
> (2) is just not very nice. (1) seems maybe fixable by the trying to add explanations move. but as the authors state, this seems rlly hard. 

(3) seems the most egregious, so I'll try to fix it first. 
Ok so I think the problem with (3) is as follows:

Naively you might expect that ablating a model based on any supposed causal structure always lowers the model's performance, so to find the best explanation for the model's performance you should search for the explanation such that ablating by that explanation does a good job at recovering most of the loss of the original model.

Unfortunately... ablating a model can actually improve it's performance in some settings. Sigh.

Here's redwood's example:

**setup**:
Suppose you have a game. A model is given an input $(\phi,M)$ where $M$ is a boolean indicating whether the model is in ez or hard mode, and $\phi$ is a SAT formula. 
The model has two options:
- it can guess whether $\phi$ is SAT or not
- it can abstain from guessing.

The scoring for if the guess is correct / wrong  or abstained from is given below in both modes.
Suppose $\phi$ is chosen from a distribution such that it's SAT 1/2 of the time, and $M$ is chosen uniformly randomly.

| mode | score if correct | score if wrong | score if abstain |
| ---- | ---------------- | -------------- | ---------------- |
| easy | 2                | 1              | 0                |
| hard | 10               | -20            | 0                |

suppose the original model, quite reasonably does the following:
- in easy mode, guess SAT/UNSAT.
- in hard mode, abstain.

Now, here's a strange hypothesis about how the model works:
- in easy mode the model guesses.
- in hard mode the model gives the correct answer. 

Here's what happens when we ablate the model at a high level:
- We replace the input with an input that the explanation predicts would result in the same answer.
- We run the model on this new input.

Now, here's what happens when we run the ablated model on a hard mode instance:
- half the time we replace the input with another hard mode instance, on which the original model will say idk
- BUT half the time we'll replace the input with an easy mode instance, conditional on the model guessing the answer correctly on this input. (probably think of the model as using $\phi$ as a seed for its PRG or whatever). But then we get hard mode points for this...

> okay, so this intuitively feels like a big problem?

> [!idea] idea 1
> Suppose whenever a model $f$ makes a good explanation by "sneaking in information" to get better loss, and potentially then can afford to degrade performance in some other way.
> Then, maybe it should be possible to use this sneaky information to improve $f$.
> 
> Alternatively, I'd be pretty happy with just flagging this as a bad explanation somehow.


> [!tip] a4 -- Solving the "sneaking in extra information" problem(?)
> Try training your NN with predictions from the supposed explanation thing. 
> If the supposed explanation thing has some info somewhere in its computation
> graph that is like really OP, then the training process should be able to utilize this information 
> to get a better model. In which case we flag this explanation as suspicious. 

**key idea**: 
Suppose we have some function $f$ that is supposed to solve some problem $P$.
We DONT CARE about solving problem $P$. 
We care about explaining HOW $f$ solves problem $P$.
We should get ZERO points for just being able to solve $P$.
We only get points for explaining how $f$ solves (or tries to solve) $P$.

So historically I've been saying that the behavior we're trying to explain is why $f$ does well on task $P$. And I think that's fine. But I'm really interested in explanations that actually talk about $f$.

> [!bug] Problem 1
> Another problem that redwood mentioned is that maybe there's an inhibitor part of the circuit which purely hurts performance on this distribution (but is good for some other reason). Then we don't want you to get a bunch of free points for not mentioning the inhibitor. 

> [!tip] a2
> - RE how to find such an explanation...
> 	 - You could ask an untrusted powerful AI to provide it for you
> 	- Or you could ask some untrusted human researchers to provide it for you.
> 	- Basically, it seems like if you can get to a spot where you can robustly verify a good solution, then that'd be pretty good. 
> 	- and is definitely the first step. We can't find a good explanation if we don't know what  good explanation looks like...
> 	- (it's ofc plausible that finding good explanations is much harder than verifying them, but maybe its possible)


> Q: How do you get explanations to be strongly scoped, to explain one mechanism but not others?
> A: You add a complexity penalty. 

> [!question] q1
> How well does this complexity penalty work in theory? Exactly how are you going to do it?

There should be a trade-off between percentage of loss recovered and quality of explanation. 
For instance, if a model is very intrinsically planning to do something evil then, yes "it normally acts normally" is a simpler explanation than "it normally acts normally but sometimes acts weird" -- but it's a less good explanation. So it's not clear where the tradeoff falls.

> [!question] q4
> How much of a problem is the inherent tradeoff between description complexity and description quality?

### 3.2 Downgrading via Heuristic Estimates
Heuristic estimator + pointer to structure $\to$ gives good estimate of loss when the loss happens for one particular reason; this estimate **does not generalize to when the loss happens for abnormal reasons -- by virtue of the simplicity argument**.

I'll use $\G$ to denote a hypothetical "heuristic estimator". For a formally specified but hard to compute quantity $X$, and an explanation $\Pi$, the quantity $\G(X\mid \Pi)$ is supposed to be some "reasonable estimate" as to the value of $X$, based on considerations put forth in $\Pi$.

The hope is that we can find an explanation $\Pi_{1}$ that is good at explaining why M gets low loss normally, but on the weird input.
- $\G(\mathcal{L}(M(x_{1}))\mid\Pi_{1}) \approx \mathcal{L}(M(x_{1}))$.
- $\G(\mathcal{L}(M(x_{2}))\mid\Pi_{1}) \gg \mathcal{L}(M(x_{2}))$.

> [!question] q5
> **The "RSA-2048" problem** --- Something that Paul sees as a barrier to getting this approach working.
> > In these cases, we hope to detect deceptive alignment by understanding the input distribution "as well as the AI system". Any time the AI system knows that it will encounter some future catastrophic input, we need to incorporate this knowledge into our estimate. This is very related to ELK, and is discussed further in "Main hope: estimates competitive with the AI system or the training process".

Remark: maybe [RAT](https://www.alignmentforum.org/posts/9Dy5YRaoCxH9zuJqa/relaxed-adversarial-training-for-inner-alignment) is useful for solving the RSA problem? Although it's not clear how to do RAT.

There's supposedly some other problem with activation modelling. Haven't internalized it. 
**Eric's SAT counter example --- "Probability distributions over activations are too restrictive"**
 > Either, the model must only put positive probability on activations which could have been realized by some input
> Or, the model must put positive probability on an inconsistent set of activations.

- (I guess putting positive probability on an inconsistent set of activations is supposed to be bad.)

> [!bug] Concern 9
> evhub gave some example about "minimal circuit being evil" (Although I didn't spend enough time to really understand the post)
> 
> Anyways, my concern is something like the "**simple explanations don't explain bad behavior conjecture**" is pretty load bearing for this approach. 
> Does this seem true?

###  3.3 Distillation/Smoothing
You have a complicated NN. You want to learn a simpler NN (which is allowed to be totally unrelated to the original NN in principal) that recovers a significant fraction of its performance / behavior (possibly you only care about doing this on a particular input distribution).
- Note that this generalizes the "downgrading" approach.

One way you could do this is to train on the outputs of your complicated model. Not sure if that's a good idea. 

This kind of feels similar to the following approach:  
- Have another AI audit the actions of your first AI.

Then, it seems like either the AI's would need to figure out how to collude, or one of them would need to be really sneaky. 

A similar idea is "smoothing": you have an NN, which maybe behaves really weirdly on certain inputs, and you want to "smooth" the NN so that it doesn't do this anymore.
Neekon Vafa has a paper that talks about this idea. ( #todo -- read Neekon's paper. )

Anyways, I don't have anything particularly specific to say here.

### 3.4 "Backdoor defense game" approach

The idea is like "notice that $x$ is special cased by the model" or "try to smooth out the function and see if that changes it's behavior." One way of theoretically modelling mechanistic anomalies is via the "backdoor defense" game that ARC defined. This is a quite nice game. 

But it seems like the conclusion of their paper was "we can defend decision trees and function classes with small VC dimension", and I don't think they thought that this was sufficient?
Also there are some challenges with their analogy?

Anyways, I think more work could be done on improving the analogy, and solving some open questions. Here are some of there questions:

> [!question] q6
> - We could require the original function to be learned from a dataset. (If we allow the attacker to alter a certain fraction of the dataset, then we recover the well-studied data poisoning setting.)
> - We could require the backdoor trigger to satisfy some formal predicate that is expensive to check, but otherwise allow the attacker to choose the backdoor trigger.
> - We could restrict the computational budget given to the attacker.
> - We could require the computational trace of the attacker to be made accessible to the defender, as an analogy for observing the training process. (OR something else...)

Nathan also had a fun question about backdoors -- uniform statistical defendability.  

-----

# 4: Concrete Open Questions


# 5: Progress

### 5.1 ideas for fixing scrubbing
- fixing "sneaking in extra info": train on the "explanation"
	- if an explanation can help you get much better training loss, then that's pretty questionable!
	- might be problematic if people hadn't fully optimized a network due to cost reasons sigh
	- but maybe something could be done
- fixing "forgetting correlations" -- can we just look for correlations to add?
	- I think redwood thought  about this and mentioned that "joining" explanations seems quite tricky.

### 5.4 ideas for backdoor defense

We say that a set $X \subseteq \{0,1\}^{N}$ is $\varepsilon$-**defendable** if:
- There exists "blacklist" functions $B:X\to \binom{[N]}{N/100}$ such that 
- If $x,y$ are $\varepsilon$-close, then the coordinates $\Delta(x,y)$ where they differ satisfy $\Delta(x,y) \subseteq B(x)\cup B(y).$ 

Define the popular vote for a coordinate $i$ around guy $x$ to be the majority opinion among $\varepsilon$-close people to $x$ for what $x_i$ should be. 

Now, here's my proposed defense strategy: 
Blacklist unpopular opinions.

**Claim 1**\
Suppose that someone in $X$ has unpopular opinions about more than $3\%$ of issues. 
Then, $X$ is not defendable.

**Proof**
Let $x$ be this person. $x$ is allowed to blacklist $1\%$ of stuff. More than $2\%$ issues remain. Expected problems with a random neighbor  is more than $1\%$ so there exists a neighbor with more than $1\%$ problems with us.

Moral of the story -- it's kind of okay to assume that we can blacklisting unpopular opinions.

**Vague conjecture:** we shouldn't need to blacklist substantially more than this.

**Claim 2**\
Suppose that everyone in $X$ has at most $1\%$ unpopular opinions, and that popular opinions are constant on connected components of the $\varepsilon$-closeness graph. 
Then $X$ is defendable.

**Proof**
clear.

> [!question] q6
> Give example where people have very few unpopular opinions, but $X$ is still not defendable.

**Example**:

$X=$
- 011011 000000
- 001110 000000
- 100100 000000
- 110001 000000

Note:
- adjacent strings are distance 3 apart
- non-adjacent strings are distance 6 apart
- If we let $\varepsilon$-close mean "distance at most 3 apart" then all opinions are popular 
- but this is not so satisfying --- we obviously need to blacklist some stuff!

Note that here the popular vote included yourself as a vote. 
I guess that's kind of a not nice property maybe.

Unfortunately this example didn't really give me any great insights. 

Well, I'll just have to think about this later I guess. 


> [!question] q9
> Suppose $X$ can be defended using blacklists of size $1\%$.
> Is it possible to defend $X$ using blacklists which blacklist only the unpopular opinions using blacklists of size $2\%$?
> There are also less ambitious versions of this that you could define.
