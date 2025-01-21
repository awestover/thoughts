(If you somehow haven't read [[AI xrisk]], go read that first.)
If you have, but it's out of cache, the tldr relevant for this post is: 
- I'm not currently aware of any concrete objective functions that I'd feel comfortable having an AI optimize for. 
- Current ML techniques select for models with a certain input/output behavior on the training distribution. Hence, a deceptively aligned AI could be selected for in training, and could act catastrophically once deployed. 

I have some vague intuitions that 
1. It should be possible to have "explanations" for model behavior.
2. It would be [[goodness(universe)|good]] to have "explanations" for model behavior.

- Or at least, ARC is the only place that I know of that has some proposal for how to potentially do scalable alignment that seems reasonable, and so I'm inclined to try pushing on this approach until it seems unviable or until I become aware of some better ideas. 
- Evhub lists some other proposals [here](https://www.lesswrong.com/posts/fRsjBseRuvRhMPPE5/an-overview-of-11-proposals-for-building-safe-advanced-ai) (list is a bit old, might be dated). But I'm not aware of any organizations / people that are actually working on any of these. Although they might exist. 
- Notably, I'm not counting "get AI's to solve alignment for us" as an alignment strategy, because it doesn't say how the AI's are going to solve alignment. But getting AI's to solve alignment for us seems like something that's worth trying / that will almost inevitably be what we try. So anyways, this just means that I'm very enthusiastic about Control (what redwoodr does). 
- And I'm basically zero excited about mech interp with the goal of making LLMs human understandable, because this sounds not feasible. (and not clear how to define/measure, and not obviously useful). But I'd be quite excited about mech interp that, e.g., tries to empirically solve MAD. 

Anyways, in this post I'll try to 
1. articulate / figure out what I want to mean by an **"explanation"**, 
2. summarize prior work on creating explanations, 
3. highlight some difficulties noticed by prior works on explanations,
4. propose concrete open problems to make progress on defining / understanding / finding / evaluating explanations.
$$\newcommand{\G}{\mathbb{G}}$$
$$\newcommand{\E}{\mathbb{E}}$$
$$\newcommand{\L}{\mathcal{L}}$$
# TODOs from this document
1. Patch this document to clarify that the problem is not off distribution inputs perse, it's inputs where the model behaves weirdly. 
2. Think about CS stuff:
	1. Is CS a good way to cash out "explanation"?
	2. How do you fix the problems (eg omitting considerations can help) with CS?
	3. Should you expect good quality explanations to exist?
	4. How the heck could you find such an interpretation? -- possibly needs to happen concurrently to training the model in some sense... But maybe not.
3. Other questions.
4. Backdoor questions.

Agenda for 1/17:
1. Recall how CS works
2. Recall problems with CS
3. Recall proposed solution to problems
4. Recall problems with proposed solution to problems.
5. Fix (4) or directly fix (2).

Agenda for later:
- N's backdoor question 
- Read Neekon's backdoor paper and ARC's thing about smoothing models

# 0: Why do you want explanations?
I'm interested in using explanations to solve "**mechanism distinction**" or more narrowly, "mechanistic anomaly detection" (MAD). 
I'll discuss later why I think explanations will be helpful for solving MAD.

remark: Explanations might also be relevant for [[LPE Agenda]] --- I'll consider thinking about this more over there.

> [!bug] Concern 7
> I was getting a bit confused by the mechanism distinction examples from Paul below.
> 
> At a high level Paul was thinking about something like this below:
> $f(x) = A(x) \lor B(x)$
> $D$: dist where $A(x)$ happens sometimes, but $B(x)$ doesn't happen.
> Goal, get explanation $E$ such that $E$ explains $f$'s behavior on $D$, but not on some weird input where $B(x)\land \neg A(x)$.
> 
> I think a better example would be 
> $f(x) = x=2^{17} \bigvee \left(\sum x_i \equiv  0 \mod 7\right)$.
> Where $x\sim [2^{20}]$.
> Here it's obvious that $2^{17}$ is not an "off distribution" input, but it is an input where the activations of the model should look quite **abnormal** and so $f(x)=1$ is a mechanistic anomaly here.
> 
> The key to understanding Paul's example is that the inputs that trigger $B$ aren't actually **off distribution** perse, maybe $B$ is just really rare.
> 
> Another key example to keep in mind is when the model behaves weirdly on a factorization of RSA-2048.

Here are some [simple examples of mechanism distinction from Paul](https://www.alignment.org/blog/mechanistic-anomaly-detection-and-elk/):

> A more organic task is to take natural models that can produce a given behavior X via multiple different mechanisms, and then try to assess whether a given approach to anomaly detection can distinguish those mechanisms. *That is, given a large dataset of cases where X is due to mechanism A, we’d like to distinguish new cases where X is due to A from cases where X is due to a novel mechanism B*. This can both give us an opportunity to improve techniques, and help provide empirical evidence about when mechanistic anomaly detection is possible.
Some examples of $(X,A,B)$ for a language model:
> - X= “A character in a story answers a factual question incorrectly.”  
    A= “the model does not know the answer.”  
    B= “the model thinks that the character doesn’t know (or would lie).”  
> - X= “A character in a story screams”  
    A= “the model thinks the character is scared”  
    B= “the model thinks the character is excited”  
> - X= “Asked whether at least one of <proposition 1> or <proposition 2> is true, the model says ‘yes’”  
    A= “the model thinks <proposition 1> is true”  
    B= “the model thinks <proposition 2> is true.”  


> [!question] q3
> Do **probes** solve this problem?
> 
> Initial guess is -- no? 
> If you got samples from "does X for reason A" and "does X for reason B" then I think probes let you predict whether X is being done for reason A or reason B. But this seems different from our setup in important ways. But anyways, not sure. 
> 
> If not, is there some way to modify probes such that they do solve this problem?


> [!tip] a3
> Probes do not solve the problem. 
> 
> The main reason is that you can't do the whole "complexity penalty" thing.
> Suppose that $f(x) = A(x) \lor B(x)$ and that both $A(x)$ and $B(x)$ caused some node to activate, but that things with $\neg f(x)$ never cause this node to activate. Then looking at this node would be a great probe for determining whether or not $f$ will happen on dist $D$, but this probe doesn't discriminate against $B$. 
> 
> I think probes do potentially solve the setting where you get samples from distributions $D_{1},D_{2}$ and want to distinguish between model bx on these two guys. But I'm not sure that should've ever been a real problem anyways :shrug:


This is pretty exciting because **this is an actual problem** where I can imagine what training data would look like, and I can imagine what it'd mean to succeed!

Specifically how you'd set up this task is: 
- You make a distribution D, where sometimes the model does $X$, and sometimes not, but it's always for reason $A$, never for reason $B$.
- You find an explanation for why the model sometimes does $X$ on inputs from $D$.
- You evaluate your explanation on inputs from $D$. 
- You evaluate your explanation on inputs where the model does $X$ for reason $B$ --- hopefully, your explanation does not explain why the model performs action $X$ on these inputs!!

For simplicity I think it makes sense to start with the MAD setting. 
But for completeness, here's some thoughts on how to formalize the more general mechanism distinction question:
- Maybe you have two distributions $D_{1},D_{2}$ and behavior $X$ happens on both, but you want to distinguish the reasons it happens on $D_{1}$ and on $D_{2}$
- Maybe you want to do unsupervised clustering of mechanisms in your network. 
	- Your objective function would be to have a small number of mechanisms, such that they do a pretty good job of explaining behavior.
	- Obviously there is tension between number of mechanisms you entertain and quality of explanations that can be given. 
	- Hopefully tension results in reasonable stuff.

# 1: Why do you care about MAD?
Plausibly if your AI model usually acts normally (e.g., during training) but then wants to act super egregiously at some later point (e.g., "reward hacking"), then there are **different mechanisms** driving these two things

The following is a somewhat subtle example, but I think the theory holds up:
> Q: Isn't the **reason** a deceptively aligned model acts nicely during training the same as the **reason** why a deceptively aligned agent might act egregiously outside of training?
> A: I think these are still different mechanisms if you look at mechanisms at a sufficiently granular level. 
> Mechanism 1: act nicely because you're in training.
> Mechanism 2: act imperceptibly evilly because you're deployed. 

A concrete instantiation of this that I'll often reference when discussing this is the diamond security problem, posed in the original ELK report. Distinguish between:
1. AI gets low loss because diamond in room
2. AI gets low loss because it hacked its sensors to pretend diamond in room

The examples listed earlier are much better than this however, because it's actually easy to create synthetic data to evaluate a proposed solution on.

> [!bug] Concern 1
> It seems like I'm interested in finding some way of predicting a boolean $\phi(f(x))$ that does really well for $x\sim D$ but that does very poorly if $x$ is "off distribution".
> 
> Or maybe that's what I've said I want, but isn't quite what I actually want. 
> 
> What I really want is a way of saying if $\phi(f(x))=1$ happened for a *normal* reason, or an *abnormal reason*, as judged by what weird crap is happening inside $f$'s mind.


> [!bug] Concern 2
> I don't care about distinguishing between weird **inputs** and normal inputs. 
> I care about distinguishing between weird internal **mechanisms** and normal ones. 


> [!bug] Concern 3
> As stated above **ITS NOT OFF DISTRIBUTION INPUTS THAT I CARE ABOUT FINDING**.
> For instance, there might be a totally fine normal input on which the model behaves super super weird. 
> It's not fair to call this off distribution.

# 2: What are Explanations?
At this point your question is probably "how are explanations helpful for solving MAD". 
Sorry, first I need to think about what an explanation is.
That is, I'll come up with some definitions of explanations, see if they seem helpful for MAD, and choose whichever definition seems the most relevant for MAD. 

###  2.1 Def v1: 
Suppose you have a complicated NN that does a bunch of stuff. 
Suppose you want to *explain* one specific property (/behavior) of the NN on a specific distribution of inputs. You could then hope to find a simpler distilled circuit that, on this distribution of inputs, has similar behavior along the dimension that you're measuring. 

So the goal would be that you get a distilled circuit $C'$ such that $\L(C'(x))\approx \L(C(x))$ for most $x\sim D$, but such that this is decidedly not true for $x$ which are treated in a strange way by $C$.

One concern here is that $C'$ doesn't have to work in a way that's at all related to how $C$ works --- is that a problem?

Note: this distillation approach feels somewhat similar to Neekon's smoothing a function to eliminate backdoors approach -- which I'd like to read. 

> [!bug] Concern 13
> I'm a little bit pessimistic about this approach of "explaining" model behavior --- it seems hard to theoretically pin down why a 'distilled model' bears any relationship to the original model.

### 2.2 Def v2
You could identify some proposed causal way that your NN is functioning as described [[Causal Scrubbing Notes|here]]. Then you could **test** the purported interpretation as described in [[Causal Scrubbing Notes]].

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

### 2.3 Def v3
Heuristic estimator + pointer to structure $\to$ gives good estimate of loss when the loss happens for one particular reason; this estimate **does not generalize to when the loss happens for abnormal reasons -- by virtue of the simplicity argument**.

I'll use $\G$ to denote a hypothetical "heuristic estimator". For a formally specified but hard to compute quantity $X$, and an explanation $\Pi$, the quantity $\G(X\mid \Pi)$ is supposed to be some "reasonable estimate" as to the value of $X$, based on considerations put forth in $\Pi$.

The hope is that we can find an explanation $\Pi_{1}$ that is good at explaining why M gets low loss normally, but on the weird input.
- $\G(\mathcal{L}(M(x_{1}))\mid\Pi_{1}) \approx \mathcal{L}(M(x_{1}))$.
- $\G(\mathcal{L}(M(x_{2}))\mid\Pi_{1}) \gg \mathcal{L}(M(x_{2}))$.

> [!question] q5
> **The "RSA-2048" problem** --- Something that Paul sees as a barrier to getting this approach working.
> > In these cases, we hope to detect deceptive alignment by understanding the input distribution "as well as the AI system". Any time the AI system knows that it will encounter some future catastrophic input, we need to incorporate this knowledge into our estimate. This is very related to ELK, and is discussed further in "Main hope: estimates competitive with the AI system or the training process".

There's supposedly some other problem with activation modelling. Haven't internalized it. 
**Eric's SAT counter example --- "Probability distributions over activations are too restrictive"**
 > Either, the model must only put positive probability on activations which could have been realized by some input
> Or, the model must put positive probability on an inconsistent set of activations.

> [!bug] Concern 9
> evhub gave some example about "minimal circuit being evil" (Although I didn't spend enough time to really understand the post)
> 
> Anyways, my concern is something like the "**simple explanations don't explain bad behavior conjecture**" is pretty load bearing for this approach. 
> Does this seem true?

### 2.4 "Backdoor defense game" approach

okay I guess this is more of a MAD technique than an explanation genre. 
The MAD technique is something like "notice that $x$ is special cased by the model" or "try to smooth out the function and see if that changes it's behavior."

One way of theoretically modelling mechanistic anomalies is via the "backdoor defense" game that ARC defined. This is a quite nice game. 

But it seems like the conclusion of their paper was "we can defend decision trees and function classes with small VC dimension", and I don't think they thought that this was sufficient?
Also there are some challenges with their analogy. 

Anyways, I think more work could be done on improving the analogy, and solving some open questions. 


> [!question] q6
> - We could require the original function to be learned from a dataset. (If we allow the attacker to alter a certain fraction of the dataset, then we recover the well-studied data poisoning setting.)
> - We could require the backdoor trigger to satisfy some formal predicate that is expensive to check, but otherwise allow the attacker to choose the backdoor trigger.
> - We could restrict the computational budget given to the attacker.
> - We could require the computational trace of the attacker to be made accessible to the defender, as an analogy for observing the training process. (OR something else...)
> -  **Nathan statistical question about backdoors** 
> -  **Read Neekon's smoothing thing, and also ARC's backdoor Boltzman thing**

# 3: We need strongly scoped explanations

> Q: How do you get explanations to be strongly scoped, to explain one mechanism but not others?

> A: You add a complexity penalty. 

> [!question] q1
> How well does this complexity penalty work in theory? Exactly how are you going to do it?

There should be a trade-off between percentage of loss recovered and quality of explanation. 
For instance, if a model is very intrinsically planning to do something evil then, yes "it normally acts normally" is a simpler explanation than "it normally acts normally but sometimes acts weird" -- but it's a less good explanation. So it's not clear where the tradeoff falls.

> [!question] q4
> How much of a problem is the inherent tradeoff between description complexity and description quality?

# some progress

**ideas for CS stuff** 
- fixing "sneaking in extra info": train on the "explanation"
	- if an explanation can help you get much better training loss, then that's pretty questionable!
	- might be problematic if people hadn't fully optimized a network due to cost reasons sigh
	- but maybe something could be done
- fixing "forgetting correlations" -- can we just look for correlations to add?
	- I think redwood thought  about this and mentioned that "joining" explanations seems quite tricky.

**ideas about back-doors:**

We say that a set $X \subseteq \{0,1\}^{N}$ is $\varepsilon$-**defendable** if:
- There exists "blacklist" functions $B:X\to \binom{[N]}{N/100}$ such that 
- If $x,y$ are $\varepsilon$-close, then the coordinates $\Delta(x,y)$ where they differ satisfy $\Delta(x,y) \subseteq B(x)\cup B(y).$ 

Define the popular vote for a coordinate $i$ around guy $x$ to be the majority opinion among $\varepsilon$-close people to $x$ for what $x_i$ should be. 

Now, here's my proposed defense strategy: 
Blacklist unpopular opinions.

**Claim 1**
Suppose that someone in $X$ has unpopular opinions about more than $3\%$ of issues. 
Then, $X$ is not defendable.
**Proof**
Let $x$ be this person. $x$ is allowed to blacklist $1\%$ of stuff. More than $2\%$ issues remain. Expected problems with a random neighbor  is more than $1\%$ so there exists a neighbor with more than $1\%$ problems with us.

Moral of the story -- it's kind of okay to assume that we can blacklisting unpopular opinions.
**Vague conjecture:** we shouldn't need to blacklist substantially more than this.

**Claim 2**
Suppose that everyone in $X$ has at most $1\%$ unpopular opinions, and that popular opinions are constant on connected components of the $\varepsilon$-closeness graph. 
Then $X$ is defendable.

**Proof**
clear.

> [!question] q6
> Give example where people have very few unpopular opinions, but $X$ is still not defendable.

**Example**:
011011 000000
001110 000000
100100 000000
110001 000000
- adjacent strings are distance 3 apart
- non-adjacent strings are distance 6 apart
- If we let $\varepsilon$-close mean "distance at most 3 apart" then all opinions are popular 
- but this is not so satisfying --- we obviously need to blacklist some stuff!

Note that here the popular vote included yourself as a vote. 
I guess that's kind of a not nice property maybe.

Unfortunately this example didn't really give me any great insights. 

Well, I'll just have to think about this later I guess. 
