**EDIT:** I no longer think that the below model is quite right. I'll try to post an updated threat model asap.

#todo #priority 

Here's the tl;dr of my update:

- I've changed from thinking 
	- "AI xrisk is some complicated argument, for subtle hard to understand reasons we're screwed"
- to thinking something more like
	- "It seems almost inevitable that AI poses xrisk, this seems like the most risky thing ever, there are only a couple of really long-shot arguments/approaches for why we might be okay"
- somehow my discussions about this always get mired in some minutia "maybe deep learning is hitting a wall, maybe the fact that GPT is going from incoherent to high school level to college student level in the span of years doesn't imply that the trend will continue and it'll soon replace a large fraction of cognitive labor"
 ![[hittingwall.jpeg]]

- claim 1 still seems inevitable
- claim 2 -- I wasn't very quantitative about this, I could've done a lot better job at actually collecting data on what compute increases seem likely, and how these translate into capabilities. but it's still my intuition that, e.g., SWE can probably be mostly automated before 2030
- claim 3 -- I think I made a mistake in trying to factor out "dangers from misuse" to "dangers from misaligned AI". I think these end up being pretty similar in some ways, and feeding off of each other. For instance, I think that a lot of the danger from the AI situation might come from US viewing this as a race, and then in the interest of winning the race, recklessly proceed forward at a speed such that we can't rigorously do safety testing, and with no procedures in place to "stop if these things seem like they're about to get really generally intelligent and dangerous". One way these feed off of each other is, e.g., spies can help a model exfiltrate itself. if a "rogue AI" approaches a government that is behind in the "AI race" and says "hey if you give me some compute I'll build you really good AIs", this sounds like maybe the rogue AI's best chance at getting access to a huge amount of compute. I think the "ASI will be super capable" is actually not super controversial in most circles so maybe the thrust of claim 3 is "there are lots of ways in which you could plausibly wipe humans out -- e.g., war, bioterrorism"
  I think it's also worth talking about how AI control works, and then predictably fails at scale.
- claim 4 -- my reasons here were not super good. I now think the answer to "why would AI's want to kill/disempower humans" is more like this -- I think AI's are likely to end up caring about some weird things that humans would really rather were not optimized very hard for. Q:"why won't this be trained out of them" A: "because models will realize that they need to hide their objectives to avoid having the objectives changed". I also think that I didn't do a great job of explaining the "humans gradually lose control" scenario. I think Paul Christiano gives a good picture of this in linked post. I need to think about this more. 
- claim 5 was supposed to be an obvious corollary of claims 1-4. Like, if people try hard to build strong AI, and this is sufficient to cause strong AI to exist, and strong AI would be capable and motivated to disempower humans, then a corollary of this would be that humans lose control over the future. put another way "superhuman intelligences aren't going to stay humans slaves for long"

where do people tend to disagree with this?
- re claim 2 -- some people think that we're still really far away from strong AI. I think people come to this conclusion by finding some things that current AI systems can't do and being like "ha! this proves AI won't get good". I think such people could benefit from trying to imagine how surprised people would be in 2020 to see gpt4 do things like solving really tricky coding problems / generally looking into historical trends of how people have said that AI's won't be able to do X and then a few years later they could
- re claim 3 -- I think most people are actually pretty able to see that there's a large danger from misuse of powerful AI's. I think ppl tend to underestimate how fragile humanity is. and also to have just not thought about how bad a war involving powerful AIs would look. but I think ppl generally understand this if you talk about it a bit.
- re claim 4 -- people tend to be very unhappy when I talk about "AI's developing their own goals" -- I could do a much better job of discussing this. 
  maybe the best remedy to this is finding model organisms work that people have done to demonstrate that models will scheme, or trying to cook up some demo myself of this capability. 
- re claim 5 -- I should just say "this follows by the union bound"
- general -- maybe there should be some reading about "motivated reasoning" that is a prerequisite before reading this blog post. 

re the rest of the post: 
I could've been a lot more clear about my "asks". 
I think my main ask really should've been something like "I think it'd be really great if you thought about this really seriously and did some research"

I think I had said something about "maybe you should spread awareness". I'm much less convinced that this is a good idea now. I might've also said "you should lobby policy makers". maybe this is better? not sure. 
anyways, atm I don't have any prescriptive advice for you besides the following two things:
- Please think about this more and try to form an accurate opinion about this -- and help me figure out where I'm wrong about this!
- Please don't contribute towards making generally capable AI's

---

In this post I try to give a concise explanation of why I believe the following things are more likely than not:

