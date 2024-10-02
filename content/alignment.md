In this post I try to give a concise explanation of why I believe the following things: 

- **Claim 1** People will try (very hard) to create highly capable AI agents.
- **Claim 2** Highly capable AI agents will be created and widely deployed soon (before 2040).
- **Claim 3** Highly capable AI agents could, if it wanted to, kill / disempower humans.
- **Claim 4** Highly capable AI agents would have incentives to kill / disempower humans.
- **Claim 5** Highly capable AI agents would be likely to kill / disempower humans.

First some notes: 
- There are reasons why you will want to dismiss these claims without serious consideration. 
- Here is a list of rationalizations that I engaged in when I heard about this argument.
	1. Someone *else* would have noticed and done something about this if it were really such a huge problem. 
	2. It's not my responsibility. 
	3. The lack of substantial restrictions from governments, lack of concern among companies pursuing intelligence, and lack of concern among the general population indicate that this isn't really a problem. 
	4. Maybe highly capable AI isn't achievable. 
	5. Maybe highly capable AI will be nice by default. 
	6. This can't be a real concern. Surely people would stop before things actually got dangerous.
	7. Humans are pretty resilient. We could handle it. 
	8. Hmm, this seems uncomfortable, I'd rather not think about it.
- These rationalizations basically boil down to: (1) bystander effect / refusal to take responsibility for something, (2) motivated reasoning, (3) conformity pressures. 
I find these rationalizations un-enticing, and so have decided to seriously consider these claims.

Before arguing for these claims, let me comment on what I mean by "Highly capable AI agents".
- I don't require the AI agents to be good at everything that humans are good at. 
- But here are some examples of what "highly capable agents" could entail:
- superhuman coding / math abilities
- superhuman protein modeling abilities
- superhuman communication abilities / psychology abilities / understanding humans abilities
- ability to control lots of things, like factories, self-driving cars, military tools
- If you want a formal definition of "highly capable AI agent", I guess you can say "an agent that can do the same work as a human software engineer". Or "agents that can replace 50% of the workforce".

Now I present the reasons why I think these claims are true.

**Claim 1** People will try (very hard) to create highly capable AI agents.\
**Reasoning**
- First, consider a scenario where there aren't any "small" AI catastrophes that scare people.
	- I'll talk about whether or not "small" AI catastrophes are likely, and whether or not this will stop people from developing powerful AI in a bit.
- There is a ton of money to be made from AI.
	- Companies/countries understand this and are investing huge amounts in AI.
	- As an example of how much value could come from AI systems, imagine they could replace a large amount of the work-force (e.g., software engineers), or that they could make huge contributions to e.g., medicine.
- People are pretty excited about making AGI.
- Companies/countries are scared of being left behind.
	- Note that this holds even if your organization is scared about safety too.
	- If you stop, then someone else can get ahead by not stopping. This is what makes the coordination problem pretty tricky.
	- This fear is rational if AI progress really is possible -- if you halt AI progress but other places don't, then you'll quickly become irrelevant.
- You might hope that a huge disaster will happen, and that this will spur global coordination.
	- Unfortunately, there are some reasons to believe that this is unlikely.
	- First, if an agent is smart enough to cause a huge disaster, then it is probably also smart enough to realize that it would be smarter to wait until it has a decisive advantage before initiating a "takeover" attempt.
	- This issue is exacerbated by the fact that there will be tiny disasters, and then the agent will get feedback telling it not to make these disasters. And from this the agent could either learn not to create disasters, or to only create disasters large enough so that humans can't penalize it afterwords. 
	- You might hope that the AI generalizes and learns the first rule (which might seem like a simpler rule). 
	- We'll discuss this more in a bit. I will say that I think understanding generalization seems like a good direction of alignment research, and I'm working on a toy project in this area.
- Another reason why it'd be really hard to stop AI dev is that as we become reliant on powerful AI systems, it's harder to abandon them. For instance, imagine a policy of "everyone needs to get rid of their phones and computers". This is just unthinkable to most people at this point. 

