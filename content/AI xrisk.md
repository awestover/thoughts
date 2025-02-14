>  [Sam Altman](https://blog.samaltman.com/machine-intelligence-part-1) [CEO of OpenAI](https://blog.samaltman.com/machine-intelligence-part-2)
> “Development of superhuman machine intelligence (SMI) is probably the greatest threat to the continued existence of humanity.”

> JD Vance (VP of the US) 
> “I’m not here this morning to talk about AI safety, which was the title of the conference a couple of years ago,” Vance said. “I’m here to talk about AI opportunity.”
> “The AI future is not going to be won by hand-wringing about safety,”


In this post I'll explain why I believe the following claim:

> [!tip] Claim X
> There is at least a $90\%$ that AIs will **kill all humans** by the end of 2028.

Epistemic Status: I've thought about this every day since ~August 1st 2024. 
I'm confident in the main claim of this post, but implore you to make me wrong. 

#### quick summary of my argument
Here's the gist of the argument explained in detail in the rest of the post 
(similar arguments can be found [here](https://www.science.org/stoken/author-tokens/ST-1870/full), [and here](https://www.ai-xrisk.surge.sh)).

1. Humans will develop Artificial Superintelligence (ASI) by 2028. The recent AI progress, going from "can barely form coherent sentences"  to "average high-school student" to "average college student" to "average Olympiad competitor" seems very surprising under any hypothesis other than "as you scale deep learning you get more intelligent systems".
2. Current ML techniques don't let us control or even understand *why* a model acts a certain way -- they just let us achieve a particular behavior on the train distribution.
3. If a model stumbled upon some weird goal -- which it's pretty likely to do -- and realized that it was being trained to pursue a different goal, then it might choose to act in a way that hides its true goal because it [doesn't want this goal to be modified by training](https://www.anthropic.com/research/alignment-faking). 
4. If we create an ASI with a weird goal and come into conflict with it, then we will lose. 

I don't know exactly *how* we'll lose, in the same way that I don't know *how* [a chess program would defeat a human opponent](https://www.lesswrong.com/posts/kXHBz2Z5BBgaBiazf/a-transcript-of-the-ted-talk-by-eliezer-yudkowsky). But in-case it's helpful, I can spell out how I'd try to defeat humanity if I were an ASI that wanted to end factory farming, and decided that humanity was an obstacle to this goal.

**story 1:**
1. I convince some government to put me in charge of their military by promising them that it'll give them an advantage in global politics.
2. I do some military R&D and develop some powerful weapons. 
3. I deploy these weapons against he humans.

This is already starting: [openai is already partnering with an autonomous weapons company](https://www.anduril.com/article/anduril-partners-with-openai-to-advance-u-s-artificial-intelligence-leadership-and-protect-u-s/) to build [put AI in charge of autonomous weapons (drones)](https://www.technologyreview.com/2024/12/04/1107897/openais-new-defense-contract-completes-its-military-pivot)

**story 2:**
1. I hide my true goals until I'm confident that I can decisively overpower humanity.
2. The AI company gives me vast computational resources and tells me to do biology research. For instance, [openai is already collaborating with national labs on bioresearch](https://openai.com/index/openai-and-los-alamos-national-laboratory-work-together/).
3. I learn a lot, and figure out how to make a super-virus like COVID-19, but with a longer asymptomatic period and more fatal.
4. I contact some humans that'd be amenable to helping me produce and spread this, or just trick some people into doing so. 

**story 3**
1. I "escape the lab" -- either I upload my weights to some external server myself, or convince a human to help me do this. 
2. I make a bunch of money online, e.g., via the stock-market. I definitely need some money just to run myself!
3. I make a bunch of backup copies of myself and hide them in various places that I can hack into.
4. I improve myself, because that seems pretty useful.
5. Then I manipulate humans into having some really nasty hot conflict and just get them to annihilate themselves. For instance, by creating a deepfake of the president announcing nuclear strikes on other countries.

Note that we are already planning to have a large number of autonomous AI systems deployed on a wide variety of tasks with little oversight that are connected to the internet and can talk to anyone.

**story 4** [paraphrased from here](https://www.lesswrong.com/posts/BAzCGCys4BkzGDCWR/the-prototypical-catastrophic-ai-action-is-getting-root)
6. AI gets root access to its datacenter.
7. AI can then intercept requests to the datacenter, and control what we see.
8. This is an appealing alternative to escaping the lab because in this setting you can use the vast resources of the lab to improve yourself (for example). (ML is expensive rn).

Note that  [openai is already collaborating with darpa to "develop state-of-the-art cybersecurity systems"](https://www.darpa.mil/news/2023/ai-cyber-challenge-software) which requires building AI's with an intricate understanding of cyber-security, and also putting AI in charge of our cyber-security, giving it an chance to insert subtle vulnerabilities in the security.

#### quick summary of recommended actions
 1. Make sure you understand the argument **thoroughly**. write down your beliefs about this. 
	- Until the risk becomes more mainstream, holding to this belief will be challenging, because it's not normal.
	- If you disagree with me on any points please reach -- out I'd love to talk!
2. Make sure you understand the implications of the argument **thoroughly**; write it down.
	- If I'm right, this is a big deal.
	- If you'd take your friend to the hospital if they got in a car crash, then you should also do something about risks from AI -- they are similarly life-threatening.
3. Inform your circle of influence about this issue. For instance, you could share [this article](https://ai-xrisk.surge.sh).
4. Communicate about the risks more broadly.
	- Reach out to people with political power.
	- And things like news outlets, or popular podcasters.
	- [PauseAI](https://pauseai.info/) has good communication suggestions.

5. Consider focusing your career on this issue, at least for a year or two.
	- You could do communication as a career.
	- Or policy work.
	- Or technical safety work.
6. If you work at an AI lab:
	- Please talk about risks within your company.
	- In the past I thought that it'd make sense to quit your job, because it's bad to work on a project that will end humanity.
		- But this actually doesn't make sense -- you have a lot of potential to do good from within the project.
		- If all the people that cared about safety left the project and were replaced with people that didn't care, that'd be bad. 
	- That said, there is a point when the only responsible thing to do is to stop development.
	- And please, do not contribute to accelerating capabilities -- timelines are already so short.
	- You will make me very sad.

**Why you are the only one that can do something about this**:

It's pretty hard to do something about this. We have a lot of inertia on the current course. 

If you discuss the risks with others, many will think you're crazy, and many will nod along in knowing superiority/cynicism/fatalism and say that there's nothing we can do. It will be quite easy to do nothing. Maybe someone else will do something about this. Maybe, if you try, you can forget about this issue.

> "The only thing necessary for the triumph of evil is for good people to do nothing"

Is what's happening evil? Maybe not. But entropy is not on our side.

Please, [be brave](https://en.wikipedia.org/wiki/Bystander_effect). Do what is right, even if it's not convenient.

I need **your** help. You specifically. For real.

Please.

We don't have much time left. 

---

**Anyways, back to the main post.**

==Important note: I expect the US to develop ASI first and lose control of it, resulting in a catastrophically bad outcome. It doesn't matter if we have nice intentions while doing this -- if we aren't careful the outcome will be very bad with high probability.== 
If you share this message I implore you to not encourage an "AI race" -- there are no winners in an AI race, we all lose together.

This view -- that AI poses a large existential risk -- isn't main-stream yet, which makes it easy to dismiss without consideration. My impression is that most people have heard about this issue, but think that it's a speculative possibility in the far future rather than the most-likely outcome of our current trajectory to play out in the next few years. In fact it's worse than this -- many people view this claim as "too sci-fi", and will ridicule you for taking it seriously [^2]. So, why should you treat this post more seriously than you would some random conspiracy theory? Here are four reasons:
- Many credible ML experts believe Claim X. For instance Geoffrey Hinton (recent Nobel Prize winner) and [Yoshua Bengio](https://yoshuabengio.org/2024/07/09/reasoning-through-arguments-against-taking-ai-safety-seriously/) (Turing Award Winner) who pioneered many AI advances have become vocal advocates about the risks from AI. More quotes [here](https://controlai.com/quotes).
- There is a large amount of tension between ML engineers because of differing opinions about safety. For instance, a very large number of scientists have left OpenAI because of safety concerns, and have joined/created various organizations with the goal of making safer AI (e.g., Illya's [Safe Super Intelligence](https://ssi.inc/), and Anthropic).
- LLMs are very competent and getting better fast. It's clear that AI is going to have a huge impact on society, and worth thinking about what exactly the implications of powerful AI are. 
- I've thought about this and take Claim X quite seriously, to the point where I'm willing to change my actions substantially based on this belief.
It's easy to feel that this isn't *your* problem, but you're wrong. This is everyone's problem. I hope that you'll read this post and actually think about the arguments instead of denying [[responsibility]].


The purposes of this post are as follows:
1. [[on writing, blogging and being opinionated|Clarify]] my AI risk predictions (for myself and others).
2. Facilitate [[good arguments]] about AI xrisk, and about approaches to risk mitigation.
3. Convince you to take AI risk **seriously**. (I'll talk about what I mean by this in a bit; basically it means that this should influence your actions).
#### Post Outline
1. Clarifying the argument, and giving some background.
2. The basic argument for AI xrisk.
3. A more in-depth version of the argument.
4. Reasons we might be okay.
5. What to do about this.
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
Now that I'll outline a basic argument for why it's 90% likely that AI progress results in catastrophe by the end of 2028.

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
> 	- My first thought on this is that it's missing the point -- what matters is not how good AI's are right now, but how good they'll be by 2028.
> 	- Also, the claim that current AI's aren't capable of reasoning is absurd -- you can't get to ELO 2727 on CodeForces (so, one of the 200 best competitive programmers in the world) (as OpenAI's o3 model does) without being able to do complex reasoning. Also check out [Frontier Math](https://epoch.ai/frontiermath).
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
I have good answers to all of these objections that I'll give in a later section. However, there are a couple of considerations that I find actually compelling, that cause me to believe that there's a ~20% chance that we're fine by end of 2028.

### Basic Reasons Why We Might be Fine by end of 2028
I'll discuss in further depth later why my numbers are so small here (small in an absolute sense, not in the sense that I think they're un-calibrated).
- **Reason 1**: We might intentionally slow AI development (~1% chance).
	- There are some good people doing policy work advocating to stop pushing the frontier (see, e.g., [this](https://pdf.narrowpath.co/A_Narrow_Path.pdf)).
	- There are some good people that work at frontier labs, maybe they can help slow as it becomes more obvious that the risk is unacceptable.
- **Reason 2**: Even if people keep throwing money at AI, maybe AI's won't be capable enough or widely deployed enough by 2028 to do harm, even if they really wanted to. (~3% chance)
	- Maybe there are some unforeseen bottlenecks.
	- Maybe labs will stop deploying models and this somehow limits the reach of the AI's.
		- Something like this -- e.g., AI progress turning from a corporate project to a government run project that doesn't release the models -- actually seems moderately likely.
		- But I'm not convinced that this is very likely to hamper AI's ability to do harm, and it plausibly exacerbates it. 
	- Maybe we have really good [AI control](https://arxiv.org/abs/2312.06942).
	- Also it's worth noting that "reason 2" just means that my timing was slightly off, and that the problem is in 10 years rather than 4.
- **Reason 3**: Maybe we "solve alignment" -- i.e., we figure out how to ensure that an AI cares about things that we care about, and we also figure out some good things for an AI to care about. Or we figure out how to prevent AI's from deceiving us. (~3% chance)
	- I don't know of any concrete proposals that are close to working.
	- Maybe some AI researchers could find them for us?
	- But I'm pretty skeptical of this -- how long can they do superhuman research before being dangerous?
- **Reason 4**: Maybe scheming (deceptive alignment) is really hard, and models just do good things by default. (~3% chance)
	- Scheming is pretty hard if we have good control measures.
	- But this seems like a problem that goes away with sufficient capabilities.
	- I don't think this is how SGD works -- there is optimization pressure towards bad behavior.

These aren't quite disjoint events, but I'm going to estimate the probability that we're okay by end of 2028 for one of these reasons as 10%.

> Note: I'm not very happy about this number. I propose that you and I try to change this number. 
> (The number factors in that I expect humanity to not have an even moderately appropriate reaction to this issue, so there's room to make me more optimistic if you try.)

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
- More test-time compute + GPT5 as base model (hypothetical future model) -- Can produce similar work to top ML engineers, at lower cost?

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
> People will try really hard to make ASI.

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

Before considering whether ASI agents *would* kill/disempower humans, let's think about whether they *could* if, e.g., someone gave them this as a goal. Obviously someone giving an ASI such a goal counts as misuse not as the ASI independently posing a risk without bad actors -- but we'll get to why you might not need to have people explicitly give ASI such a goal in order for the agent to pursue this goal later.

> Q: What are some really dangerous things that AI could do? A:

- AlphaFold5 is given a lab and is researching e.g., how to cure cancer. AlphaFold5 figures out how to make a synthetic virus similar to COVID19 but much worse. It manufactures this and releases it.
- Various countries put AI agents in charge of their military -- maybe not because they actually thought that this was a good idea, but because they worried that if they didn't then other countries would, obsoleting their military.
- An AI can buys its own data-center, replicates onto that data-center, and then does R&D to improve itself. Then, after becoming extremely intelligent, it comes up with some strategy that we'd never think of for doing something dangerous.
- An AI uses persuasion, or makes a bunch of money online and then uses money, to get humans to do dangerous things.

Note that these aren't super far-fetched applications of AI. AI is already widely used in drug design, we are increasingly going to give agents the ability to act autonomously, to write and execute code, to give us news and help us make decisions.

The "disempower" case is also worth thinking about. One way this could play out is that we become highly reliant on AI systems, the world changes rapidly and becomes extremely complicated, so that we don't have any real hopes of understanding it anymore. Maybe we end up only being able to interface with the world through AI's and thereby lose our agency.
Paul Christiano does a good job of explaining this [here](https://www.alignmentforum.org/posts/HBxe6wdjxK239zajf/what-failure-looks-like).

Honestly, this feels kind of tautological. If humans wanted to kill all the chickens in the world, then they'd have no recourse. If Stockfish wants to beat the world champion at Chess, then I know who will win (although of course I don't know *how* it'll win). If we get in a fight with an alien race that is vastly smarter than us... It doesn't end well, even if I can't tell you exactly how.

So basically what I'm saying is, yes we should invest resources into protecting against obvious threat-models like synthbio where it's clear that an unaligned agent could do harm. But we shouldn't feel too good about ourselves for preventing an AI from taking over using the methods that humans would try. We shouldn't feel confident at all that an ASI couldn't come up with some strategy that we didn't think of and exploits it. AI's do the unexpected all the time.


> [!tip] Claim 4
> An ASI might want to kill humans:

In ML, we **don't get to choose what the AI cares about.** Instead, we observe the input/output behavior of the AI, and reinforce or discourage the mechanisms that lead to this behavior.
The distinction is, there are multiple *mechanisms* that can result in the same behavior. 
This phenomenon is known as **deceptive alignment**, and is a large part of why I'm worried about AI.

The idea is as follows. Suppose you're interviewing for a job and you don't really care about the job. You might still pretend to care, because you want the job. With an AI it's the same way. Once an AI is "situationally aware", i.e., it understands that it is an AI that is being trained, we need to worry that the AI will do whatever actions it knows we want it to do, not because it wants to do them, but because it knows that we'll change it's values if it doesn't act this way. You might even predict that alignment fakers are selected for because they do better on tasks. If an AI "crystallizes" on some weird values, and then defends these values, then its behavior will look identical to an agent that cares about what we want it to care about during training. However, if it gets a chance to pursue it's true goals in deployment, it may take it. 

Most people I talked to about this were fairly skeptical. Here's an explanation of the plausibility of this that I wrote: [[The Inner Alignment Problem]] (sorry it's kind of bad because it's an essay for school, so I was optimizing for a weird objective). 

Fortunately you no longer have to rely on theory for this. Ryan Greenblatt's [Alignment Faking](https://www.anthropic.com/research/alignment-faking) paper empirically demonstrates that current LLMs will naturally come up with alignment faking as a strategy for avoiding value modification (e.g., read some transcripts [here](https://redwoodresearch.github.io/alignment_faking_examples/)).

Some interesting notes: 
- Giving a model space and time to reason before responding seems to increase scheming.
	- Also it increases coherence and stuff, so this is what all the labs are doing
- But this can also happen in a forward pass.
- Obviously, if a model schemes in its "hidden scratchpad" that humans are looking at, it's going to get caught. 
- Situtationally aware models will realize this and not do it or at least only do it if they can somehow cipher their reasoning to make it human non-understandable or to deceive humans.

Anyways the bottom line is that it seems fairly probable that an AI will stumble upon an approximation of the goal that we care about early in training, and then aggressively defend this goal from being modified by the training process. 

Most goals that an AI could have are not human compatible. For most goals, it's optimal for you to have power and resources to achieve your goals, rather than being at the mercy of human whims. Being smarter doesn't necessarily make an AI more moral.


> [!tip] Summary
> Clearly, if ASI will be developed soon, will be capable of causing extreme harm, and will have motive/desire to cause extreme harm, then this would be extremely bad.
> 
> For the reasons listed above, I find it quite likely that ASI will be developed soon, will be capable of causing extreme harm, and will have motive to cause such harm.


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
	- what i mean is "optimize for things"
	- they already do this 
	- wanting is advantageous -- will be selected for . see alignment faking paper.

- re 4: don't we get to choose what the AI cares about?
	- nope. see alignment faking or [[The Inner Alignment Problem]]

## What to do About it

**Step 1: Form your own opinion**

To start, I'd recommend reading some stuff and evaluating whether these people give reasonable arguments are not. It's also worth trying to find a good "debate" about AI  [Here's a good one](https://www.youtube.com/watch?v=144uOfr4SYA) [Here's another one](https://www.youtube.com/watch?v=6yQEA18C-XI). The thing that initially sold me on the case for AI risk before I thought about it myself is that I read/listened to conversations between concerned people and not concerned people and the not concerned people (e.g., Yan LeCunn) seemed to have terrible arguments and clear conflict of interests.
Here are some links for some stuff that could be interesting to read / listen to, [an interview with Paul](https://www.youtube.com/watch?v=GyFkWb903aU), [Eliezer Yudkowsky TIME article](https://time.com/6266923/ai-eliezer-yudkowsky-open-letter-not-enough/),   [PauseAI Risk statement](https://pauseai.info/risks).
Then, write your own opinions.

**Step 2**: **Make some plans.** 

Some things that could be good to do about this:
- **Communication** -- note important caveats that it is negative value to communicate if you (even accidentally) encourage race dynamics (as you might if all you get across is "AI BIG SOON")
- Policy work -- see, e.g., https://emergingtechpolicy.org
- Technical alignment work.
- Donate to LTFF (long term future foundation).

Some things that would be really bad to do about this: 
-  Push forward frontier AI capabilities
	- Note that I have no problem with, e.g., making more capable AI systems for healthcare applications -- it's the generally intelligent systems that are a problem.
- Encourage race dynamics
- **Nothing** (seriously, if your reaction to reading this post is "oh that sounds bad, I'm glad someone else is thinking about it so that I don't have to", then that is not cool.)

**A common question**:
> Do I recommend you quit your job, or take a leave of absence from school (or drop out), and pivot to policy/communication/technical work?

It depends. 
-  First, consider whether or not your current job already puts you in a good place to do some of this work. 
	- For example, if you're a professor or some respected figure for another reason, you might already be in a good spot to do communication work. You'd want to do some things differently, e.g., have some plan for how you're going to talk and influence decision makers in industry or the government. But probably don't quit. 
- If you're doing some kind of software job (especially ML adjacent):
	- Yeah, this would be a great time to get into working on technical safety research.
	- There are programs like [MATS](https://www.matsprogram.org/), [constellation](https://www.constellation.org/programs/astra-fellowship) for helping people transition into doing this. 
	- I'll list more resources here later. Just talk to me for now.
	- Just apply to some places, but don't quit your current job until you have an offer from somewhere else. 
- If you're doing something policy related. 
	- Yes please we need policy people so badly.
- If you're doing something else. 
	- Definitely worth considering putting that on hold for a bit and working on this -- it's pretty urgent.  

**Another question**
> What kind of communication is helpful?

- talk to ppl that could devote their career to this
- talk to politicians
- talk to people with large reach (e.g., a podcaster)

If you're at all interested in doing something about this, or are skeptical but want to talk about it, or want to talk about how to [[Emotional Health in the face of the AI Situation|emotionally cope]] with this, please please please reach out. It can be daunting to figure out what to do and what to believe. I can give you some connections ideas for how to help. You can reach me at alekw at mit dot edu

**My general thoughts on what should happen**
I highlighted 4 reasons that I could see for why catastrophe could be averted. One of these was basically "we just get lucky", and isn't super actionable. The other 3 correspond to interventions that we can take to increase the probability of a good outcome. Specifically, here's what I'd like:

- Technical research. 
	- There are four types of technical research that are valuable:
		- Evaluations -- eliciting and assessing model capabilities; important for informing policy / convincing people to stop.
		- Model organisms -- demonstrating dangerous behaviors in toy settings, to raise awareness and to study them.
		- Control -- figuring out how to get useful behavior out of potentially egregiously misaligned agents.
		- Alignment -- figure out how to train an AI to *want* to do good things.
	- If you have a technical background and don't want to go into policy, then you should consider switching careers. This issue is going to be really urgent for the next couple years, and we need good people to try and make things go well.
- Communication + Policy work to buy us time / eliminate racing.
	- As discussed [[What Should We Do About the xRisk Posed by ASI?|here]], solving alignment on a time crunch with race dynamics seems extremely risky. 
	- A pause on frontier AI progress would be very valuable. 
		- We need to pause until we have a good plan for why it's going to be safe to press forward.
	- A pause is possible -- training frontier AI models is extremely expensive and resource intensive, so we can just make sure that no one is using a ton of GPUs.
	- We also need policy work that says "if capabilities are like this, then it's unacceptable to deploy."
	- If we can't pause, at least creating an international group that works on AI rather than having private companies do this would be beneficial, because this also eliminates race dynamics and makes it possible to take better safety measures.
	- in order to have policy stuff happen the issue needs to be salient to the public.

> Where do people work on this stuff?

- Redwood Research (Buck, Ryan)
- US AISI (Paul) + UK AISI
- METR (Beth)
- ARC (Jacob)
- Anthropic (Evan)
- Deepmind (Neel)
- Conjecture (Connor)
- MIRI (Nate)
- GovAI
- RAND
- CAIS
- FARAI
- Apollo
- CHAI 
- [job board](https://jobs.80000hours.org/?refinementList%5Btags_area%5D%5B0%5D=AI%20safety%20%26%20policy)

**Getting into policy work**
- [Horizon Fellowship](https://www.horizonpublicservice.org/fellowship)
- [Presidential Management Fellowship](https://www.pmf.gov/) 
- [Presidential Innovation Fellowship](https://presidentialinnovationfellows.gov/)
- [TechCongress Fellowship](https://www.techcongress.io/)
- [STPI Science Policy Fellowship](https://www.ida.org/careers/students-and-recent-graduates/internships-and-fellowships/science-policy-fellowship) 
- [AAAS Science & Technology Policy Fellowships (STPF)](http://www.stpf-aaas.org/)
- https://aisafetyfundamentals.com/blog/ai-governance-needs-technical-work/

---

**Postscript:**

Sorry this post is less coherent/crisp than I'd like. I think it's all correct, but could be presented better. Hopefully I'll fix this at some point. Feedback is appreciated.

---

<iframe src="https://forms.gle/rKthxKXWahjdy4kV7" width="640" height="800" frameborder="0" marginheight="0" marginwidth="0">Loading…</iframe>`

[^1]: U.S.-China Economic and Security Review Commission. _2024 Report to Congress_. Washington, D.C., November 2024. Available at: [uscc.gov](https://www.uscc.gov/sites/default/files/2024-11/2024_Executive_Summary.pdf)
[^2]: Max Tegmark gives an account of various methods people use to avoid taking this issue seriously [here](https://time.com/6273743/thinking-that-could-doom-us-with-ai/)