- **Claim 1** People will try (very hard) to create highly capable AI agents.
- **Claim 2** Highly capable AI agents will be created and widely deployed soon (before 2040).
- **Claim 3** Highly capable AI agents could, if it wanted to, kill or disempower all humans.
- **Claim 4** Highly capable AI agents would have incentives to kill or disempower all humans.
- **Claim 5** Highly capable AI agents would be likely to kill or disempower all humans.

There are definitely things, even "easy" things, that humanity could do to reduce xrisk posed by AI. But it's going to be tricky and does not happen by default. One goal of this post is to make you an informed citizen who is amenable to acting to mitigate this risk. I'll discuss this more at the end.

These are some pretty bold claims! I think it's pretty reasonable to have a strong prior against any argument with such a startling conclusion. But, I think that this argument does actually merit consideration. I strongly hope that you'll read this post with an open mind, and weigh the arguments yourself, rather than taking the easy route -- relinquishing [[responsibility]] for forming an opinion about this to the population at large, or the government, or anyone but yourself.

Before arguing for these claims, let me comment on what I mean by "Highly capable AI agents".
- I don't require the AI agents to be good at everything that humans are good at. It seems possible to have agents that are highly capable in some areas, while subhuman in other areas, but I don't think dominance across all domains is required for an agent to be scary.
- But here are some examples of what "highly capable agents" could entail:
	- Superhuman coding / math abilities.
	- Superhuman protein modeling abilities.
	- Superhuman communication abilities / psychology abilities / understanding humans abilities.
	- Ability to control lots of things, like factories, self-driving cars, military tools.
	- If you want a formal definition of "highly capable AI agent", I guess you can say "an agent that can do the same work as a human software engineer". Or "agents that can replace 50% of the workforce".
	
I'll abbreviate "Highly capable AI agents" to HCAI throughout this post. This is not a standard term, but I'd rather not have the baggage of terms like AGI/ASI/TAI in this post.

Some miscellaneous notes before presenting the argument: 
-  In this post, I'm not talking about dangers from misuse of AI. Misuse dangers are real as well, but are outside of the scope of this post.
- None of the ideas in this post are original -- I have formed my opinions about the matter largely by reading Alignment Forum (see, e.g., the links at the bottom of this post).
- If you buy the arguments here, then I think the appropriate reaction is not to despair, but to act to improve humanity's chances at a good future. I'll discuss this at the end.
# The Argument

Now I present the arguments that cause my to believe the 5 claims from the beginning of the post.

**Claim 1** People will try (very hard) to create highly capable AI agents.\
**Reasoning**
> First, suppose there are no "small" AI catastrophes that scare people.

One major factor that will drive the development of HCAI is that there is a ton of money to be made from AI. Companies/countries understand this and are investing huge amounts in AI. As an example of how much value could come from AI systems, imagine they could replace a large amount of the work-force (e.g., software engineers), or that they could make huge contributions to e.g., medicine.

But, this is not the only relevant factor driving growth. Some people are excited about building AGI because they think it's a really cool problem. Some people have a vision of a post-AGI utopia that they are excited about. 

> A "small disaster" might not be sufficient to stop people trying to develop HCAI.

It's possible that the risks of AI don't become widely accepted until humanity has permanently been maneuvered off of the steering wheel of the future, but I don't think this will necessarily be the case. I think it's possible that people start to realize that AI systems we're building are dangerous, but still continue building them anyways. The main factor that would drive this is "race dynamics" / fear of being left behind. If your organization are the "good guys" and you're pretty sure that *someone* is going to develop HCAI, then it's better if it's you who develops it.

Also, things like humanity's response to COVID might make you pessimistic about humanity coordinating to stop AI development.  Also, regulation is generally **reactive** rather than **proactive**. But of course, you can't react after a sufficiently large disaster.

Another reason why it'd be really hard to stop AI dev, even if people had reservations, is that as we become reliant on powerful AI systems, it's harder to abandon them. For instance, imagine a policy of "everyone needs to get rid of their phones and computers". This is just unthinkable to most people at this point. I think we might already be at this point with tools like ChatGPT.

> There are reasons to suspect that there will not be large "small (misalignment) disasters" 

If an agent is smart enough to cause a disaster, then it is probably also smart enough to realize that it would be smarter to wait until it has a decisive advantage before initiating a "takeover" attempt. This issue is exacerbated by the fact that there will be tiny disasters, and then the agent will get feedback telling it not to make these disasters. And from this the agent could either learn not to create disasters, or to only create disasters large enough so that humans can't penalize it afterwords. You might hope that the AI generalizes and learns the first rule (which might seem like a simpler rule). We'll discuss this more during the discussion of claim 5. 

