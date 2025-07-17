Suppose you want to lower the chance that a misaligned AI takes over the world. One natural approach is to deter AIs from "taking over AI labs", for instance by launching a [rogue internal deployment](https://redwoodresearch.substack.com/p/ai-catastrophes-and-rogue-deployments). But, an under-explored question is, "Are there tractable interventions that decrease $\Pr$("AI takes over the world" $\mid$  "AI takes over an AI lab")?"

> **This is a draft**. I'm interested in feedback. Message me if you want the gdoc to give comments on. Especially if you know anything about computer security (I am planning to learn about computer security and then rewrite the stuff about computer security). Also happy to chat the post!

> Note: Most of the ideas in this post are not original.
> Thanks to Buck for proposing that I think about this question, and chatting with me about it. 
> He does not necessarily endorse anything said here, and thinks that the stuff I've said about computer security is wrong.

In this post I will:
- Summarize four classic strategies for how an AI could take over the world.
- I'll decompose each of the takeover strategies into a "taking over the lab" part and a "after taking over the lab" part.
- I'll give a long list of ideas for blocking the "take over lab --> take over the world" part of the takeover stories.
- I'll conclude by highlighting what I think are the most leveraged interventions for small organizations (e.g., a small company or research group), or [AI lab employees](https://redwoodresearch.substack.com/p/ten-people-on-the-inside) to take for preventing AI's which have already "taken over the lab" from also taking over the world.
## Four classic takeover paths
Here are four paths to an AI taking over the world that warrant somewhat different mitigations.
1. "Humans hand over power willingly" (Kind of similar to the AI 2027 takeover story)
	- For a variety of reasons -- possibly including AI persuasion -- AI is broadly deployed in the military, the economy and/or scientific research.
	- Here are some ways that these could go poorly:
		- Military: AI makes a robot army for us and then turns it against us. (Note that "robot army" probably should include things like bombs, not just drones).
			- Also note that, at least initially, AI's advantage in the military is more likely to be: data analysis (e.g., look at tons of satellite images), strategy, and real time coordination of drones, rather than "making new weapons".
		- Economy: AI's make a ton of factories that make widgets and we have no idea what's going on. It turns out one that the factories had parts for making a robot army, which then is turned against us.
		- Science: The AI is curing all diseases. To do this, we are frequently running a bunch of experiments in a wet-lab for the AI. Turns out that some subset of these experiments were for making a powerful bioweapon. AI deploys this, wiping out most humans. AI keeps a couple loyal humans around to "shovel coal" and craft robotic hands for the AI.
2. "Covertly develop weapons" (Heavily inspired by @joshc's takeover story)
	- AI launches a rogue internal deployment, and also exfiltrates itself to get a rogue external deployment (possibly in collaboration with some other government).
	- The rogue AI hires some humans to run bio experiments for it, and then makes some bioweapons.
	- The rogue AI also secretly develops some weapons.
	- Note that the AI probably has to have acquired some amount of wealth and influence to build the weapons and bioweapons.
	- The AI also provokes the US and China into a war, to deplete the human's war resources.
	- To disrupt humanity's response to the AI's attacks, the AI: 
		- Spreads a bunch of misinformation on the internet.
		- Does a massive amount of cyberattacks / hacking.
			- Maybe it can hack to trick humans into attacking the wrong target.
			- Maybe it can sabotage human weapons.
			- Maybe just shutting down human comms would already be strong enough.
3. "Novel offensive technology" (Kind of similar to Yudkowsky's TED talk takeover story)
	- The AI does science --- either because we asked it to and gave it a bunch of resources, or because an AI escaped the lab and is doing this, or because the AI is able to do this in a rogue internal deployment --- and discovers a crazy new technology that we have no defense against. For instance, "nanobots".
4. Political takeover (thanks to Anshul for mentioning this strategy to me)
	- The AI convinces people to elect it, or a puppet, as president.
	- Or the AI convinces the US president to defer to it on all actions, effectively making the AI president. 
	- The AI could either try to pull this political maneuvering off in multiple countries, or escalate from controlling one country to controlling the world. (Which is more plausible if that country has a DSA over other countries).
## Different senses of "taking over a lab"
In the four scenarios above, there are several distinct senses in which a lab was "taken over" (most stories involve several of these):
- **Rogue internal deployments.** The AI exploits a security vulnerability (which was possibly also *inserted* by the AI) to gain access to unmonitored compute. 
- **Rogue external deployments.** The AI somehow exfiltrates itself (possibly with the help of employees, or by exploiting security vulnerabilities). 
- **Opaque actions**. The AI takes actions that we don't understand, but would think are bad if we understood. 
- **Opaque plans**. The AI has the ability to perform large amounts of effectively unmonitored reasoning / planning.
- **Persuasion**. The AI convinces relevant parties that it's actions aren't bad, or are "necessary".

(TODO: calling opaque actions + plans "lab takeover" is weird terminology)
## Which parts of takeover come after "taking over a lab"? 
Here are actions that AIs took *in addition* to "taking over the lab" (as defined in the previous section) in some of the above stories:
- Developing novel offensive technology (e.g., nanotech).
- Develop and deploy strong bioweapons.
	- Requires doing good bio research.
	- Requires working with humans while keeping them in the dark.
- Rogue external deployments require:
	- Not getting caught and shutdown.
	- Obtaining money.
	- Obtaining compute to run yourself on.
- Rogue internal deployments require:	
	- Not getting caught.
		- In particular, making sure that humans don't make substantial progress into making AI cognition transparent.
		- And somehow obfuscating logs / preventing humans from discovering the AI's secret use of compute. 
- Opaque actions require: 
	- High collusion ability (assuming we use untrusted monitoring).
- Hacking.
- Spreading misinformation to sow content.
- Military strategy.
- Commanding a robot army; in particular, preventing humans from turning the army off.
- If the AI is trained, it might be important that it retain its' goals.
	- So either the AI is incentivized to attack ASAP, 
	- Or to secretly "align its successor system to itself" but hide this from us.
- Having the people lead a political takeover may require being able to talk to the people.
## Preventing AIs from taking over the world, after they've already taken over the lab
Now, I'll give a long list of ideas for preventing an AI from taking over the world, given that they've already taken over a lab. Specifically, I'd focus on the case where the AI has taken over the *leading* lab, although this detail will mostly not be important. Most of the measures I'll described will help guard against several of the takeover stories that I presented earlier. Some of these measures might be quite undesirable in worlds where the AI is not misaligned.

For some of the ideas that I list, it's obvious how to implement the idea, and the main interventions that a small group can perform are:
- Popularize the idea. For instance:
	- Talk to friends at labs / in government about this idea, or publish a position paper.
- If you are in a lab or the government, directly implement these measures.
However, some of the ideas need more technical research on how to implement. Here the main intervention I propose is: 
- One person / a small group could do research and get SoTA ideas for how to do this.

Here are the ideas.

**Catching rogue deployments**
Here are thoughts on how you could catch a rogue internal deployment:
- Have really good **accounting** for all of your compute usage. 
- Save AI action logs so that it's really hard for the AI to tamper with them.
	- Probably you want a hardware based solution for securely saving logs.
		- The military, and some critical infrastructure has hardware that can implement a "unidirectional gateway" called [data diodes](https://en.wikipedia.org/wiki/Unidirectional_network) .
	- Maybe there are adequate software solutions though. For instance, if AI would take an action sequence $x_{1},\dots,x_n$ then you could force it to write down $(x_{1},h_{1}),(x_{2},h_{2})\dots$" where $h_i = \mathsf{hash}(x_{i-1}||h_{i-1})$ (and $h_{1}$ is random). This means that you can't after-the-fact modify the history without reproducing all the subsequent actions, which could be expensive.
- Periodically go over old action logs of the AI using AI's from other labs as untrusted monitors.

Here are thoughts on how you could catch a rogue external deployments:
- Have a bounty for turning in rogue AIs.
- Have KYC on GPU providers (like runpod).
- Have inspection of AI factories to make sure that they aren't secretly making weapons.

**Handling rogue deployments**
- Mickens et al. have proposed a variety of measures for what to do if you suspect a rogue internal deployment:
	- You can turn off power to the model (and turn off the backup generator), or turn off its internet connection.
	- Or, you can destroy the data-center via cutting cables, setting it on fire, flooding it, EMPs, or a kinetic strike.
	- Note that, for the less extreme measures here, work is required to actually make these be options. There are technical challenges, such as making the off-switch a one-way communication channel (we want to be able to remotely turn off the AI, but don't want people to remotely steal the AI).
- In the longer term, it'd be nice is there was an easy way to restore your hardware to a non-compromised state after the AI took over. 

**D/ACC**
Defensive technologies could make it much harder for an AI to successfully takeover. Vitalik gives some idea [here](https://vitalik.eth.limo/general/2025/07/10/2027.html) about what technologies could help defend against bio, cyber and persuasion threats. Someone needs to execute on these ideas. Plausibly a small company could:
- Install UV lights at airports.
- Make good PPE that is feasible for people to ~always wear.
- Make it cheap and easy to harden your house and workplace against bio threats.

**Protecting against manipulation / misinformation**
- If an AI has super-persuasion capabilities, then great care is needed in interacting with it.
- If you want to stop this AI from gaining influence, you could not let this AI talk to the general public.
- If there are some people that must talk to this AI, then you could require that people only talk to the AI in groups of at least two people, where the AI only knows one of the people present. 
- You could also tell humans that they should adopt a policy of ignoring threats from AIs.
- You can perform stings on humans for if they are susceptible to AI manipulation, and not allow humans that are susceptible to manipulation to talk with the AIs.
- To prevent AI from hiring / pressuring / black-mailing humans into helping it you could:
	- Have a bounty for ratting out rogue AI's.
	- Use AI to find human groups that might help rogue AIs, and then perform sting operations these human groups. Monitor humans that are willing to help rogue AIs closely.
- To make it harder for AI to impersonate specific people, have humans use specific details of past encounters to verify their identity.
- Humans should have some communication methods which aren't easy to sabotage with cyber attacks. Probably the military has this by default, but AI companies should have this too.
- Improve human epistemics so that AI has harder time tricking us into thinking that alignment is solved if its not.

**Restricting affordances**
- There seems to be somewhat broad support for banning the use of AI in the military. Making this happen would make it harder for an AI to takeover by being in charge of the military.
	- A related measure is to not put AI's in charge of nuclear codes (or talk to humans in charge of nuclear codes), which would be desirable if the AI is misaligned.
- AI's doing certain science research could be siloed, both physically and in terms of information.
	- It could be useful for a small company to develop methods for doing extremely siloed science research.

**Making deals**
- The classic "making deals with AI" approach is to promise an AI resources if it admits to being misaligned and admits it.
- A different approach is a "[dead hand](https://en.wikipedia.org/wiki/Dead_Hand)" approach:
	- For instance, you could set up the datacenter to self-destruct (or go offline) unless a human gives a certain password every week. It's probably best if the human in charge of this doesn't have contact with AI.

**Ideas for a multipolar world**
- It is possible that AI's from different labs won't collude with each other, even if an AI would collude with collude of itself. This seems highly likely if one of the labs has developed an "aligned" AI. This creates lots of opportunities. 
- Suppose a lagging AI lab has aligned AI and is 2 months behind (in terms of capabilities).
	- Then, you can have the aligned AI look over 3 month old actions / plans from the misaligned AI, to try and find evidence of the AI launching a rogue internal deployment, or doing other sketchy things.
- You could also tell AI's to report if they are contacted by another lab's AI asking to team up and takeover. You could develop honey pots for this.
## What are the most leveraged interventions?
- Develop methods for **detecting** rogue deployments.
	- The most promising ideas I've had here are:
		- Have a hardware mechanism for reliably storing actions from AI's, so that an AI that launches a rogue internal deployment can't just "wipe the traces of its bad actions".
		- Use humans, and AI's from multiple different labs to look for traces of the AI launching a rogue deployment, especially in the past (when they were less capable). 
- Develop plans for what to do once you've caught egregious AI misbehavior.
	- For instance, plans for shutting down rogue internal/external deployments (e.g., an off-witch, KYC for GPU services).
	- Establish something similar to the Moscow-Washington nuclear hotline, but for AI.
- Bio d/acc: harden the world against bio threats.

## Conclusion
It seems like there are some tractable interventions here that can avert some catastrophes, which might not happen by default. Worlds where AI has already taken over a lab are quite scary but, these measures may: 
- Buy humanity more time to figure stuff out, especially in slow take-off worlds where there are no low-hanging novel offense dominant technologies.
- Give humanity a chance to *react to* instead of *prepare for* threats from advanced AI.
