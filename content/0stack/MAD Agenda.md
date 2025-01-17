(If you somehow haven't read [[AI xrisk]], go read that first.)
If you have, but it's out of cache, the tldr relevant for this post is: 
- I'm not currently aware of any concrete objective functions that I'd feel comfortable having an AI optimize for. 
- Current ML techniques select for models with a certain input/output behavior on the training distribution. Hence, a deceptively aligned AI could be selected for in training, and could act catastrophically once deployed. 

I have some vague intuitions that 
1. It should be possible to have "explanations" for model behavior.
2. It would be [[goodness(universe)|good]] to have "explanations" for model behavior.

- Or at least, ARC is the only place that I know of that has some proposal for how to potentially do scalable alignment that seems reasonable, and so I'm inclined to try pushing on this approach until it seems unviable or until I become aware of some better ideas. 
- Evanh lists some other proposals [here](https://www.lesswrong.com/posts/fRsjBseRuvRhMPPE5/an-overview-of-11-proposals-for-building-safe-advanced-ai) (list is a bit old, might be dated). But I'm not aware of any organizations / people that are actually working on any of these. Although they might exist. 
- Notably, I'm not counting "get AI's to solve alignment for us" as an alignment strategy, because it doesn't say how the AI's are going to solve alignment. But getting AI's to solve alignment for us seems like something that's worth trying / that will almost inevitably be what we try. So anyways, this just means that I'm very enthusiastic about Control (what redwoodr does). 
- And I'm basically zero excited about mech interp with the goal of making LLMs human understandable, because this sounds not feasible. (and not clear how to define/measure, and not obviously useful).

Anyways, in this post I'll try to 
1. articulate / figure out what I want to mean by an **"explanation"**, 
2. summarize prior work on creating explanations, 
3. highlight some difficulties noticed by prior works on explanations,
4. propose concrete open problems to make progress on defining / understanding / finding / evaluating explanations.
$$\newcommand{\G}{\mathbb{G}}$$
$$\newcommand{\E}{\mathbb{E}}$$
$$\newcommand{\L}{\mathcal{L}}$$
# TODOs from this document

1. Think about whether probes are useful for MAD
2. Patch this document to clarify that the problem is not off distribution inputs, its inputs where the model behaves weirdly. 
3. Think about CS stuff:
	1. Is CS a good way to cash out "explanation"?
	2. How do you fix the problems (eg omitting considerations can help) with CS?
	3. Should you expect good quality explanations to exist?
	4. How the heck could you find such an interpretation? -- possibly needs to happen concurrently to training the model in some sense... But maybe not.
4. Other questions.
5. Backdoor questions.

# 0: Why do you want explanations?
I'm interested in using explanations to solve "**mechanism distinction**" or more narrowly, "mechanistic anomaly detection" (MAD). 
I'll discuss later why I think explanations will be helpful for solving MAD.

remark: Explanations might also be relevant for [[LPE Agenda]] --- I'll consider thinking about this more over there.

Here are some [simple examples of mechanism distinction from Paul](https://www.alignment.org/blog/mechanistic-anomaly-detection-and-elk/):

> A more organic task is to take natural models that can produce a given behavior X via multiple different mechanisms, and then try to assess whether a given approach to anomaly detection can distinguish those mechanisms. That is, given a large dataset of cases where X is due to mechanism A, we’d like to distinguish new cases where X is due to A from cases where X is due to a novel mechanism B. This can both give us an opportunity to improve techniques, and help provide empirical evidence about when mechanistic anomaly detection is possible.
Some examples of (X,A,B) for a language model:
> - X= “A character in a story answers a factual question incorrectly.”  
    A= “the model does not know the answer.”  
    B= “the model thinks that the character doesn’t know (or would lie).”  
> - X= “A person gives consistent answers when a question is asked twice in different forms.”  
    A= “the model recognizes the question is the same.”  
    B= “the model coincidentally gives the same answer.”  
> - X= “A character in a story screams”  
    A= “the model thinks the character is scared”  
    B= “the model thinks the character is excited”  
> - X= “Asked whether at least one of <proposition 1> or <proposition 2> is true, the model says ‘yes’”  
    A= “the model thinks <proposition 1> is true”  
    B= “the model thinks <proposition 2> is true.”  
> - X= “After saying <event 1> occurred at 12:03pm, the model says <event 2> occurred at 12:04pm.”  
    A= “The model thinks <event 1> and <event 2> were 1 minute apart.”  
    B= “The model thinks that <event 2> was scheduled to occur almost exactly 24 hours after <event 1>.”



> [!question] q3
> Do **probes** solve this problem?
> 
> Initial guess is -- no? 
> If you got samples from "does X for reason A" and "does X for reason B" then I think probes let you predict whether X is being done for reason A or reason B. But this seems different from our setup in important ways. But anyways, not sure. 
> 
> If not, is there some way to modify probes such that they do solve this problem?


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

### 2.2 Def v2
You could identify some proposed causal way that your NN is functioning as described [[Causal Scrubbing Notes|here]]. Then you could **test** the purported interpretation as described in [[Causal Scrubbing Notes]].

There are a lot of open questions here:

> [!question] q2
> - How would you find such an explanation?
> - Can you efficiently test how good an explanation is?
> - Are there even nice small explanations for things?
> The biggest theoretical difficulty in my opinion is 
>- **How could you inefficiently test how good an explanation is**?
	> - (The authors of [[Causal Scrubbing Notes|scrubbing]] mentioned several problems with their current approach, such as the fact that dropping considerations can help)
		> - they propose looking into add/join operations as a fix, but it's tricky

### 2.3 Def v3
Heuristic estimator + pointer to structure $\to$ gives good estimate of loss on a particular distribution, **does not generalize off distribution**.

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