**Claim 2** Highly capable AI agents will be created and widely deployed soon (before 2040).
**Reasoning**
- The first ingredient in my reasoning for Claim 2 is Claim 1.
- However, the fact that people want to create AGI isn't a sufficient condition for it happening.
- Conditional on people throwing enough money at AI development, the things that you might hope would prevent AGI from happening could include:
	- AI development gets bottle-necked by energy, data scarcity, chip production speeds.
		- There are [some](https://www.lesswrong.com/posts/SoWperLCkunB9ijGq/can-ai-scaling-continue-through-2030-epoch-ai-yes) [analyses](https://www.lesswrong.com/posts/WZXqNYbJhtidjRXSi/what-will-gpt-2030-look-like) where people have thought about this and conclude that we have enough room to increase training compute by 10,000x in the next 6 years.
		- If I understand correctly, scaling laws predict that GPT2030:GPT4 is a similar comparison to GPT4:GPT2. The gap between GPT2/GPT4 is huge, so if GPT4/GPT2030 is a similar gap, this would probably imply Claim 2.
	- One other thing that you might think could prevent the development of AGI is that it's just too hard / it's impossible. Humans are special and machines can't be intelligent.
		- But I don't think this is accurate.
		- First, the brain is just a machine, and it was optimized by evolution, which is in some sense a similar algorithm to how we train artificial agents.
		- Current artificial models are on pretty similar scales to brains
			- By which I mean, number of axons in a brain is only a few OOM larger than number of parameters in GPT4.
			- Is this a fair comparison? I'm not sure. 
			- Honestly I'd lean towards saying that computer neurons are better, because biology is messy and inefficient. 
		- Some people say that machines will cap out at human level. But I don't see why this should be the case. We already have tons of examples of computers being superhuman in specific domains. 
			- e.g., chess, standardized tests
		- there are tons of tasks where you can generate feedback loops that create agents much stronger than humans.
		- For instance, getting computers really good at math seems possible, because you can check when a theorem is true or false easily.

#todo *claims 3-5 are less well developed. flesh them out more*

**Claim 3** Highly capable AI agents could, if it wanted to, kill / disempower humans.

> Q: What are some really dangerous things that AI could do? A:

- AlphaFold5 is given a lab and is researching e.g., how to cure cancer. AlphaFold5 figures out how to make a synthetic virus similar to COVID19 but much worse. It manufactures this and releases it.
- If you have AI in charge of your military this could be bad.
- If you have an AI that is allowed to manage some money, then maybe it can make a bunch of money and use this to do evil things.
- Maybe an AI can buy its own data-center and replicate onto that data-center.
- Maybe AI can do manipulation of humans.

> Q: What are some other things that could happen?

- We become very reliant on AI systems. The world changes rapidly and we no longer have any hope of understanding it.
- We just learn whatever AI's tell us. 
- So this could maybe result in humans losing agency, even though they are not dead.

Honestly, I think you don't even need to be super powerful to start out with.
The whole "agent buys a datacenter to replicate onto where it isn't regulated" thing seems pretty possible. We are going to give these agents power to act autonomously. 
Even if we didn't, if chatgpt could come up with some complicated scheme to make you a bunch of money would you just copy-paste it into a python script without reading it and run it? / would you authorize it to do so? "yeah, seems pretty likely".

**Claim 4** Highly capable AI agents would have incentives to kill / disempower humans.

**Lemma 1**: The ["Orthogonality Thesis"](https://www.alignmentforum.org/tag/orthogonality-thesis)  (due to [Nick Bostrom](https://nickbostrom.com/) or maybe Eliezer Yudkowsky)\
Basically any level of intelligence can be paired with basically any objective function.
In other words, being super intelligent is orthogonal from being super moral.

**Argument**:
"Intelligence level" probably is more accurately described as "level of capability" or "level of skill at getting what you want (normalized by how hard it is to get what you want)". 

An agent is just some entity that has a space of possible actions, and a utility function over possible world states. The ability to evaluate larger action spaces, and make more powerful causal inferences 

**Lemma 2** "Instrumental convergence" (also Bostrom or Yudkowsky)
Activities like *power seeking* and *resource acquisition* make sense across a broad range of possible utility functions.
**Argument**
- power and resources help you achieve your objectives. 
- this seems pretty clear: to the extent to which you are not in control there is risk that something external causes you to be unable to achieve your goal.


**Claim 5** Highly capable AI agents would be likely to kill / disempower humans.

- In Claims 3 and 4 we have established that agents will be capable and incentivized to kill / disempower humans.
- So the remaining question is, "are the optimization pressures that we apply to agents strong enough to find an action like this?"
- I think the answer is yes because some of these takeover plans don't seem super complicated.
- If we run agents for long enough, the will probably consider this strategy.

----

OK, well, what to do about it?
One simple (but still pretty hard because it requires researching the issue to form an opinion yourself, and then putting your reputation on the line for believing something somewhat radical) thing you can do is spread awareness of the problem.
I'm pretty sure this is a net positive? The potential downside is that it could cause people to experience negative emotions. The potential upside is that you can better optimize when you have more information about the constraints you're working under. 
This is discussed a bit more here: [[5 years]].

---
#### further reading

- [Richard Ngo on Alignment difficulties](https://arxiv.org/pdf/2209.00626)
- [Here are some more resources on ML / alignment](https://course.aisafetyfundamentals.com/alignment)
- [Some of Paul Christiano's thoughts on alignment difficulty](https://www.lesswrong.com/posts/CoZhXrhpQxpy9xw9y/where-i-agree-and-disagree-with-eliezer)
- [Deepmind thoughts on difficulty](https://www.lesswrong.com/posts/qJgz2YapqpFEDTLKn/deepmind-alignment-team-opinions-on-agi-ruin-arguments)
- [Eliezer views on difficulty](https://www.lesswrong.com/posts/uMQ3cqWDPHhjtiesc/agi-ruin-a-list-of-lethalities)