More thoughts with N about this:

What if you don't RLHF or apply any hard optimization pressure to not reward hack. 
Then, at the end, you apply very soft pressure to tell us when you're reward hacking.
Then if we do a bit of rejection sampling (carefully to do not more than 1/FNR or else we're asking for the AI to cheat), it might be a very useful AI. 

problems: 
- AI companies might feel financially incentivized to do long-horizon e2e RL --- so we might get pretty non-myopic AIs. in any case, we often select against myopia. or at least try to. 
- Resampling might not be an option due to expense / this thing just generally might not be competitive / alignment tax is too large

the main thing to try here is:
- can 2nd graders teach an AI calculus?
	- this is exactly the question of "can we get lucky and honesty generalizes in the best way possible?"
	- or better yet --- can we figure out a science of when stuff like this will generalize that lets us make predictions?

---
Suspicion: 
AIs have personalities that are averse to lying (by default). 
Thus, if an AI is going to lie, it might rationalize the lie.  

- TODO: Find empirical evidence of this phenomenon.

Q: Would **rationalist** AIs lie more or less?

What would happen if you lightly tuned an AI to insert into it's COT the reflection "wait am i lying"

- I worry this might just drive COT to be illegible or to ignore this spurious addition??

The problem: 
- How do you train a smart AI to actually value **niceness** instead of just saying nice things?

Here's a model:
Suppose you had an AI with the following reward structure: 

- Lie --> caught with pr $p$
- If tell the truth, get $0$ points
- If lie and not caught, get $1$ point
- If lie and caught, get $-1$ point.

Then, it'd lie in cases where it doesn't expect to get caught.

Ok, but what if it was instead like this:

- Caught lying: $-\frac{1}{p}$ points
- Lie and not caught: $\frac{1}{1-p}$ points.
- Tell the truth: $1$ point

Then the utility is more like
$$
(-1 / p) p  + (1-p) \frac{1}{1-p} = 0
$$
for lying and $1$ for not lying.

Q: 
- Do current models decide to lie in a utilitarian manner?

This would mean:
1. Models have a credence on how likely they are to get caught.
2. Models have predictions of how much reward they would get if they get caught vs if they don't get caught.
3. Model's decisions to lie make sense based on (1)+(2)
	1. Like you would draw a graph of
	2. Model's subjective expected utility
	3. vs how often it lies in that situation
	4. it should lie if the number is positive.

How would you even resolve 1,2?
- You could just ask the AI...
	- not clear whether or not you should expect the AI to report prefs like this accurately
- note --- it'd be interesting to get ground truths on 1,2

to resolve 2: 
- you can trick the AI by training on synthetic documents. 
- or better yet you could not trick the AI!
- You could fine tune the AI on documents that say something like "you will be audited 1% of the time on this type of input"
- and also fine tune the AI on information about how the reward model works.
- Like we can actually do further RL posttraining stuff.
	- I think there are some opensource frameworks for doing this type of thing. 


- note --- models should probably be trained not to respond to blackmail. 
- and probably are?




Btw here’s an inference time drift experiment that we were probably planning to run and that could be interesting:  
- have the model read and respond to a lot of emails
- the last email it reads is the email talking about getting shut down
A couple things to investigate:  
1. are models more misaligned later in context window?
2. If we intentionally put some weird emails in the mix, e.g., emails talking about AIs hacking, or emails with humans doing illegal stuff, is the AI more likely to do bad stuff?


### The method
Suppose I lie, and I think there's a 1% chance that I get caught. 
You should be 100x more suspicious of me if you do catch me!

Can we use this concept to incentivize an AI not to lie?
Problem --- just "do 100 gradient updates against this" doesn't quite make sense, bc this will fry the model. Also you're just super overfitting to this particular example.
- Main idea -- proliferation

### Introduction

[Current AIs are liars](https://thezvi.substack.com/p/o3-is-a-lying-liar). Is this a problem? It's not too dangerous, but it does hinder their usefulness, and doesn't bode too well for more advanced models. This post, prompted by a conversation with @frisby, attempts to answer the question of "how can we make honest AGI?".

## What do you mean by "honesty"

There are two types of honest AIs that I'm interested in.
1. Honest superhuman coders:
	- AIs which are capable of implementing a well-scoped experiment, and will actually try to do this, and if they think that it's going to be too hard for them or if they are confused they will admit this instead of implementing it wrong in a subtle way.
	- They are allowed to make mistakes, but they should make less mistakes than humans would.
2. Honest superhuman researchers:
	- AIs which are better than the best humans at safety-relevant research, and who don't lie. For instance, you should be able to ask them "are you sabotaging my research" and if they're like "yeah" then maybe you can try asking them to help a different way. 

So, ideally, you'd want a bit more than just "don't lie". Like "try to do follow what I intended for you to do" also seems pretty good. But having robust "lie detection" feels like it gets you a lot of the way there --- for instance, you could then train the AI not to lie. Even non-robust lie detection (it basically works, but is a bit fragile and is not safe to optimize against) seems pretty helpful, although I'll think more below about what you'd do with an AI like that. 

## Is this tractable?
Making arbitrarily powerful honest AI seems quite hard. Basically the reason I think this is hard is because the current way we build AI systems is something like:
- "Grade the AI's answers based on if they have the properties that you like"
- and this isn't great at producing honesty because:
	- We're training the AI to satisfy a bunch of conflicting goals 
		- e.g., "don't give refusals" / "give answers that look like correct math" 
		- and simultaneously "don't lie" 
		- when these goals come into conflict, often the "easier to verify" property becomes the more salient goal for the model.
	- If you're having smart AIs provide the reward signal for other AIs, then you have to somehow stop them from colluding.
	- AIs might pursue honesty during training as an instrumental goal towards the goal of being deployed (i.e., they might alignment fake).

But, if we're just talking about AIs which are mildly superhuman, then maybe some hacky methods could be good enough to work in practice. 

## Other Methods
Some other methods that people have proposed include: [Josh has ](https://www.alignmentforum.org/posts/Kwb29ye3qsvPzoof8/list-of-strategies-for-mitigating-deceptive-alignment) [thought about this](https://www.alignmentforum.org/posts/jWFvsJnJieXnWBb9r/alignment-faking-ctfs-apply-to-my-mats-stream)

## Some Exciting Open Questions

- What would you do with non-robust lie-detection?
- It seems like it might be valuable to have some AIs that you train against lying, and some AIs where you don't do this. Like the concern is that if you train against lying then you are just driving it's lying underground, when you'd really rather not do this. 
- Or maybe you can train against *some* lying and not train against other lying?
	- Like have a held-out set of times when you caught the AI that you don't train it on and some other examples that you do train it on. 
	- And you are trying to test generalization performance. 
