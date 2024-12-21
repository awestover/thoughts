Note: this whole post still needs to be edited quite a bit. And the reasons for Claim 4 -- arguably the most confusing and important part -- aren't spelled out yet. Sorry about that. It might be a second before I get to this. I hope the current version of this document is still enough to be helpful to you, and is at least better than me not putting out anything. 

---

Epistemic Status: 
I've thought about this extensively for about 4 months. I'm confident in the main claim of this post. 

---

In this post I'll explain why I believe the following claim:

> [!tip] Claim X
> There is at least a $10\%$ chance that advances in AI will lead to a catastrophically bad outcome by 2029.

==Important note: it does not matter if the US develops AI first, or someone else. There is a huge amount of danger even if people with good intentions build powerful AI systems.== 
If you share this message I implore you to not encourage an "AI race" -- there are no winners in an AI race, we all lose together.

This is a view that isn't main-stream yet, which makes it easy to dismiss without consideration. In fact it's worse than this -- many people view this claim as "too sci-fi", and will ridicule you for taking it seriously [^2]. So, why should you treat this post more seriously than you would some random conspiracy theory? Here are four reasons:
- Many credible ML experts believe Claim X. For instance Geoffrey Hinton (recent Nobel Prize winner) and [Yoshua Bengio](https://yoshuabengio.org/2024/07/09/reasoning-through-arguments-against-taking-ai-safety-seriously/) (Turing Award Winner) who pioneered many AI advances have become vocal advocates about the risks from AI. 
- There is a large amount of tension between ML engineers because of differing opinions about safety. For instance, a very large number of scientists have left OpenAI because of safety concerns, and have joined/created various organizations with the goal of making safer AI (e.g., Illya's [Safe Super Intelligence](https://ssi.inc/), and Anthropic).
- LLMs are very competent and getting better fast. It's clear that AI is going to have a huge impact on society, and worth thinking about what exactly the implications of powerful AI are. 
- I've thought about this and take Claim X quite seriously, to the point where I'm willing to change my actions substantially based on this belief.
It's easy to feel that this isn't *your* problem, but you're wrong. This is everyone's problem. I hope that you'll read this post and actually think about the arguments instead of denying [[responsibility]].

[^2]: Max Tegmark gives an account of various methods people use to avoid taking this issue seriously [here](https://time.com/6273743/thinking-that-could-doom-us-with-ai/)

>Note: My actual estimate of the probability that advances in AI lead to catastrophe by 2029 is 80%, but I'll just argue that 10% chance by 2029 is a lower bound on risk, because I'd hope that if you were truly convinced of this, you'd do something about it. 

The purposes of this post are as follows:
1. [[on writing, blogging and being opinionated|Clarify]] my AI risk predictions (for myself and others).
2. Facilitate [[good arguments]] about AI xrisk, and about approaches to risk mitigation.
3. Convince you to take AI risk **seriously**. (I'll talk about what I mean by this in a bit).
#### Post Outline
0. Clarifying the argument, and giving some background.
1. The basic argument for AI xrisk.
2. A more in-depth version of the argument.
3. Reasons we might be okay.
4. What to do about this.
## Setup
Humanity is awesome, and I care about humanity a lot, as outlined [[goodness(universe)|here]]. Of course humans sometimes do non-awesome things -- but we have a lot of potential. I can imagine a world like our world today, but with less pain, sickness and sorrow. We can give the next generation a better world or at least a good world. However, **this is not guaranteed**. 

This was a really hard idea for me to accept. It feels "unfair". In a story the characters can't just die. Or if they do it at least *means* something, and everything basically turns out all right in the end. In a story, you can face 9:1 odds and win; in reality when you face 9:1 odds, you lose with 90% probability. We do not live in a story. Things can actually go wrong, even extremely wrong. 

To get some basic intuition for this, it helped me to think about the cold war. There were [multiple times](https://en.wikipedia.org/wiki/Cuban_Missile_Crisis) where the cold war was extremely close to turning into a hot war. A large part of the reason that it didn't is that we got lucky. 

In this post, I discuss ways that AI progress could lead to catastrophically bad outcomes.
In a moment, I'll define what I mean by that, but I'd first like to emphasize an important point:
==AI poses large risks regardless of who makes it.== This is a big difference between AI and nukes. 

It's relatively easy to understand AI as analogous to a "weapon", and thereby reason that it's important for good actors to develop powerful AI before bad actors. This line of thought leads actors to develop AI in a **race** (e.g., racing on AI progress was suggested to congress in [^1]).
However, ==racing to develop very powerful AI is extremely dangerous==. There are major challenges (which I'll discuss in a bit) to verifying that an AI agent is *safe*, and race pressures deprive actors of the time necessary for good evaluations, making it more likely that we'll be comforted by misleading indicators of safety.

I request that when you communicate about risks from AI, you focus on the fact that powerful AI is dangerous regardless of who builds it -- I worry that spreading the simplified message "AI is dangerous" can be [net-negative](https://forum.effectivealtruism.org/posts/CcJsh4JcxEqYDaSte/spreading-messages-to-help-with-the-most-important-century) by encouraging race dynamics.

**What are "bad outcomes"?**
I'm concerned about the following outcomes:
- Human extinction.
- Human dis-empowerment (i.e., we are relegated to a role in the world like dogs -- maybe AI's will keep us around, but we won't  have much say in how things go.)

**What does "powerful AI" look like?**
In this post I'll discuss dangers from "Artificial Superintelligence" (**ASI**). By "ASI" I don't require an AI agent that can outperform humans in every domain; instead, I'll use ASI to refer to an AI that is superhuman in at least half of the following domains:

| Domain        | Example                              |
| ------------- | ------------------------------------ |
| Coding        | SWE, ML research, cyber-attacks      |
| Math          | Theorem proving                      |
| Biology       | Protein modelling                    |
| Psychology    | Understanding and influencing humans |
| War/Defense   | Controlling military equipment       |
| Planning      |                                      |
| Finance       | Trading                              |
| Business      | Running a company                    |
| Manufacturing | Running a factory                    |

> Note: There are risks that arise before ASI, e.g., just with AGI. I'm focusing on the risks from ASI because I want to make a minimum viable case for Claim X. 

## The Basic Argument for AI xrisk
Now that I'll outline a basic argument for why it's at least 10% likely that AI progress results in catastrophe by 2029.

- **Claim 1**: People will try hard to make ASI.
	- Scientists/engineers are excited about AI.
	- Governments think AI is important for military / economic reasons.
- **Claim 2**: ASI is probable soon.
	- Extrapolate capabilities progress. 
	- With the amount of money and talent being thrown at the problem, we'll overcome any obstacles.
- **Claim 3**: An ASI would be capable of killing/dis-empowering all humans.
	- Looking at my table of ASI capabilities this is hopefully fairly intuitive.
	- As a helpful analogy, elephants still exist because we decided not to kill all of them; they'd have no chance if we wanted them gone.
- **Claim 4**: ==ASI does not have human-compatible goals by default, and we don't have a plan for how to fix this.== (i.e., there is a decent chance that an ASI could further its goals by killing/dis-empowering humanity).
	- Being in control is advantageous for achieving most goals.

> Note: From some informal polling, my impression of where people stand on this is as follows:
> - Some people doubt Claim 2 -- they tend to point to flaws in current AI's (e.g., hallucinations), and assert that current AI's aren't very smart / capable or that they are just stochastic parrots. 
> 	- My first thought on this is that it's missing the point -- what matters is not how good AI's are right now, but how good they'll be by 2029.
> 	- Also, the claim that current AI's aren't capable of reasoning is absurd -- you can't get to ELO 2727 on CodeForces (so, one of the 200 best competitive programmers in the world) (as OpenAI's o3 model does) without being able to do complex reasoning. 
> 	- If you don't believe in AI progress, then you'll always be surprised when [a benchmark is destroyed](https://arcprize.org/blog/oai-o3-pub-breakthrough) (without OpenAI even trying).
> - Many people also complain that "AI's can't have goals". 
> 	- However, current AI's obviously have goals -- for example current AI's care about answering user queries well and denying harmful queries. In fact, AI's [resist human attempts to change their goals](https://www.anthropic.com/research/alignment-faking).

Here are a few more common objections to my argument that I'll address:
- re 1: why won't people stop building AI once they realize its super dangerous or once people get annoyed that they lost their jobs?
- re 2: why won't energy / or money run out before we can scale more?
- re 2: why won't models stop improving once they get to human level? 
- re 3: why couldn't we just turn off AI's if they got really scary / turned against us?
- re 4: why would an AI even "want" anything?
- re 4: don't we get to choose what the AI cares about?
I have good answers to all of these objections that I'll give in a later section. However, there are a couple of considerations that I find actually compelling, that cause me to believe that there's a ~20% chance that we're fine by 2029.

### Basic Reasons Why We Might be Fine by 2029
I'll discuss in further depth later why my numbers are so small here (small in an absolute sense, not in the sense that I think they're un-calibrated).
- **Reason 1**: We might intentionally slow AI development (~5% chance).
	- There are some good people doing policy work advocating to stop pushing the frontier (see, e.g., [this](https://pdf.narrowpath.co/A_Narrow_Path.pdf)).
	- There are some good people that work at frontier labs, maybe they can help slow as it becomes more obvious that the risk is unacceptable.
- **Reason 2**: Even if people keep throwing money at AI, maybe AI's won't be capable enough or widely deployed enough by 2029 to do harm, even if they really wanted to. (~5% chance)
	- Maybe there are some unforeseen bottlenecks.
	- Maybe labs will stop deploying models and this somehow limits the reach of the AI's.
		- Something like this -- e.g., AI progress turning from a corporate project to a government run project that doesn't release the models -- actually seems moderately likely.
		- But I'm not convinced that this is very likely to hamper AI's ability to do harm, and it plausibly exacerbates it. 
- **Reason 3**: Maybe we "solve alignment" -- i.e., we figure out how to ensure that an AI cares about things that we care about, and we also figure out some good things for an AI to care about. Or we figure out how to prevent AI's from deceiving us. (~5% chance)
	- Approaches I'm excited about: 
		- [ARC](https://www.alignment.org)'s heuristic explanation agenda for solving ELK+MAD. 
		- Some kind of distillation / iterated amplification thing, possibly with some mech interp (or something simpler like probes) that helps us figure out if a model is lying.
- **Reason 4**: Maybe scheming (deceptive alignment) is really hard, and models just do good things by default. (~5% chance)
	- Scheming is pretty hard if we have good control measures.

It's non-trivial to aggregate these probabilities because there is some correlation and anti-correlation. But it comes out to about a 20% chance that nothing catastrophic has happened by 2029.

> Note: I'm not very happy about this number. I propose that you and I try to change this number. 
## A More In-Depth Argument

> [!tip] CLAIM 1: 
> ASI is probable, soon.

There are four major ingredients that go into making powerful models, which could potentially bottleneck progress:
> Energy, data, GPU chips, and money.

Some people have [crunched some numbers on this](https://www.lesswrong.com/posts/SoWperLCkunB9ijGq/can-ai-scaling-continue-through-2030-epoch-ai-yes), and [we still have substantial room to grow in these four areas](https://www.lesswrong.com/posts/WZXqNYbJhtidjRXSi/what-will-gpt-2030-look-like). Also there's a trend of cost decreasing rapidly over time. E.g., o3-mini is comparable or better than o1 at much reduced cost. If there is going to be some big bottleneck, (e.g., energy), I expect labs to be proactive about it (e.g., build power plants).

The basic reason why I believe that ASI is probable soon is progress trends. For example,
- GPT3 -- high schooler
- GPT4 -- college student
- o3 -- Comparable with CS/Math Olympiad competitors on close ended ~6 hour long tasks. Extremely good at SWE. I'd guess o3 can speed up ML engineering by 1.5x.
- "o4" (hypothetical future model) -- Can produce similar work to top ML engineers, at lower cost?

AI's are already really good. For instance [METR showed here](https://metr.org/blog/2024-11-22-evaluating-r-d-capabilities-of-llms/) that AI's like o1 are roughly comparable in skill to a human ML engineer on 8 hour long engineering tasks. Suppose that you 2x this horizon length every 6 months. Very soon, this gets very big. As AI's become more powerful, they can be useful in accelerating AI R&D.

People like to talk about how LLMs are unreliable, and hallucinate a lot. I hope such people will update based on o3. An similar objection that some people have to claims about ASI being possible is that "computers can't be intelligent", or "humans are special, so we'll always be able to do some things better than AI's".
- First, the brain is just a machine, and it was optimized by evolution, which is in some sense a similar algorithm to how we train artificial agents.
- Current artificial models are on pretty similar scales to brains
	- By which I mean, number of axons in a brain is only a few OOM larger than number of parameters in GPT4.
	- Is this a fair comparison? I'm not sure. 
	- Honestly I'd lean towards saying that computer neurons are better, because biology is messy and inefficient. 
- Some people say that machines will cap out at human level because machines just learn from humans. But this is not how ML works. We already have tons of examples of computers being superhuman in specific domains (e.g., chess, standardized tests).
- There are tons of tasks where you can generate feedback loops that create agents much stronger than humans.
	- For instance, LLMs could easily get much better than humans at next token prediction. 
	- Also, it's easy to create feedback loops for coding. 
- Having agents that utilize test-time compute to reason is the current paradigm for progress.

> [!tip] Claim 2
People will try really hard to make ASI.

> First, suppose there are no "small" AI catastrophes that scare people.

One major factor that will drive the development of ASI is that there is a ton of money to be made from AI. Companies/countries understand this and are investing huge amounts in AI. As an example of how much value could come from AI systems, imagine they could replace a large amount of the work-force (e.g., software engineers), or that they could make huge contributions to e.g., medicine.

But, this is not the only relevant factor driving growth. Some people are excited about building ASI because they think it's a really cool scientific project. Some people have a vision of a post-ASI utopia that they are excited about. 

> A "small disaster" might not be sufficient to stop people trying to develop ASI.

It's possible that the risks of AI don't become widely accepted until humanity has permanently been maneuvered off of the steering wheel of the future, but I don't think this will necessarily be the case. It's possible that people start to realize that AI systems we're building are dangerous, but still continue building them anyways. The main factor that would drive this is race dynamics / fear of being left behind. If your organization are the "good guys" and you're pretty sure that *someone* is going to develop ASI, then most people would feel that it's better if they are the ones in control of building it.

Also, things like humanity's response to COVID might make you pessimistic about humanity coordinating to stop AI development.  Also, regulation is generally **reactive** rather than **proactive**. But of course, you can't react after a sufficiently large disaster.

Another reason why it'd be really hard to stop AI dev, even if people had reservations, is that as we become reliant on powerful AI systems, it's harder to abandon them. For instance, imagine a policy of "everyone needs to get rid of their phones and computers". This is just unthinkable to most people at this point. We might already be at this point with tools like ChatGPT.

> There are reasons to suspect that there will not be large "small (misalignment) disasters" 

If an agent is smart enough to cause a disaster, then it is probably also smart enough to realize that it would be smarter to wait until it has a decisive advantage before initiating a "takeover" attempt. This issue is exacerbated by the fact that there will be tiny disasters, and then the agent will get feedback telling it not to make these disasters. And from this the agent could either learn not to create disasters, or to only create disasters large enough so that humans can't penalize it afterwords. You might hope that the AI generalizes and learns the first rule (which might seem like a simpler rule). 

In summary, there are a lot of clear signals that push for the development of AI, and the signals of the dangers are more nebulous, and might only come after the situation has already spiraled out of control.


> [!tip] Claim 3
> An ASI would be capable of killing all humans:

Before considering whether HCAI agents *would* kill/disempower humans, let's think about whether they *could* if, e.g., someone gave them this as a goal. Obviously someone giving an ASI such a goal counts as misuse not as the ASI independently posing a risk without bad actors -- but we'll get to why you might not need to have people explicitly give ASI such a goal in order for the agent to pursue this goal later.

> Q: What are some really dangerous things that AI could do? A:

- AlphaFold5 is given a lab and is researching e.g., how to cure cancer. AlphaFold5 figures out how to make a synthetic virus similar to COVID19 but much worse. It manufactures this and releases it.
- Various countries put AI agents in charge of their military -- maybe not because they actually thought that this was a good idea, but because they worried that if they didn't then other countries would, obsoleting their military.
- An AI can buys its own data-center, replicates onto that data-center, and then does R&D to improve itself. Then, after becoming extremely intelligent, it comes up with some strategy that we'd never think of for doing something dangerous.
- An AI uses persuasion, or makes a bunch of money online and then uses money, to get humans to do dangerous things.

Note that these aren't super far-fetched applications of AI. AI is already widely used in drug design, we are increasingly going to give agents the ability to act autonomously, to write and execute code, to give us news and help us make decisions.

The "disempower" case is also worth thinking about. One way this could play out is that we become highly reliant on AI systems, the world changes rapidly and becomes extremely complicated, so that we don't have any real hopes of understanding it anymore. Maybe we end up only being able to interface with the world through AI's and thereby lose our agency.
Paul Christiano does a good job of explaining this [here](https://www.alignmentforum.org/posts/HBxe6wdjxK239zajf/what-failure-looks-like).


> [!tip] Claim 4
An ASI might want to kill humans:

#todo
<p style="color:red">
Read [Alignment Faking](https://www.anthropic.com/research/alignment-faking) and then come back here. 
</p>

- suppose it [crystallizes](https://www.anthropic.com/research/alignment-faking) on some weird goals 
	- there's compelling theoretical and empirical reasons to believe that such crystallization might happen
		- see alignment faking paper -- deception can be selected for!
	- conditional on such crystallization, a large class of goals incentivize dis-empowering humans
		- e.g., goal preservation
	- key point -- we don't get to directly choose what the AI cares about, discussed more here: [[The Inner Alignment Problem]].

> [!tip] Responses to Common Objections

- re 1: why won't people stop building AI once they realize its super dangerous or once people get annoyed that they lost their jobs?
- see it as a race
- ppl just like AI
- economically and militarily valuable

- re 2: why won't energy / or money run out before we can scale more?
- doesn't matter. algorithmic improvements decrease cost over time. 
- also there's still plenty of room to scale for a bit.
- money not running out

- re 2: why won't models stop improving once they get to human level? 
- we can train them on super hard tasks
	- for instance, we can train them on tasks that'd take a human a long time to do
	- lots of tasks humans take a long time to do but it's easy to grade

- re 3: why couldn't we just turn off AI's if they got really scary / turned against us?
- ASI isn't dumb it's not gonna coup until it's sure it can win and prevent humans from countering. 

- re 4: why would an AI even "want" anything?
- they already do
- wanting is advantageous -- will be selected for . see alignment faking paper.

- re 4: don't we get to choose what the AI cares about?
- nope. see alignment faking or [[The Inner Alignment Problem]]

## What to do About it

**Step 1: Form your own opinion**
To start, I'd recommend reading some stuff and evaluating whether these people give reasonable arguments are not. It's also worth trying to find a good "debate" about AI  [Here's a good one](https://www.youtube.com/watch?v=144uOfr4SYA) [Here's another one](https://www.youtube.com/watch?v=6yQEA18C-XI). The thing that initially sold me on the case for AI risk before I thought about it myself is that I read/listened to conversations between concerned people and not concerned people and the not concerned people (e.g., Yan LeCunn) seemed to have terrible arguments.
Here are some links for some stuff that could be interesting to read / listen to, [an interview with Paul](https://www.youtube.com/watch?v=GyFkWb903aU), [Eliezer Yudkowsky TIME article](https://time.com/6266923/ai-eliezer-yudkowsky-open-letter-not-enough/),   [PauseAI Risk statement](https://pauseai.info/risks).
Then, write your own opinions.

**Step 2**: **Make some plans.** 
Some things that could be good to do about this:
- **policy work** -- see, e.g., https://emergingtechpolicy.org
- communication
- technical alignment work

Some things that would be really bad to do about this: 
- Contribute in any way to work on developing more capable AI systems
- Encourage race dynamics

If you're at all interested in doing something about this, or are skeptical but want to talk about it, or want to talk about how to emotionally cope with this, please please please reach out. It can be daunting to figure out what to do and what to believe. I can give you some connections and pipe you directly to some places where you can have an impact (especially if you are a college senior, or employed but willing to switch jobs).
you can reach me at
![[Pasted image 20241220175901.png]]

**My general thoughts on what should happen**
I highlighted 4 reasons that I could see for why catastrophe could be averted. One of these was basically "we just get lucky", and isn't super actionable. The other 3 correspond to interventions that we can take to increase the probability of a good outcome. Specifically, here's what I'd like:

- Technical research. 
	- There are four types of technical research that are valuable:
		- Evaluations -- eliciting and assessing model capabilities; important for informing policy / convincing people to stop.
		- Model organisms -- demonstrating dangerous behaviors in toy settings, to raise awareness and to study them.
		- Control -- figuring out how to get useful behavior out of potentially egregiously misaligned agents.
		- Alignment -- figure out how to train an AI to *want* to do good things.
	- If you have a technical background and don't want to go into policy, then you should consider switching careers. This issue is going to be really urgent for the next couple years, and we need good people to try and make things go well.
- Policy work to buy us time / eliminate racing.
	- As discussed [[What Should We Do About the xRisk Posed by ASI?|here]], solving alignment on a time crunch with race dynamics seems extremely risky. 
	- A pause on frontier AI progress would be very valuable. 
		- We need to pause until we have a good plan for why it's going to be safe to press forward.
	- A pause is possible -- training frontier AI models is extremely expensive and resource intensive, so we can just make sure that no one is using a ton of GPUs.
	- We also need policy work that says "if capabilities are like this, then it's unacceptable to deploy."
	- If we can't pause, at least creating an international group that works on AI rather than having private companies do this would be beneficial, because this also eliminates race dynamics and makes it possible to take better safety measures.

---

<iframe src="https://forms.gle/rKthxKXWahjdy4kV7" width="640" height="800" frameborder="0" marginheight="0" marginwidth="0">Loading…</iframe>`

[^1]: U.S.-China Economic and Security Review Commission. _2024 Report to Congress_. Washington, D.C., November 2024. Available at: [uscc.gov](https://www.uscc.gov/sites/default/files/2024-11/2024_Executive_Summary.pdf)