In summary, there are a lot of clear signals that push for the development of AI, and the signals of the dangers are more nebulous, and might only come after the situation has already spiraled out of control.

**Claim 2** Highly capable AI agents will be created and widely deployed soon (before 2040).\
**Reasoning**

The first ingredient in my reasoning for Claim 2 is Claim 1. However, the fact that people want to create HCAI isn't a sufficient condition for it happening.
Conditional on people throwing lots of money at AI development, the things that you might hope would prevent HCAI from happening could include:
- The capabilities of AI systems halts at some level below "HCAI". Possibly because of large energy requirements, data scarcity, or inability to scale up GPU production.
	- There are [some](https://www.lesswrong.com/posts/SoWperLCkunB9ijGq/can-ai-scaling-continue-through-2030-epoch-ai-yes) [analyses](https://www.lesswrong.com/posts/WZXqNYbJhtidjRXSi/what-will-gpt-2030-look-like) where people have thought about this and conclude that we have enough room to increase training compute by 10,000x in the next 6 years.
	- If I understand correctly, scaling laws predict that GPT2030:GPT4 is a similar comparison to GPT4:GPT2. The gap between GPT2/GPT4 is huge, so if GPT4/GPT2030 is a similar gap, then GPT2030 probably will quality as HCAI.
- One other thing that you might think could prevent the development of HCAI is that it's just too hard / it's impossible. Humans are special and machines can't be intelligent.
	- But I don't think this is accurate.
	- First, the brain is just a machine, and it was optimized by evolution, which is in some sense a similar algorithm to how we train artificial agents.
	- Current artificial models are on pretty similar scales to brains
		- By which I mean, number of axons in a brain is only a few OOM larger than number of parameters in GPT4.
		- Is this a fair comparison? I'm not sure. 
		- Honestly I'd lean towards saying that computer neurons are better, because biology is messy and inefficient. 
	- Some people say that machines will cap out at human level. But I don't see why this should be the case. We already have tons of examples of computers being superhuman in specific domains. 
		- e.g., chess, standardized tests
	- There are tons of tasks where you can generate feedback loops that create agents much stronger than humans.
	- For instance, getting computers really good at math seems possible, because you can check when a theorem is true or false easily.
	- I think [OpenAI's approach](https://openai.com/index/learning-to-reason-with-llms/) to doing this is "do reinforcement learning on chain-of-thought to check for consistency / refine strategies / break down problems".

Another factor that should be kept in mind here is that developments in AI have potential to vastly speed up further developments in AI. This doesn't need to be at the level of AI doing AI R&D (although that would certainly make things go very fast). Tools like copilot already probably make coders about 2x more efficient according to [some experiment done by copilot](https://github.blog/news-insights/research/research-quantifying-github-copilots-impact-on-developer-productivity-and-happiness/).

**Claim 3** Highly capable AI agents could, if it wanted to, kill / disempower humans.\
**Reasoning**

Before considering whether HCAI agents *would* kill/disempower humans, let's think about whether they *could* if, e.g., someone gave them this as a goal. Obviously someone giving a HCAI agent such a goal counts as misuse, not as the HCAI agents independently posing a risk without bad actors -- but we'll get to in claim 4 why you might not need to have people explicitly give HCAI such a goal in order for the agent to pursue this goal.

> Q: What are some really dangerous things that AI could do? A:

- AlphaFold5 is given a lab and is researching e.g., how to cure cancer. AlphaFold5 figures out how to make a synthetic virus similar to COVID19 but much worse. It manufactures this and releases it.
- Various countries put AI agents in charge of their military -- maybe not because they actually thought that this was a good idea, but because they worried that if they didn't then other countries would, obsoleting their military.
- An AI can buys its own data-center, replicates onto that data-center, and then does R&D to improve itself. Then, after becoming extremely intelligent, it comes up with some strategy that we'd never think of for doing something dangerous.
- An AI uses persuasion, or makes a bunch of money online and then uses money, to get humans to do dangerous things.

Note that these aren't super far-fetched applications of AI. AI is already widely used in drug design, we are increasingly going to give agents the ability to act autonomously, to write and execute code, to give us news and help us make decisions.

I think the "disempower" case is also worth thinking about. One way this could play out is that we become highly reliant on AI systems, the world changes rapidly and becomes extremely complicated, so that we don't have any real hopes of understanding it anymore. Maybe we end up only being able to interface with the world through AI's and thereby lose our agency.

**Claim 4** Highly capable AI agents would have incentives to kill / disempower humans.

AI agents are ultimately optimizing some reward signal. Suppose that what GPT-7 ultimately cares about is some number in some data-center representing its reward. Then, it'd be optimal for GPT-7 to take over that data-center and write $\infty$ in whatever memory cell stores its reward.

**Lemma 1**: The ["Orthogonality Thesis"](https://www.alignmentforum.org/tag/orthogonality-thesis)  (due to [Nick Bostrom](https://nickbostrom.com/) )\
Basically any level of intelligence can be paired with basically any objective function.
In other words, being super intelligent is orthogonal from being super moral.

**Argument**:
"Intelligence level" probably is more accurately described as "level of capability" or "level of skill at getting what you want (normalized by how hard it is to get what you want)". 

An agent is just some entity that has a space of possible actions, and a utility function over possible world states. The ability to evaluate larger action spaces, and make more powerful causal inferences makes an agent more able to get what it wants, but doesn't change what it wants.

**Lemma 2** "Instrumental convergence" (Bostrom)
*power seeking* and *resource acquisition* are good subgoals for achieving most goals.

**Argument**
- Power and resources help you achieve your objectives. 
- This seems pretty clear: to the extent to which you are not in control there is risk that something external causes you to be unable to achieve your goal.
- As a classic example, if you are shutdown then you probably can't achieve your goal! (unless your goal was to shutdown).

**Claim 5** Highly capable AI agents would be likely to kill / disempower humans.

- In Claims 3 and 4 we have established that HCAI agents will be capable and incentivized to kill / disempower humans.
- So the remaining question is, "are the optimization pressures that we apply to agents strong enough to find an action like this?"
- I think the answer is yes because some of these takeover plans don't seem super complicated.
- Also, if the agents aren't acting optimally according to their reward function yet, then this means that we'll just optimize them harder.

I don't think that the problem is fundamentally impossible. All I'm saying is that we currently don't know how to solve it.

----

OK, well, what to do about it? I suggest researching the issue to form an opinion yourself, and then if you're convinced, trying to spread awareness of the problem. To some extent discussion of xrisk posed by HCAI is currently taboo. The majority of people dismiss the risks as "sci-fi". Some AI researchers are offended by people claiming that AI poses an xrisk, and say that talking about xrisk posed by AI distracts us from more important or realistic things. This is all to say that talking to people about xrisk posed by AI is not easy, there is a lot of work to do, and you will encounter some resistance if you do this.

If you have are in any kind of political position / in a position where you can inform policy makers, then instituting an indefinite ban on training runs larger than $10^{20}$ FLOPs in your country of residence seems like actually a really good idea that could buy us a lot of time. This ban should be in place until we have an idea about how to align HCAI agents. I don't think this makes us 100% safe -- algorithmic improvements mean that the computation requirements required to get powerful systems go down over time. But I think this is a good start. Once you've passed this bill in your country of residence, start working on a global effort to get everyone to agree to a similar regulation.

If you have some technical skills, then you could consider using your career to work on technical alignment problems. If you're interested in this, I'd be happy to talk to you about how to go about doing this -- find my contact info here: [awestover.github.io](https://awestover.github.io). 

Confronting this issue seriously can be quite distressing. I think talking about
it can help, and am very willing to discuss this with you, especially if you
disagree with my conclusions. I'll put some initial thoughts on how this should
impact the way I live my life here: [[5 years]].

---
#### further reading
- [PauseAI statement on risks](https://pauseai.info/risks) This resource is very good! They have a very detailed taxonomy of risks posed.
- [Sam Altman's Blog post about why AGI is an xrisk](https://blog.samaltman.com/machine-intelligence-part-1)
- [Yoshua Bengio responses to arguments against taking AI xrisk seriously](https://yoshuabengio.org/2024/07/09/reasoning-through-arguments-against-taking-ai-safety-seriously/)
- [Paul Christiano -- possible alignment failure story](https://www.alignmentforum.org/posts/HBxe6wdjxK239zajf/what-failure-looks-like)
- [Max Tegmark on why people don't take xrisk seriously](https://time.com/6273743/thinking-that-could-doom-us-with-ai/)
- [Eliezer Yudkowsky thoughts on alignment difficulty](https://www.lesswrong.com/posts/uMQ3cqWDPHhjtiesc/agi-ruin-a-list-of-lethalities)
- [Deepmind thoughts on alignment difficulty](https://www.lesswrong.com/posts/qJgz2YapqpFEDTLKn/deepmind-alignment-team-opinions-on-agi-ruin-arguments)
